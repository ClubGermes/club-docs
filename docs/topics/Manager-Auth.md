# Авторизация и Безопасность

## Процесс входа

1.  Пользователь вводит данные на странице `/auth` (`src/views/Login.svelte`).
2.  Используются запросы из `src/queries/auth.ts`:
    *   `userSetCode` (запрос SMS/Email кода).
    *   `userLogin` (вход по паролю).
    *   `userValidateCode` (вход по коду).
3.  При успешном ответе сервер возвращает объект, содержащий `_token` и `_user`.

## Хранение сессии

*   **Токен:** Хранится в `localStorage` с префиксом.
    *   Ключ: `___:token` (реализовано в `src/helpers/storage.ts`).
    *   При каждом `XMLHttpRequest` токен автоматически добавляется в тело запроса (поле `_token`), а не в заголовки Authorization (особенность `src/helpers/request.ts`).
    *   В GraphQL запросах токен передается в заголовке `Authorization: TOKEN ...`.

## WebSocket интеграция

После авторизации (`src/helpers/auth.ts` -> `init()`):
1.  Устанавливается соединение с `VITE_WSO_URL`.
2.  Отправляется сообщение `register` с токеном.
3.  Сервер шлет события (`message`), которые обрабатываются через `src/helpers/notifications.ts`.

## Ролевая модель (Client-side)

Проверка прав осуществляется в `src/layouts/Main/index.svelte` и Guards.
*   Роли хранятся в массиве `$user.roles`.
*   Основные роли: `admin`, `manager`, `moderator`, `chief`, `community manager`.
