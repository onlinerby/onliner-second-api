## Получить информацию о загруженных уцененных товарах

### GET /shop/offers

### Параметры<a name="parameters"></a>:

|Параметр|Тип|Описание|
|---|---|---|
|page|integer|Номер страницы|
|limit|integer|Лимит записей на странице, по умолчанию - 50, максимально - 50|

### Пример запроса <a name="request"></a>:

```http
GET /shop/offers
Accept: application/json
Content-Type: application/json
Authorization: Bearer eyJ0eXAiOiJKV1MiLCJhbGciOiJIUzUxMiJ9.eyJ1c2VyX2lkIjoxLCJpYXQiOjE0NDQ2NDIxODMsInNjb3BlcyI6WyJ0ZXN0LnNjb3BlIl0sImV4cCI6MTQ0NTY0MjE4M30.7rZjdI5_ARGeufWF2ZaSP-88Hd2sUvWT49EAjSC6yrxmhu0wDWry9BJDPWV8yG6ZtRJlGHhCPPA4jGG1GE78Uw
```

### Пример ответа <a name="response"></a>:

```http
HTTP/1.1 200 OK
```
```json
{
    "offers": [
        {
            "id": "offer1",
            "status": "correction",
            "reason": {
                "id": 1,
                "text": "Недостоверная цена",
                "comment": "Слишком маленькая цена"
            }
        },
        {
            "id": "offer2",
            "status": "approved",
            "reason": null
        }
    ],
    "total": 2,
    "page": {
        "limit": 50,
        "items": 2,
        "current": 1,
        "last": 1
    }
}

```

### Описание полей ответа<a name="fields"></a>:

|Параметр|Тип|Описание|
|---|---|---|
|offers.*.id|string|ID объявления, его можно использовать для обновления данных и/или изменения статуса объявления|
|offers.*.status|string|Статус объявления [описание статусов](../README.md#import-statuses)|
|offers.*.reason|object or null|Причина последнего изменения статуса|
|total|int|Количество всех объявлений в выборке|
|page.limit|int|Ограничение по количеству объявлений на странице|
|page.items|int|Количество объявлений на данной странице|
|page.current|int|Номер текущей страницы|
|page.last|int|Номер последней страницы|

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

### Возможные ошибки для полей:

|Параметр|Ошибки|
|---|---|
|page|Значение поля должно быть целым числом<br/> Минимум 1|
|limit|Значение поля должно быть целым числом<br/> Минимум 1<br/> Максимум 50|
