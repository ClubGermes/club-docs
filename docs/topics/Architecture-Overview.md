# Архитектурный Обзор

Система представляет собой гибридную архитектуру, где сосуществуют legacy-сервисы и новые микросервисы.

## Компоненты

*   **Gateway (`club-gateway-m`)**: Единая точка входа для мобильных клиентов. Агрегирует запросы к микросервисам.
*   **Legacy Core (`club-api`)**: Основной монолит (Starlette). Отвечает за пользователей, чаты, уведомления, ленту. Использует Raw SQL (asyncpg).
*   **New Services (`club-events`)**: Микросервис событий (Litestar). Реализован по Clean Architecture + DDD. Использует Tortoise ORM и GraphQL (Strawberry).
*   **Infrastructure**:
    *   **DB**: Yandex Managed PostgreSQL (кластер). Сервисы используют разные схемы/таблицы в одной физической БД или разные БД.
    *   **Cache/Bus**: Redis (кэширование, pub/sub для WebSocket).

## Структура Базы Данных (ERD Highlights)

На основе анализа моделей и SQL запросов:

### Таблицы Core (`club-api`)
*   `users`: Основная таблица пользователей (`id`, `name`, `active`, `role_id`...).
*   `users_info`: Детали профиля (компания, должность).
*   `messages`: Чат-система (`author_id`, `target_id`, `text`).
*   `notifications`: Система уведомлений.

### Таблицы Events (`club-events`)
*   `events`: Мероприятия.
*   `events_users` (или `event_registrations`): Таблица связка M2M (User <-> Event). Содержит статус участия (`audit`).
*   `events_speakers`: Связка спикеров.

*Примечание: Сервисы, похоже, делят одну базу данных (или используют Foreign Data Wrappers), так как `club-api` делает запросы к таблицам событий, а `club-events` использует `user_id`.*
