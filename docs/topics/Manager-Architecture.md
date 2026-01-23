# Архитектура и Структура кода

Приложение построено как **SPA** (Single Page Application) с кастомной реализацией маршрутизации и слоя данных.

## Структура папок (`src/`)

```text
src/
├── components/     # Переиспользуемые UI-компоненты (Avatar, Charts, Inputs)
├── data/           # Статические данные (константы, списки)
├── helpers/        # Утилиты и ядро логики
│   ├── request.ts    # Обертка над XHR для API запросов
│   ├── websocket.ts  # Управление WS соединением
│   ├── entity.ts     # Базовый класс для моделей данных
│   └── collector.ts  # Менеджер очереди запросов
├── layouts/        # Обертки страниц (Main, Auth, Error)
├── libs/           # Сторонние или сложные кастомные модули
│   └── Router/       # Собственный движок маршрутизации
├── queries/        # Описания API энпоинтов (Паттерн "Query Object")
├── routes/         # Конфигурация маршрутов
├── stores/         # Глобальные Svelte сторы
└── views/          # Страницы приложения (экраны)
```

## Data Flow (Слой данных)

В приложении используется кастомный паттерн **Entity/Collector**.

1.  **Queries (`src/queries/`)**: Здесь описываются модели данных и методы их получения. Это не просто функции, а объекты, содержащие `model` (структура данных по умолчанию) и `retriever` (асинхронная функция запроса).
2.  **Entity (`src/helpers/entity.ts`)**: Класс-обертка, который связывает `Query` с состоянием загрузки (`loading store`).
3.  **Collector (`src/helpers/collector.ts`)**: Центральный диспетчер запросов. Вместо прямого вызова API, компоненты отправляют задачи в `collector.get()`.
4.  **Transport (`src/helpers/request.ts`)**: Низкоуровневая обертка над **XMLHttpRequest**.
    *   **Внимание:** Используется `XMLHttpRequest`, а не `fetch` или `axios`.
    *   Обрабатывает кастомные заголовки `x-binding-*`.

### Пример использования (в компоненте):

```typescript
/* Инициализация Entity */
const userSearchHandler = new Entity({
    model: userSearch.model,
    retriever: userSearch.retriever,
    onSuccess: (data) => { /* обработка */ }
});

/* Вызов через Collector */
function get() {
    collector.get([
        [ userSearchHandler, { filter: '...' } ]
    ]);
}
```

## State Management

Используются нативные **Svelte Stores**:
*   `src/stores/user.ts`: Данные текущего администратора. Сохраняются в localStorage.
*   `src/stores/token.ts`: Токен авторизации.
*   `src/stores/states.ts`: Глобальные флаги (loading, connect, api urls).