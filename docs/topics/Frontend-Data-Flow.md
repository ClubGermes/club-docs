# Управление данными (The "Entity" Pattern)

В проекте реализован собственный паттерн через классы `Entity` и `Collector`, вместо прямого использования `fetch` или библиотек вроде TanStack Query.

## Как это работает

1.  **Определение (Queries):**
    В папке `src/queries/` описываются модели данных.
    *   Пример (`src/queries/user.ts`): Объект `userSearch` содержит `model` (начальное состояние) и `retriever` (функция запроса к API).

2.  **Инициализация (View):**
    В компоненте (например, `Home.svelte`) создается экземпляр `Entity`.
    ```typescript
    const userSearchHandler = new Entity({
        model: userSearch.model,
        retriever: userSearch.retriever,
        onSuccess: (data) => { /* логика обновления UI */ }
    });
    ```

3.  **Выполнение (Collector):**
    Запросы не вызываются напрямую. Они передаются в `collector.get(...)`.
    *   `collector` (в `src/helpers/collector.ts`) управляет очередью запросов, предотвращает дублирование и управляет состоянием `busy`.

4.  **Реактивность:**
    `Entity` создает Svelte-сторы внутри себя (`this.loading`). Компоненты подписываются на `$loading` для отображения индикаторов загрузки.
