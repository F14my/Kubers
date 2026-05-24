# Lab K8s Messager

Микросервисное приложение мессенджера, демонстрирующее работу в Kubernetes с использованием Kustomize, Argo CD, S3 CSI и других облачных технологий.

## Архитектура

Приложение состоит из следующих сервисов:

- **Frontend**: Nginx + SPA (JS), точка входа для пользователя.
- **BFF (Backend For Frontend)**: Агрегатор API на Go, взаимодействует с внутренними сервисами.
- **User Service**: Управление пользователями и аутентификацией.
- **Message Service**: Управление сообщениями и загрузкой файлов.
- **PostgreSQL**: Общая база данных (разделена на логические БД `messager_users` и `messager_messages`).

---

## Развертывание в Kubernetes

Манифесты подготовлены для использования с **Kustomize**.

### 1. Локальное развертывание (через Kustomize)

```bash
# Создание namespace (если не создан)
kubectl apply -f k8s/base/namespace.yaml

# Деплой dev-окружения
kubectl apply -k k8s/overlays/dev
```

### 2. Деплой через Argo CD

В репозитории подготовлены `Application` манифесты:

```bash
kubectl apply -f argocd/dev-application.yaml
# или для prod
kubectl apply -f argocd/prod-application.yaml
```

### 3. Доступ к приложению в K8s


```bash
# Проброс порта для Frontend
kubectl port-forward svc/frontend-dev 8080:80 -n messager-dev
```
После этого приложение будет доступно по адресу [http://localhost:8080](http://localhost:8080).


## Полезные ссылки

- [Финальный отчёт](./docs/FINAL_REPORT.md) — подтверждение выполнения всех задач.
- [Инструкция по S3 CSI](./docs/03-s3-csi.md) — настройка внешнего хранилища.
- [Описание Kustomize Overlays](./docs/dev-prod-differences.md) — разница между Dev и Prod.
