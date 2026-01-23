# Club Client: Обзор и Стек

⚠️ **Важно:** Вопреки первоначальным ожиданиям, это приложение **не является** React Native проектом.
Это **Hybrid Application**, написанное на **Svelte 4**, которое рендерится в системном WebView с помощью **Capacitor**.

## Технологический стек

| Слой | Технология | Версия | Описание |
|---|---|---|---|
| **UI Framework** | **Svelte** | 4.2.19 | Реактивный компонентный фреймворк. |
| **Runtime** | **Capacitor** | 7.0.0 | Мост к нативным API (iOS/Android). |
| **Build Tool** | **Vite** | 5.4.19 | Сборщик проекта. |
| **Language** | **TypeScript** | 5.5.3 | Строгая типизация. |
| **Styling** | **TailwindCSS** | 3.4 | + DaisyUI для UI-кита. |
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

## Обзор Интерфейса

### Главный экран
Центральный хаб приложения, агрегирующий статус резидента и быстрые действия.

![Home Screen](mobile/home/home.png) { width="300" }

**Ключевые элементы:**

### 1. Карточка резидента
Отображает аватар, статус (например, "Резидент") и клубный ID.
![User Card](home-user-card.png) { width="300" }

### 2. Сканер (QR)
Кнопка для быстрого чек-ина на мероприятиях или обмена контактами.
![Scanner Button](home-scanner-button.png) { width="150" }

*При первом использовании запрашивается системное разрешение на доступ к камере (через `@capacitor/camera`):*
![Camera Permission](home-camera-permission-prompt.png) { width="250" }

### 3. Лог событий
Лента последних уведомлений и активностей пользователя.
![Activity Log](home-log.png) { width="300" }
