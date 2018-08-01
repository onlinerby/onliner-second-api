## Закрыть объявления на уцененные товары

### DELETE /shop/offers

Если при выполнении запроса встречается объявление несуществующее, либо вне доступа,
либо без товара, либо с товаром в невалидном статусе, оно просто пропускается,
все остальные объявления обрабатываются, возвращается статус 200 с указанием обновленных объявлений.

### Параметры запроса<a name="parameters"></a>:

|Параметр|Тип|Описание|
|---|---|---|
|offer_ids|array|Массив строковых ID объявлений, которые нужно закрыть|

### Пример запроса

```http
DELETE /shop/offers
Accept: application/json
Content-Type: application/json
Authorization: Bearer eyJ0eXAiOiJKV1MiLCJhbGciOiJIUzUxMiJ9.eyJ1c2VyX2lkIjoxLCJpYXQiOjE0NDQ2NDIxODMsInNjb3BlcyI6WyJ0ZXN0LnNjb3BlIl0sImV4cCI6MTQ0NTY0MjE4M30.7rZjdI5_ARGeufWF2ZaSP-88Hd2sUvWT49EAjSC6yrxmhu0wDWry9BJDPWV8yG6ZtRJlGHhCPPA4jGG1GE78Uw
```
```json
{
    "offer_ids": ["aszx98DD", "xyz"]
}
```

### Ответ

```http
HTTP/1.1 200 OK
```
```json
{
    "updated_offers": ["aszx98DD", "xyz"]
}
```

### Ответ при возникновении ошибок валидации

```http
HTTP/1.1 422 Unprocessable Entity
Content-Type: application/json; charset=utf-8
```
```json
{
    "message": "Validation failed",
    "errors": {
        "offer_ids": [
            "Значение поля должно быть последовательным индексированным массивом"
        ]
    }
}
```

### Возможные ошибки для полей<a name="errors"></a>:

|Параметр|Ошибки|
|---|---|
|offer_ids|Значение поля должно быть последовательным индексированным массивом|
