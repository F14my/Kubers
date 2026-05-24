# Отличия dev и prod

В рамках лабораторной работы подготовлены два Kustomize-overlay: `dev` и `prod`.

## Основные отличия

1. **Количество реплик**:
   - `dev`: 1 реплика для всех сервисов.
   - `prod`: 2 реплики для `frontend`, `bff`, `user-service`, `message-service`.

2. **Выделяемые ресурсы**:
   - `dev`: базовые (requests: 100m CPU / 128Mi RAM; limits: 200m CPU / 256Mi RAM).
   - `prod`: повышенные (requests: 200m CPU / 256Mi RAM; limits: 500m CPU / 512Mi RAM).

3. **Node Affinity**:
   - `dev`: без строгих ограничений по узлам.
   - `prod`:
     - Системные сервисы (`postgres`, `minio`): строго на узлах `workload=system`.
     - Прикладные сервисы (`frontend`, `bff`, `user-service`): строго на узлах `workload=app`.
     - `message-service`: строго на `workload=app`, с предпочтением (preferred) узлов с меткой `disk=fast`.

4. **Теги образов**:
   - `dev`: используется тег `latest`.
   - `prod`: используется фиксированный тег `v1.0.0`.

5. **Изоляция окружений (Namespace)**:
   - `dev`: разворачивается в неймспейсе `messager-dev`.
   - `prod`: разворачивается в неймспейсе `messager-prod`.
