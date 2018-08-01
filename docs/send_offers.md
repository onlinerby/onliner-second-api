## Поставить уцененные товары в очередь на обработку

### POST /shop/offers

В теле запроса должен быть указан массив из объявлений в формате JSON.

Сохраняются только те объявления, которые прошли валидацию. Узнать количество объявлений,
которые не прошли валидацию, и причины отклонения можно, запросив статус обработки.

### Параметры объявления<a name="parameters"></a>:

|Параметр|Тип|Описание|
|---|---|---|
|id|string|Строковый ID объявления, присваивается автором объявления, нужен для обновления и изменения статуса|
|product.category|string|Название раздела|
|product.vendor|string|Название производителя|
|product.model|string|Название товара|
|price|object|Цена предложения, объект с полями amount и currency, допустимая валюта - только BYN|
|location.geo_town_id|integer|ID города [список допустимых городов](towns.md)|
|contact.name|string|Имя для контакта|
|contact.phones|array|Список контактных телефонов|
|contact.call_time|array|Время для звонка, указывается как два числа, с какого часа по какой звонить можно (0-23)|
|condition|integer|Состояние по шкале от 0 до 10|
|description|string|Описание предложения|
|delivery.available_for_country|boolean|Есть ли доставка по стране|
|photos|array|Список урлов фотографий, полученных из [Upload API](upload_images.md).  Принимаются только изображения, размещенные на домене onliner.by.|

### Пример запроса

```http
POST /shop/offers
Accept: application/json
Content-Type: application/json
Authorization: Bearer eyJ0eXAiOiJKV1MiLCJhbGciOiJIUzUxMiJ9.eyJ1c2VyX2lkIjoxLCJpYXQiOjE0NDQ2NDIxODMsInNjb3BlcyI6WyJ0ZXN0LnNjb3BlIl0sImV4cCI6MTQ0NTY0MjE4M30.7rZjdI5_ARGeufWF2ZaSP-88Hd2sUvWT49EAjSC6yrxmhu0wDWry9BJDPWV8yG6ZtRJlGHhCPPA4jGG1GE78Uw
```
```json
[
    {
        "id": "aszx98DD",
        "product": {
            "category": "Велосипеды",
            "vendor": "Orbea",
            "model": "Alma H50 27.5 (2015)"
        },
        "price": {
            "amount": "1250.00",
            "currency": "BYN"
        },
        "location": {
            "geo_town_id": 17030
        },
        "contact": {
            "name": "Александр",
            "phones": [
                "+375295181888"
            ],
            "call_time": [9, 18]
        },
        "condition": 10,
        "description": "Продам велосипед ORBEA ALMA H50",
        "delivery": {
            "available_for_country": true
        },
        "photos": [
            {
                "1280x1280": "https://content.onliner.by/baralog/1684694/x1280/e1ffcd1e6d8dc975e1457a1e01e8aeab.jpeg",
                "x80": "https://content.onliner.by/baralog/1684694/x80/e1ffcd1e6d8dc975e1457a1e01e8aeab.jpeg",
                "mobile_list": "https://content.onliner.by/baralog/1684694/mobile-list/e1ffcd1e6d8dc975e1457a1e01e8aeab.jpeg"
            },
            {
                "1280x1280": "https://content.onliner.by/baralog/1684694/x1280/998f13b1ab59aa306302713cdb2bba49.jpeg",
                "x80": "https://content.onliner.by/baralog/1684694/x80/998f13b1ab59aa306302713cdb2bba49.jpeg",
                "mobile_list": "https://content.onliner.by/baralog/1684694/mobile-list/998f13b1ab59aa306302713cdb2bba49.jpeg"
            },
            {
                "1280x1280": "https://content.onliner.by/baralog/1684694/x1280/d576be2d20ac28db67c69c5fcaeb9383.jpeg",
                "x80": "https://content.onliner.by/baralog/1684694/x80/d576be2d20ac28db67c69c5fcaeb9383.jpeg",
                "mobile_list": "https://content.onliner.by/baralog/1684694/mobile-list/d576be2d20ac28db67c69c5fcaeb9383.jpeg"
            },
            {
                "1280x1280": "https://content.onliner.by/baralog/1684694/x1280/c874bfa66af8153bca0cf28b5a1002eb.jpeg",
                "x80": "https://content.onliner.by/baralog/1684694/x80/c874bfa66af8153bca0cf28b5a1002eb.jpeg",
                "mobile_list": "https://content.onliner.by/baralog/1684694/mobile-list/c874bfa66af8153bca0cf28b5a1002eb.jpeg"
            }
        ]
    }
]
```

### Ответ при успешном создании предложения<a name="response"></a>:

```http
HTTP/1.1 202 Accepted
```
```json
{
    "id": "5a16c41acab2fd0202641102"
}
```

### Описание полей ответа<a name="fields"></a>:

|Параметр|Тип|Описание|
|---|---|---|
|id|string|ID импорта, по нему будет можно узнать статус импорта|

### Ответ при ошибке авторизации

```http
HTTP/1.1 403 Forbidden
Content-Type: application/json; charset=utf-8
```
```json
{
    "code": "access_denied",
    "message": "Требуется авторизация"
}
```
