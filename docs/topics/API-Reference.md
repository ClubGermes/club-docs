# Справочник API

> **ℹ️ Динамическая документация**
> Вместо статического списка эндпоинтов, который быстро устаревает, мы используем автогенерируемую документацию Swagger/OpenAPI.

## Где найти документацию

При запущенном проекте (`make dev`) документация доступна по следующим адресам:

### Backend Services

| Сервис | Тип | URL Документации |
|--------|-----|------------------|
| **club-api** | REST (Swagger) | [http://localhost:8000/docs](http://localhost:8000/docs) |
| **club-users** | REST (Swagger) | [http://localhost:8001/docs](http://localhost:8001/docs) |
| **club-comments** | REST (Swagger) | [http://localhost:8002/docs](http://localhost:8002/docs) |
| **club-gateway-m** | REST (Swagger) | [http://localhost:8004/docs](http://localhost:8004/docs) |
| **club-media** | REST (Swagger) | [http://localhost:8005/docs](http://localhost:8005/docs) |
| **club-events** | GraphQL Playground | [http://localhost:8003/schema/graphql](http://localhost:8003/schema/graphql) |

### Как пользоваться
1.  Откройте ссылку в браузере.
2.  Нажмите кнопку **Authorize** (если требуется).
3.  Используйте `Try it out` для тестирования запросов прямо из браузера.