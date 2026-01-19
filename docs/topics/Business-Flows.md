# Потоки Данных (Data Flow)

## 1. Request Lifecycle (Общий поток)

```mermaid
sequenceDiagram
    participant Client as Mobile App
    participant Gateway as Club Gateway
    participant Events as Club Events (Litestar)
    participant Core as Club API (Starlette)
    participant DB as PostgreSQL

    Client->>Gateway: HTTP Request (Bearer Token)
    Gateway->>Gateway: Auth Check (Redis/DB)
    Gateway->>Gateway: Resolve User ID
    
    alt Event Request (GraphQL)
        Gateway->>Events: POST /graphql (Header: X-User-ID, X-API-Key)
        Events->>DB: ORM Query (Tortoise)
        DB-->>Events: Result
        Events-->>Gateway: JSON (GraphQL Response)
    else Core Request (REST)
        Gateway->>Core: HTTP Request (Header: X-User-ID)
        Core->>DB: Raw SQL Query
        DB-->>Core: Result
        Core-->>Gateway: JSON Response
    end
    
    Gateway-->>Client: Final Response
```

## 2. Аутентификация и Безопасность

1.  **Клиент -> Gateway**: Использует сессионный токен. Gateway валидирует его через Redis/DB и извлекает `user_id`.
2.  **Gateway -> Microservices**:
    *   **Authentication**: Gateway добавляет заголовок `X-API-Key` (секретный ключ сервера).
    *   **Context**: Gateway добавляет заголовок `X-User-ID` (ID текущего пользователя).
3.  **Club Events Middleware**:
    *   Проверяет `X-API-Key`. Если неверен -> 403 Forbidden.
    *   Доверяет `X-User-ID` и инжектит его в контекст GraphQL резолверов.
