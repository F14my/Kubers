# Lab K8s Messager

Микросервисное приложение мессенджера, развернутое в Kubernetes.

## Структура репозитория

- k8s/base/: Базовые манифесты Kubernetes.
- k8s/overlays/: Конфигурации для разных окружений (dev, prod).
- argocd/: Манифесты для деплоя через Argo CD.
- docs/: Подробная документация и финальный отчёт.

## Быстрый запуск

### 1. Подготовка локально (через kustomize)

```bash
# Просмотр манифестов для dev
kubectl kustomize k8s/overlays/dev

# Деплой dev окружения
kubectl apply -k k8s/overlays/dev
```

### 2. Деплой через Argo CD

Примените манифесты из папки argocd:

```bash
kubectl apply -f argocd/dev-application.yaml
kubectl apply -f argocd/prod-application.yaml
```

## Документация

- [Финальный отчёт](./docs/FINAL_REPORT.md) — основные результаты и подтверждение выполнения.
- [Отличия Dev и Prod](./docs/dev-prod-differences.md) — детальное описание Kustomize overlays.
- [Архитектура](./docs/01-architecture-and-resources.md) — описание компонентов системы.


