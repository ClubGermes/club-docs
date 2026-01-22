# Запуск и Сборка (Mobile)

Приложение можно запускать в двух режимах: как обычный веб-сайт (для быстрой верстки) и как мобильное приложение (через эмулятор/устройство).

## Предварительные требования

*   Node.js 20+
*   XCode (для iOS) / Android Studio (для Android)
*   CocoaPods (для iOS)

## Конфигурация (.env)

Используется файл `.env` в корне проекта `frontend/club-client`.
Основные переменные:
*   `API_URL` — адрес бэкенда (Gateway).
*   `EVENTS_API_KEY` — ключ доступа к API событий.

## Локальная разработка (Browser)

Самый быстрый способ для верстки UI без использования нативных функций устройства.

```bash
cd frontend/club-client
npm install
npm run dev
```

## Запуск в Эмуляторе (Capacitor)

Для тестирования нативных функций (камера, пуши, safe-area).

1.  **Сборка веб-части:**
    ```bash
    npm run build
    ```
    *Важно:* Capacitor берет файлы из папки `build` (настроено в `capacitor.config.ts`).

2.  **Синхронизация с нативными проектами:**
    ```bash
    npx cap sync
    ```

3.  **Запуск:**
    *   **iOS:** `npx cap run ios` (откроет XCode).
    *   **Android:** `npx cap run android` (откроет Android Studio).

## OTA Обновления (Live Updates)

В проекте настроен плагин `@capgo/capacitor-updater`.
Конфигурация находится в `capacitor.config.ts`.
*   Позволяет обновлять JS/CSS бандл на лету, минуя App Store / Google Play.
*   Сервер обновлений: `http://ota.grizodubov.ru:5173`.
