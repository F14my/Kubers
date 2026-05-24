# Отчёт по лабораторной работе: Запуск микросервисного приложения в Kubernetes

## 1. Выполненные задачи

В ходе работы была реализована микросервисная архитектура мессенджера в кластере Kubernetes с использованием GitOps-подхода.

- **Развернуты компоненты**: frontend, bff, user-service, message-service, postgres, minio.
- **База данных**: настроены автоматические миграции через Kubernetes Jobs для user-service и message-service.
- **S3 CSI**: настроено монтирование S3-бакета через CSI-драйвер для message-service.
- **Kustomize**: подготовлены base конфигурация и два overlay (dev, prod).
- **Node Affinity**: реализовано разделение нагрузки между системными и прикладными узлами.
- **GitOps**: настроен Argo CD для автоматического деплоя из репозитория.

## 2. Реализация Kustomize (Dev vs Prod)

Подробное описание отличий приведено в [docs/dev-prod-differences.md](./dev-prod-differences.md).

- **Dev**: используется для быстрой отладки, минимальные ресурсы, 1 реплика, теги latest.
- **Prod**: повышенная отказоустойчивость (2 реплики), строгие лимиты ресурсов, закрепление за конкретными пулами узлов (nodeAffinity), фиксированные версии образов (v1.0.0).

## 3. Node Affinity (Production)

Согласно требованиям, в prod окружении настроены следующие правила:
- postgres, minio -> workload=system
- frontend, bff, user-service, message-service -> workload=app
- message-service -> дополнительное предпочтение disk=fast (Weight: 100).

## 4. Хранение данных (S3 CSI)

Для сервиса сообщений реализовано внешнее хранение файлов:
- Используется ch.ctrox.csi.s3-driver.
- Созданы Secret, PersistentVolume (PV) и PersistentVolumeClaim (PVC).
- Бакет messager-uploads монтируется в /uploads контейнера message-service.

## 5. Argo CD

Настроены два приложения:
1. messager-dev: отслеживает путь k8s/overlays/dev.
2. messager-prod: отслеживает путь k8s/overlays/prod.
Включена автосинхронизация (Prune, SelfHeal).
