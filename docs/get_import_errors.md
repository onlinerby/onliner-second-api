## Узнать ошибки обработки уцененных товаров

### GET /shop/import-report/{id}/errors

### Параметры<a name="parameters"></a>:

|Параметр|Тип|Описание|
|---|---|---|
|id|string|ID импорта, полученный после отправки объявлений на обработку|

### Пример запроса<a name="request"></a>:

```http
GET /shop/import-report/jFY/errors
Accept: application/json
Content-Type: application/json
Authorization: Bearer eyJ0eXAiOiJKV1MiLCJhbGciOiJIUzUxMiJ9.eyJ1c2VyX2lkIjoxLCJpYXQiOjE0NDQ2NDIxODMsInNjb3BlcyI6WyJ0ZXN0LnNjb3BlIl0sImV4cCI6MTQ0NTY0MjE4M30.7rZjdI5_ARGeufWF2ZaSP-88Hd2sUvWT49EAjSC6yrxmhu0wDWry9BJDPWV8yG6ZtRJlGHhCPPA4jGG1GE78Uw
```

### Пример ответа<a name="response"></a>:

```http
HTTP/1.1 200 OK
```
```json
[
    {
        "id": "offer1",
        "errors": {
            "product.model": ["Укажите товар"],
            "description": ["Добавьте описание к объявлению"]
        }
    },
    {
        "id": "offer2",
        "errors": {
            "product.category": ["Раздел не подключен"]
        }
    }
]
```
