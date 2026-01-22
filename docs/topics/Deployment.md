# Развертывание (Deployment)

Процесс доставки кода на серверы (CI/CD) построен на базе **Bitbucket Pipelines**.

## Инфраструктура (Yandex Cloud)

*   **Production Environment:** Виртуальные машины (VM) в Yandex Cloud.
*   **Database:** Yandex Managed PostgreSQL (кластер).
*   **Container Registry:** Yandex Container Registry (для хранения Docker-образов).

## Пайплайн (Pipeline Overview)

Процесс деплоя автоматизирован в `bitbucket-pipelines.yml`.

### Основные этапы:

1.  **Build & Test:**
    *   Сборка Docker-образов для микросервисов.
    *   Запуск линтеров (Ruff, Mypy) и тестов (Pytest).
2.  **Push:**
    *   Собранные образы пушатся в Container Registry с тегом коммита.
3.  **Deploy:**
    *   Пайплайн подключается к продакшн-серверу по SSH.
    *   Обновляет `docker-compose.yml` (или переменные окружения).
    *   Выполняет `docker-compose pull` и `docker-compose up -d`.

### Особенности Legacy
Для старого сервиса `club-api` деплой может происходить по упрощенной схеме: `git pull` прямо на сервере и перезапуск systemd юнитов или docker-контейнеров.

## Управление секретами

Переменные окружения (`DB_PASSWORD`, `API_KEY`, `JWT_SECRET`) **не хранятся в репозитории**.

*   **В CI/CD:** Настроены как Repository Variables в интерфейсе Bitbucket.
*   **На сервере (Runtime):** Хранятся в файле `.env`, который не попадает в Git. Этот файл монтируется в контейнеры при запуске.

## Логирование

В продакшене логи приложений пишутся в стандартный вывод (`stdout/stderr`).
Docker собирает их с помощью драйвера `json-file`.

**Просмотр логов на сервере:**
```bash
# Следить за логами конкретного сервиса (последние 100 строк)
docker logs -f --tail 100 club-events_app

# Логи всех сервисов
docker-compose logs -f --tail 50
```
