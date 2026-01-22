# Работа с файлами (Club Media)

Сервис `club-media` отвечает за загрузку, обработку (ресайз) и хранение статических файлов (аватарки, иконки событий, фото отчетов).

> **⚠️ Legacy Warning:**
> Сервис использует **прямую запись на файловую систему** сервера. S3 или облачные хранилища в текущей версии **не используются**.

## Архитектура хранения

Файлы сохраняются по абсолютным путям, жестко прописанным в коде (`backend/apps/club-media/app/routes/endpoints/file.py`).

### Основные директории

| Тип контента | Путь на сервере | Публичный URL |
|--------------|-----------------|---------------|
| Аватары пользователей | `/var/www/media.clubgermes.ru/html/avatars/` | `https://media.clubgermes.ru/avatars/{id}.jpg` |
| Иконки событий | `/var/www/static.clubgermes.ru/html/events/{id}/` | `https://static.clubgermes.ru/events/{id}/icon.png` |
| Фото новостей | `/var/www/static.clubgermes.ru/html/news/{id}/` | `https://static.clubgermes.ru/news/{id}/img.jpg` |
| Новые медиа (UUID) | `/var/www/media.clubgermes.ru/html/n/` | `https://media.clubgermes.ru/n/{hash}.jpg` |

## Процесс загрузки

1.  **Auth:** Клиент отправляет токен. Сервис проверяет его через внешний запрос к `https://eta.clubgermes.ru:5007` (Auth Service).
2.  **Processing:** Загруженное изображение обрабатывается библиотекой **Pillow (PIL)**:
    *   Конвертация в JPEG/PNG.
    *   Ресайз (thumbnail) до 320x320 или 200x200.
3.  **Storage:** Файл записывается на диск через `aiofiles`.

## Локальная разработка

Так как сервис пытается писать в `/var/www/...`, локальный запуск на macOS/Windows без прав root или предварительно созданных папок **приведет к ошибке**.

**Решение для локального запуска:**
1.  Создать необходимые директории локально и дать права текущему пользователю:
    ```bash
    sudo mkdir -p /var/www/media.clubgermes.ru/html/avatars
    sudo mkdir -p /var/www/static.clubgermes.ru/html/events
    sudo chown -R $(whoami) /var/www/
    ```
2.  Или (лучше) изменить пути в коде на относительные/конфигурируемые (требует рефакторинга).
