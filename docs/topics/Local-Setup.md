# Quick Start (Локальный запуск)

Для работы с API и базой:

1.  **Запуск окружения**:
    ```bash
    docker-compose up -d postgres redis
    ```
2.  **Запуск Club Events**:
    ```bash
    cd backend/club-events
    # Создать .env
    # Установить зависимости
    python -m club_events.main
    ```
3.  **Доступ к GraphQL Playground**:
    *   URL: `http://localhost:8001/graphql`
    *   *Важно:* Добавить хедер `X-API-Key` (значение из `.env`).
