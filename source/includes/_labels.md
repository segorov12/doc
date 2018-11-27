## Печать этикеток и ценников
Средствами JSON API можно запрашивать печать этикеток и ценников с помощью шаблонов печатных форм.
При запросе на формирование печатной формы сервер при готовности этикеток и ценников, корректной
печатной форме и правильном формате запроса отвечает пустым ответом с кодом 303.
В заголовке Location ответа содержится адрес временного расположения готовой к загрузке печатной формы.
Файл во временном расположении доступен для загрузки в течение 5 минут.

Сервер может вернуть ответ 202 и заголовок Location с адресом для опроса готовности печатной формы к загрузке.
Данный вариант будет реализован позже.

Печать этикеток и ценников доступна для товаров, услуг, комплектов и модификаций.

#### Печать этикеток и ценников 

+ Parameters
  + type: `product` (required, string) - тип сущности, для которой запрашивается печать
  + id: `a86708d2-f8d3-4e67-8f04-6101158da808` (required, string) - id сущности, для которой запрашивается печать

### Запрос на печать этикеток и ценников 

Запрос на печать этикеток и ценников по шаблону печатной формы.
#### Атрибуты запроса
+ **organization** - Ссылка на ваше юрлицо в формате [Метаданных](/api/remap/1.2/doc/index.html#header-метаданные)
+ **count** - Количество ценников/термоэтикеток. Максимальное количество - `1000`
+ **salePrice** - Цена продажи
  + **priceType** - Ссылка на тип цены в формате [Метаданных](/api/remap/1.2/doc/index.html#header-метаданные)
+ **template** - Ссылка на шаблон для печати в формате [Метаданных](/api/remap/1.2/doc/index.html#header-метаданные)


+ Request Пример (application/json)
Пример запроса на печать этикеток и ценников по шаблону печатной формы для товаров.
  + Body
        <!-- include(requests/print_product.json) -->

+ Response 202
  + Headers
    Location: ссылка на статус печати
    Content-Type: application/json
  + Body
        <!-- include(responses/print_post.json) -->

+ Response 303
  + Headers
    Location: ссылка на файл
    Content-Type: application/json
  + Body
        <!-- include(responses/print_post.json) -->