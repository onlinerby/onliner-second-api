# Загрузка фотографий

Адрес API https://upload.api.onliner.by

Ресурс доступен только по протоколу HTTPS.

Перед созданием объявления необходимо загрузить изображения для объявления. Upload API не выполняет привязку изображений к объявлениям, поэтому после каждой загрузки изображения, необходимо сохранять на Вашей стороне пути, по которым находятся загруженные изображения и их привязку к объявлению в Вашей системе. При создании объявления необходимо передавать все пути, полученные из Upload API.

Загрузка фотографий осуществляется в 2 этапа:

1. [Загрузить картинку на обработку](#step_1)
2. [Запросить статус обработки картинки](#step_2)

---

## 1. Загрузить картинку на обработку<a name="step_1"></a>

### POST /upload?token=%AUTH_TOKEN%

Картинка помещается в очередь обработки, клиенту возвращается ID задачи на обработку, по которому он сможет запросить ее статус.

#### Параметры:

|Параметр|Тип|Описание|
|---|---|---|
|meta[type]|string|Должно быть указано **baralog-photo**|

#### Описание полей ответа:
|Параметр|Тип|Описание|
|---|---|---|
|id|string|Уникальный ID задачи на обработку|
|hash|string|Хэш содержимого картинки|
|images|array|Список полных путей ко всем доступным ресайзам картинки|
|created_at|string|Дата постановки на обработку|
|errors|array|Список ошибок, которые произошли во время обработки картинки|
|updated_at|string|Дата последнего обновления записи|
|status|string|Статус обработки (см. ниже)|
|url|string|Ресурс запроса статуса, из которого можно получить список ссылок на обработанные картинки|

#### Статусы обработки изображения
|Статус|Описание|
|---|---|
| **uploaded** | картинка помещена в очередь |
| **processing** | картинка поступила на обработку | 
| **processed** | картинка обработана | 
| **error** | картинка обработана с ошибкой |

#### Пример запроса:

```http
POST /upload?token=a5e3272f5f31429e61ef2833d1ca5ff051777036
Accept: application/json
```
```
meta[type]=baralog-photo
file=<содержимое файла>
```

#### Пример ответа:

```json
{
    "id" : "PhTrlafxvBUPudeM",
    "hash" : "d8b6900306954b8268cf58c4119ad106",
    "images" : [],
    "created_at" : "2014-07-29 18:01:48",
    "errors" : [],
    "status" : "uploaded",
    "url" : "https://upload.api.onliner.by/upload/PhTrlafxvBUPudeM"
}
```

---

## 2. Запросить статус обработки картинки<a name="step_2"></a>

### POST GET /upload/{id}

где id - уникальный ID задачи на обработку, полученный из шага 1.

#### Пример запроса:

```http
GET /upload/PhTrlafxvBUPudeM
Accept: application/json
```

#### Пример ответа:

```json
{
    "id" : "PhTrlafxvBUPudeM",
    "hash" : "47800cb27e72119614356d2fedc6af87",
    "images" : {
        "1280x1280": "https://content.onliner.by/baralog/1684694/x1280/e1ffcd1e6d8dc975e1457a1e01e8aeab.jpeg",
        "x80": "https://content.onliner.by/baralog/1684694/x80/e1ffcd1e6d8dc975e1457a1e01e8aeab.jpeg",
        "mobile_list": "https://content.onliner.by/baralog/1684694/mobile-list/e1ffcd1e6d8dc975e1457a1e01e8aeab.jpeg"
    },
    "created_at" : "2014-07-29 17:57:30",
    "errors" : [],
    "updated_at" : "2014-07-29 17:57:30",
    "status" : "processed",
    "url" : "https://upload.api.onliner.by/upload/PhTrlafxvBUPudeM"
}
```

---

### Пример полного процесса загрузки картинок на PHP:

```php
function getUploadImageUrls($checkUrl) {
    sleep(1);
    $process = curl_init($checkUrl);

    curl_setopt($process, CURLOPT_HTTPHEADER, ['Accept: application/json']);
    curl_setopt($process, CURLOPT_RETURNTRANSFER, TRUE);

    $result = curl_exec($process);
    $result = json_decode($result, true);

    curl_close($process);

    if ($result['status'] == 'error') {
        var_dump($result['errors']);
        exit;
    }
    if ($result['status'] == 'processed') {
        return $result['images'];
    }

    return getUploadImageUrls($checkUrl);
}

if (!function_exists('curl_file_create')) {
    function curl_file_create($filename, $mimetype = '', $postname = '') {
        return "@$filename;filename="
            . ($postname ?: basename($filename))
            . ($mimetype ? ";type=$mimetype" : '');
    }
}

$process = curl_init("https://upload.api.onliner.by/upload?token=$token");
curl_setopt($process, CURLOPT_HTTPHEADER, ['Accept: application/json']);
curl_setopt($process, CURLOPT_RETURNTRANSFER, TRUE);
curl_setopt($process, CURLOPT_POSTFIELDS, [
    'meta[type]' => 'baralog-photo',
    'file' => curl_file_create('/path/to/2.jpg')
]);
$result = curl_exec($process);

curl_close($process);

$checkUrl = json_decode($result, true)['url'];

var_dump(getUploadImageUrls($checkUrl));
```
