# Club Client: Обзор и Стек

⚠️ **Важно:** Это приложение **не является** React Native проектом. Это **Hybrid Application**, написанное на **Svelte 4**, которое рендерится в системном WebView с помощью **Capacitor**.

## Технологический стек

| Слой | Технология | Версия | Описание |
|---|---|---|---|
| **UI Framework** | **Svelte** | 4.x | Реактивный компонентный фреймворк. |
| **Runtime** | **Capacitor** | 7.x | Мост к нативным API (iOS/Android). |
| **Build Tool** | **Vite** | 5.x | Сборщик проекта. |
| **Language** | **TypeScript** | 5.x | Строгая типизация. |
| **Styling** | **TailwindCSS** | 3.x | + DaisyUI для UI-кита. |
| **Updates** | **Capgo** | 7.x | OTA (Over-the-Air) обновления без пересборки в сторы. |

## Основные библиотеки

Вместо нативных библиотек React Native используются веб-решения и плагины Capacitor:

*   **Routing:** Кастомный роутер (`src/libs/Router`), идентичный тому, что в `club-manager`.
*   **Gestures:** `svelte-swipe-cards` для свайпов (Tinder-like интерфейс).
*   **UI Components:** `swiper` (слайдеры), `cupertino-pane` (нижние шторки/модалки).
*   **Native Features:**
    *   `@capacitor/camera` — фото.
    *   `@capacitor/push-notifications` — пуши.
    *   `@capacitor-mlkit/barcode-scanning` — сканирование QR.
    *   `capacitor-plugin-safe-area` — отступы под "челку" и "бороду" телефонов.

## Отличия от React Native

Так как это WebView приложение:
1.  Весь UI — это HTML/CSS.
2.  Производительность зависит от оптимизации DOM и JS.
3.  Навигация имитирует нативные переходы, но фактически это SPA (Single Page Application).
