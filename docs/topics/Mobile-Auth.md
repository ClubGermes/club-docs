# Авторизация и Сессии (Mobile)

## Механизм входа

1.  Экран: `src/views/Login/Login.svelte`.
2.  Метод: Двухфакторная авторизация (Телефон/Email + Код подтверждения) или стандартный пароль.
3.  API запросы (из `src/queries/auth.ts`):
    *   `userSetCode` — запрос кода.
    *   `userLogin` — вход по паролю.
    *   `userValidateCode` — вход по коду.

## Экраны авторизации

В этом разделе представлен визуальный обзор процесса авторизации.

### Вход в приложение
Пользователь может войти, используя Email или номер телефона.

![Login Email](mobile/auth/auth-login-step1-email.png){ width="300" }
![Login Phone](mobile/auth/auth-login-step1-phone.png){ width="300" }
![Password Input](mobile/auth/auth-login-step1-password.png){ width="300" }

### Подтверждение кодом
При выборе входа через код (или при регистрации) требуется подтверждение.

![Code Email](mobile/auth/auth-login-step2-email-code.png){ width="300" }
![Code Phone](mobile/auth/auth-login-step2-phone-code.png){ width="300" }

### Регистрация
![Registration](mobile/auth/auth-registration-form.png){ width="300" }

### Системные разрешения
![Push Notifications](mobile/auth/auth-push-notification-permission-prompt.png){ width="300" }

## Хранение сессии

*   **Место:** `localStorage`.
*   **Реализация:** `src/helpers/storage.ts`.
*   **Ключи:** Используется префикс `___`, например `___:token`.

> **Примечание по безопасности:** Так как это WebView-приложение, токены хранятся в стандартном веб-хранилище, а не в системном Keychain/Keystore.

## Восстановление сессии

При старте приложения (`src/main.ts` -> `src/helpers/auth.ts` -> `init()`):
1.  `token.load()` загружает токен из localStorage.
2.  Устанавливается WebSocket соединение для получения уведомлений в реальном времени.
3.  Отправляется сообщение `register` с токеном.

Если токен невалиден или отсутствует, система перенаправляет пользователя на экран входа.