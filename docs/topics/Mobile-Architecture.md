# Архитектура Mobile Client

Архитектура мобильного приложения практически идентична `club-manager`, что упрощает работу фулстек-разработчика.

## Структура папок

*   `src/libs/Router/` — Кастомный роутер.
*   `src/queries/` — Слой API (описание запросов к бэкенду).
*   `src/helpers/` — Ядро приложения (`entity.ts`, `collector.ts`, `request.ts`).
*   `src/views/` — Экраны (Views) приложения.
*   `src/components/` — Переиспользуемые компоненты.

## Navigation (Навигация)

Реализован **собственный роутер** на базе History API:
*   Файл: `src/libs/Router/router.ts`.
*   Класс `Router` управляет стеком представлений и анимациями переходов.
*   Компонент `<RouterView />` в `App.svelte` отвечает за рендеринг текущей страницы.
*   Поддерживает "Гарды" (`guards.ts`) для защиты приватных маршрутов.

## API Layer & Data Flow

Используется паттерн **Entity + Collector**, знакомый по менеджеру.
*   **Collector** собирает запросы в очередь для оптимизации.
*   **Request** (`src/helpers/request.ts`) использует `XMLHttpRequest`.
*   Логика получения данных вынесена в файлы `queries/*.ts`.

**Пример вызова данных:**
```typescript
const eventsFeedHandler = new Entity({
    model: eventsFeed.model,
    retriever: eventsFeed.retriever, 
    onSuccess: (data) => { /* логика обновления */ }
});

collector.get([ [eventsFeedHandler, { ...params }] ]);
```

## State Management

Используются нативные **Svelte Stores**:
*   `src/stores/user.ts`: Профиль пользователя.
*   `src/stores/token.ts`: Токен авторизации.
*   `src/stores/states.ts`: Глобальные флаги (состояние сети, safe-area).
