# KAZNA API
<show-structure for="chapter,procedure" depth="3"/>

> Для получения полных данных по результатам поиска от всех источников необходимо
повторять запрос до получения в ответе параметра `status = complete`.
{style="note"}

- Аутентификация Партнеров осуществляется на основе токена (на тестовой зоне - `testservice`).

## 3.1. Метод search
Запрос Начислений по идентификаторам Пользователя или УИН.

`subscribe` - Уникальный идентификатор Подписки Пользователя, 
в случае заполнения данного параметра создастся подписка на уведомления по новым/измененным/оплаченным/аннулированным начислениям, 
которые будут найдены по соответствующим Идентификаторам Пользователя переданным в `payerDoc`. 
Если данный subscribe уже зарегистрирован в Сервисе, 
то новые payerDoc будут добавлены к уже существующей Подписке Пользователя.
> Для получения тестовых данных (УИН, идентификаторы пользователя) необходимо обратиться к Менеджеру проекта.

### 1. gibdd ВУ
```bash
curl -L 'https://demopay.oplatagosuslug.ru/api/kazna/2.2/search' \
-H 'Content-Type:  application/json; charset=utf-8' \
-H 'Authorization: Bearer testservice' \
-d '{
    "depType": "gibdd",
    "payerDoc": [
        {
            "code": "22",
            "value": [
                "7712345678",
                "7712345679"
            ]
        }
    ],
    "subscribe": "user12345"
}'
```
> Стандартные РФ: 
> [ABEKMHOPCTYX]{1}\d{3}
> [ABEKMHOPCTYX]{2}\d{2,3} 
> Иные:
> [ABEKMHOPCTYXDFGIJLNQRSUVWZ 
> БГДЖЗИЙЛПФЦЧШЩЪЫЬЭЮЯ0-9\-]
> Например: RUSВ887ОА763, где в самом 
> начале идет код страны.

### 2. gibdd СТС
```bash
curl -L 'https://demopay.oplatagosuslug.ru/api/kazna/2.2/search' \
-H 'Content-Type:  application/json; charset=utf-8' \
-H 'Authorization: Bearer testservice' \
-d '{
  "depType": "gibdd",
  "payerDoc": [
    {
      "code": "24",
      "value": [
        "9934749756",
        "9934749757"
      ]
    }
  ],
  "subscribe": "user12345"
}'
```
> 10 цифр или 2 цифры + 2 буквы + 6 цифр.
### 3. gibdd ИНН КПП
```bash
curl -L 'https://demopay.oplatagosuslug.ru/api/kazna/2.2/search' \
-H 'Content-Type:  application/json; charset=utf-8' \
-H 'Authorization: Bearer testservice' \
-d '{
    "depType": "gibdd",
    "payerDoc": [
        {
            "code": "200",
            "value": [
                "7717751677771801001",
                "1650322494165001001"
            ]
        }
    ],
    "subscribe": "user12345"
}'
```
> * ИНН (10 цифр) + КПП (9 симв.)
> 7717751677771801001 
> * 00000 + КИО (5 симв.) + КПП (9
симв.)
0000051677771801001 
> * Код налогоплательщика юридического лица в стране
регистрации без «№», «-» и др.
разделителей (14 симв, дополняется
слева 0) + буквенный код страны по
ОКСМ (2 симв.) + цифровой код
страны по ОКСМ (3 цифр.)
00000123456789DE276
### 4. gibdd ИП
```bash
curl -L 'https://demopay.oplatagosuslug.ru/api/kazna/2.2/search' \
-H 'Content-Type:  application/json; charset=utf-8' \
-H 'Authorization: Bearer testservice' \
-d '{
    "depType": "gibdd",
    "payerDoc": [
        {
            "code": "400",
            "value": [
                "0000000226606001503"
            ]
        }
    ],
    "subscribe": "user12345"
}'
```
> 0000000 + ИНН  (12 цифр)
> 0000000226606001503
### Выходные параметры
Статус выполнения поиска по переданным идентификаторам:

`part` – частичный результат, ответ получен не от всех источников;
`complete` – полный результат, поиск завершен.

Step with a [link](https://www.jetbrains.com)