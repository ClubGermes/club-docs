# Архитектура Mobile Client

Архитектура приложения идентична `club-manager`, что указывает на единый стиль разработки в компании (Service-Oriented Frontend).

## Структура папок

*   `src/libs/Router/` — Кастомный роутер.
*   `src/queries/` — Слой API (описание запросов).
*   `src/helpers/` — "Движок" приложения (`entity.ts`, `collector.ts`, `request.ts`).
*   `src/views/` — Экраны приложения.
*   `src/components/` — UI компоненты.

## Navigation

Не используется ни `React Navigation`, ни `Expo Router`.
Реализован **собственный роутер** на базе History API:
*   Файл: `src/libs/Router/router.ts`.
*   Класс `Router` управляет стеком представлений.
*   Компонент `<RouterView />` в `App.svelte` рендерит текущую страницу.
*   Поддерживает "Гарды" (`guards.ts`) для проверки авторизации.

## API Layer & Data Flow

Используется паттерн **Entity + Collector**.
*   **Collector** (`src/helpers/collector.ts`) собирает запросы в очередь.
*   **Request** (`src/helpers/request.ts`) использует `XMLHttpRequest` (не `fetch`).
*   Логика получения данных вынесена из компонентов в файлы `queries/*.ts`.

**Пример:**
```typescript
/* src/views/Home/Home.svelte */
const eventsFeedHandler = new Entity({
    model: eventsFeed.model,
    retriever: eventsFeed.retriever, // Лежит в queries/event.ts
    onSuccess: (data) => { /* update store */ }
});

/* Вызов */
collector.get([ [eventsFeedHandler, { ...params }] ]);
```

## State Management

Используются **Svelte Stores**:
*   `src/stores/user.ts`: Данные пользователя.
*   `src/stores/token.ts`: Токен авторизации.
*   `src/stores/states.ts`: Глобальное состояние UI (loading, safe-areas).