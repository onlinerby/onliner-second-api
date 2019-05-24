# Onliner Second API

Адрес API - https://second.api.onliner.by

Данное API создано для магазинов для автоматизации управления объявлениями на уцененные товары. 

Ресурс доступен только по протоколу HTTPS.

Для всех запросов следует указывать HTTP заголовок **Accept**. По умолчанию его необходимо передавать со значением *application/json*.

Если происходит передача данных от клиента на сервер (например, при выполнении запросов типа POST, PATCH, DELETE), необходимо указывать заголовок **Content-Type** со значением *application/json*.

При выполнении каждого запроса в заголовках запроса необходимо передавать заголовок **Authorization** cо значением *Bearer %AUTH_TOKEN%*,
где *%AUTH_TOKEN%* - авторизационный токен, полученный после [авторизации на стороне b2bapi](https://github.com/onlinerby/onliner-b2b-api/blob/master/docs/oauth20.md).

Для работы с second.api необходимо зарегистрировать новое приложение на [странице настройки API](http://b2b.onliner.by/api).
По-умолчанию данная настройка недоступна. Для получения доступа к ней необходимо связаться с отделом B2B.

**Для получения уведомлений об изменениях в API нажмите кнопку Watch вверху страницы. Полный список изменений вы сможете увидеть в файле CHANGELOG**

- [Список изменений](CHANGELOG.md)
- [Получение доступа к API](https://github.com/onlinerby/onliner-b2b-api/blob/master/docs/oauth20.md)
- [Список населенных пунктов, в которых допускается размещение объявлений на уцененные товары](docs/towns.md)
- [Загрузка изображений для объявлений на уцененные товары](docs/upload_images.md)

## Оплата за размещение объявлений 

Оплата за размещение объявлений происходит единоразово в начале размещения за весь срок размещения с лицевого счета клиента B2B.
Срок размещения - 30 дней. 
Если объявление остается открытым более 30 дней - происходит повторное списание на срок до истечения следующих 30 дней.
Если объявление будет закрыто ранее срока размещения - средства не будут возвращены.

Суммой списания берется 3% от стоимости товара в объявлении, но не более 3 тарифных единиц. 
Округление стоимости происходит в большую сторону с точностью до 0.01 тарифной единицы. 
Транзакции по списанию группируются по разделам размещения и доступны в финансовом отчете в личном кабинете клиента. 

## Доступные методы API

|Ресурс|Описание|
|---|---|
| POST [/shop/offers](/docs/send_offers.md)| поставить уцененные товары в очередь на обработку |
| DELETE [/shop/offers](/docs/close_offers.md) | закрыть объявления на уцененные товары |
| GET [/shop/import-report/{id}](/docs/get_import_report.md)| узнать статус обработки уцененных товаров |
| GET [/shop/import-report/{id}/errors](/docs/get_import_errors.md)| узнать ошибки обработки уцененных товаров |
| GET [/shop/offers](/docs/get_offers.md)| получить информацию о загруженных уцененных товарах |

## Статусы импорта объявлений<a name="import-statuses"></a>

|Статус|Описание|
|---|---|
|pending|Ожидание обработки в очереди|
|processing|Импорт в процессе|
|processed|Импорт закончен|
|failed|Импорт завершился с ошибкой|

## В случае возникновения проблем либо технических вопросов по работе с API, создавайте Issue в данном репозитории.
