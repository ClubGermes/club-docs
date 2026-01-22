# Установка и Запуск (Manager)

## Требования

*   **Node.js** 18+
*   **npm** (или yarn)

## Установка зависимостей

```bash
cd frontend/club-manager
npm install
```

## Разработка (Development)

Запуск приложения в режиме разработки с hot-reload:

```bash
npm run dev
```
Приложение будет доступно по адресу: `http://localhost:1420` (или другом порту, указанном в консоли).

## Сборка (Production Build)

Для создания статических файлов для деплоя на веб-сервер (Nginx/Apache):

```bash
npm run build
```
Результат сборки появится в папке `dist/`.

## Конфигурация (.env)

Приложение использует переменные окружения для настройки адресов API.

Пример настроек:
```ini
VITE_API_URL=https://api.clubgermes.ru/v1
VITE_MEDIA_URL=https://media.clubgermes.ru
VITE_WSO_URL=wss://api.clubgermes.ru/ws
```

> **Примечание по Tauri:** В проекте есть папка `src-tauri`, но она **не используется** в текущем производственном цикле. Сборка десктопных бинарных файлов на данный момент не является целевой.