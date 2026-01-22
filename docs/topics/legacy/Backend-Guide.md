# Работа с Backend (Litestar & Legacy)

## 1. Рекомендации по разработке (Dev Guidelines)

1.  **При создании новых фич**: Используйте **`club-events`** (Litestar) как шаблон.
    *   Определяйте Pydantic модели для валидации.
    *   Используйте Tortoise ORM модели.
    *   Выставляйте GraphQL резолверы.
2.  **Работа с Legacy (`club-api`)**:
    *   Минимизируйте изменения в `app/models/*.py`.
    *   Если нужно добавить сложную логику событий, делайте это в микросервисе, а в монолите оставляйте только ссылки.
3.  **Миграции БД**:
    *   В `club-events` используется `aerich`. Всегда генерируйте миграции: `aerich migrate`.
    *   В `club-api` миграции, вероятно, SQL-файлами (ручное управление).
4.  **Типизация**:
    *   Strict typing в новых сервисах обязателен (`mypy`).

---

## 2. Club Events Service (Target Architecture)

Этот сервис является эталонным для новой архитектуры.

*   **Стек**: Python 3.9+, Litestar, Strawberry (GraphQL), Tortoise ORM.
*   **Endpoint**: `POST /graphql`

### Ключевые Сущности (Domain)

| Сущность | Описание | Ключевые поля |
| :--- | :--- | :--- |
| **Event** | Событие клуба | `id`, `name`, `format` (enum), `time_event`, `place`, `active` |
| **EventSpeaker** | Спикер события | `event_id`, `user_id`, `audit` |
| **EventRegistration** | Регистрация участника | `event_id`, `user_id`, `confirmation` (bool), `audit` (status) |
| **EventProgram** | Программа события | `event_id`, `time_item`, `name`, `speakers` (JSON) |

### GraphQL API Spec

#### Queries (Запросы данных)

**1. Получение списка событий (`events`)**
Позволяет фильтровать и пагинировать события.
```graphql
query GetEvents($filters: EventFiltersInput, $pagination: PaginationInput) {
  events(filters: $filters, pagination: $pagination) {
    id
    name
    format      # ENUM: FORUM, BREAKFAST, WEBINAR, etc.
    timeEvent
    place
    active
    participantCount  # Computed field
    speakers {
      userId
      # Данные пользователя подтягиваются отдельно или хранятся денормализованно
    }
  }
}
```

**2. Детальная информация (`event`)**
```graphql
query GetEvent($id: Int!) {
  event(id: $id) {
    id
    detail
    program {
      timeItem
      name
    }
    registrationStatus # Computed: OPEN/FULL
  }
}
```

#### Mutations (Изменение данных)

**1. Создание события (`createEvent`)**
```graphql
mutation CreateEvent($input: CreateEventInput!) {
  createEvent(input: $input) {
    id
    name
  }
}
```

**2. Регистрация пользователя (`registerParticipant`)**
*Логика:* Проверяет существование события, создает запись в `events_users`. Обрабатывает `IntegrityError` при дублях.
```graphql
mutation Register($input: RegisterParticipantInput!) {
  registerParticipant(input: $input) {
    id
    confirmation
    audit # REGISTERED (0), CONFIRMED (1), ATTENDED (2)
  }
}
```

---

## 3. Club API Service (Legacy Core)

Монолит, обрабатывающий бизнес-логику через "толстые" модели и прямые SQL запросы.

*   **Стек**: Starlette, asyncpg (Raw SQL), Redis.
*   **Стиль**: REST-like (POST запросы с JSON body).

### Основные Endpoints

| Endpoint | Метод | Описание | Параметры (Body) |
| :--- | :--- | :--- | :--- |
| `/login` | POST | Аутентификация | `account`, `password` |
| `/user/info` | POST | Данные пользователя | `id` (опционально) |
| `/feed` | POST | Лента новостей/событий | `from`, `to` (timestamps) |
| `/message/list` | POST | Список сообщений чата | `chat_id`, `limit`, `offset` |
| `/message/add` | POST | Отправка сообщения | `chat_id`, `text` |

### Особенности реализации (Backend Analysis)

1.  **Custom ORM / Query Builder**: В файле `app/core/pg.py` реализован класс `PgBank` и `PgPool`. Вместо стандартной ORM используется ручное управление транзакциями и курсорами.
    *   *Risk:* Высокая вероятность SQL-инъекций, если параметры не экранируются (используется `$1` синтаксис asyncpg, что безопасно, но требует внимания).
2.  **Толстые модели**: Логика находится внутри классов моделей (например, `User.search` в `app/models/user.py` содержит SQL запрос на 100+ строк).
    *   *Refactoring Target:* Эту логику нужно выносить в Use Cases / Services при миграции.
3.  **WebSocket Notifications**:
    *   Используется `PgQueue` (`LISTEN/NOTIFY` в PostgreSQL) для триггера событий.
    *   `app/core/api.py` рассылает сообщения через WebSocket подключенным клиентам.
