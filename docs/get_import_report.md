## Узнать статус обработки уцененных товаров

### GET /shop/import-report/{id}

### Параметры<a name="parameters"></a>:

|Параметр|Тип|Описание|
|---|---|---|
|id|string|ID импорта, полученный после отправки объявлений на обработку|

### Пример запроса<a name="request"></a>:

```http
GET /shop/import-report/jFY
Accept: application/json
Content-Type: application/json
Authorization: Bearer eyJ0eXAiOiJKV1MiLCJhbGciOiJIUzUxMiJ9.eyJ1c2VyX2lkIjoxLCJpYXQiOjE0NDQ2NDIxODMsInNjb3BlcyI6WyJ0ZXN0LnNjb3BlIl0sImV4cCI6MTQ0NTY0MjE4M30.7rZjdI5_ARGeufWF2ZaSP-88Hd2sUvWT49EAjSC6yrxmhu0wDWry9BJDPWV8yG6ZtRJlGHhCPPA4jGG1GE78Uw
```

### Пример ответа<a name="response"></a>:

```http
HTTP/1.1 200 OK
```
```json
{
    "id": "jFY",
    "status": "processed",
    "total": 10,
    "errors": 1,
    "error_message": "Ошибка валидации"
}
```

### Описание полей ответа<a name="fields"></a>:

|Параметр|Тип|Описание|
|---|---|---|
|id|string|ID импорта|
|status|string|Статус импорта [описание](../README.md#import-statuses)|
|total|integer|Общее количество обработанных объявлений|
|errors|integer|Количество объявлений, которые не прошли валидацию и не были сохранены|
|error_message|string|Общее сообщение об ошибке|
