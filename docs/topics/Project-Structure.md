# Структура проекта

Этот проект представляет собой монорепозиторий, объединяющий бэкенд-микросервисы, фронтенд-приложения и инфраструктурные скрипты.

## Общая схема

```text
club-platform/
├── backend/                  # Все бэкенд-сервисы (Python)
│   ├── apps/
│   │   ├── club-api/         # Основной API (Legacy Core)
│   │   ├── club-users/       # Сервис пользователей и авторизации
│   │   ├── club-comments/    # Сервис комментариев
│   │   ├── club-events/      # Целевая архитектура (Litestar + GraphQL)
│   │   ├── club-gateway-m/   # API Gateway для мобильного приложения
│   │   └── club-media/       # Сервис загрузки и обработки медиа
│   ├── setup-all.sh          # Скрипт первоначальной настройки
│   └── start-all.sh          # Скрипт запуска (используется Makefile)
│
├── frontend/                 # Фронтенд-приложения
│   ├── club-manager/         # Админ-панель (Svelte + Tauri)
│   ├── club-client/          # Мобильное приложение (React Native / Expo)
│   └── ...
│
├── infrastructure/           # Инфраструктура
│   ├── postgres/             # Init-скрипты для БД
│   └── ...
│
├── club-docs/                # Документация (Writerside)
├── Makefile                  # Главная точка входа для управления проектом
└── docker-compose.yml        # Инфраструктура (Postgres, Redis)
```

## Контекстная диаграмма (C4 Context)

```mermaid
C4Context
    title System Context Diagram - Club Germes Platform

    Person(client, "Club Member", "Мобильное приложение (iOS/Android)")
    Person(manager, "Club Manager", "Веб-админка")

    System_Boundary(club_system, "Club Platform Monorepo") {
        System(gateway, "API Gateway", "Auth, Routing, User Context")
        System(legacy_api, "Club API (Legacy)", "Core logic, Users, Chat, Feed")
        System(events_api, "Club Events (New)", "Events Management, GraphQL")
    }

    System_Ext(db, "Yandex Managed PostgreSQL", "Main Database")
    System_Ext(redis, "Redis", "Sessions, Cache, Pub/Sub")
    System_Ext(s3, "Yandex Object Storage", "Backups & Logs")
    System_Ext(fs, "Local File System", "Media Storage (Legacy)")

    System_Ext(sms_gate, "Beeline A2P", "SMS Sending")
    System_Ext(firebase, "Firebase (FCM)", "Push Notifications")
    System_Ext(telegram, "Telegram API", "Bot Notifications")
    System_Ext(mail_gate, "Club Mail Service", "Internal SMTP Relay")

    Rel(client, gateway, "Uses", "HTTPS/REST")
    Rel(manager, gateway, "Uses", "HTTPS/REST")

    Rel(gateway, legacy_api, "Proxies", "HTTP")
    Rel(gateway, events_api, "Proxies", "HTTP")

    Rel(legacy_api, db, "Reads/Writes", "asyncpg")
    Rel(events_api, db, "Reads/Writes", "TortoiseORM")
    Rel(legacy_api, redis, "Auth/Cache", "aioredis")

    Rel(legacy_api, sms_gate, "Sends SMS", "HTTP")
    Rel(legacy_api, firebase, "Sends Pushes", "HTTP")
    Rel(legacy_api, telegram, "Sends Messages", "HTTP")
    Rel(legacy_api, mail_gate, "Sends Emails", "HTTP")
    Rel(legacy_api, fs, "Stores Avatars/Images", "File I/O")
```

## Ключевые компоненты

### Backend
Весь бэкенд написан на **Python**.
Архитектура представляет собой набор микросервисов, работающих через **Honcho** в режиме разработки.

*   **Технологический стек:** Starlette, Uvicorn, asyncpg (или Tortoise ORM в новых сервисах).
*   **Межсервисное взаимодействие:** Преимущественно HTTP REST.
*   **База данных:** Единый инстанс PostgreSQL (разделение на схемы логическое).

#### Статусы сервисов
*   **club-api (Legacy):** Основной монолит, содержит большую часть бизнес-логики.
*   **club-events (Target):** Новый сервис на Litestar/GraphQL. Находится в стадии активной разработки и миграции функционала из монолита.
*   **club-gateway-m:** Точка входа для мобильного приложения (BFF).

### Frontend
*   **club-manager:** Настольное приложение для менеджеров клуба (Svelte).

### Infrastructure
Для локальной разработки используется гибридная схема:
*   **Docker:** Запускает только базы данных (Postgres, Redis).
*   **Local Process:** Микросервисы запускаются прямо в хост-системе через Python (venv).