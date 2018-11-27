## Шаблон печатной формы
Средствами JSON API можно запрашивать списки шаблонов печатных форм для сущностей. Кодом сущности для стандартных шаблонов в составе JSON API является ключевое слово **embeddedtemplate**, а для пользовательских **customtemplate**.

#### Стандартные шаблоны 
#### Атрибуты сущности
+ **meta** - [Метаданные](/api/remap/1.2/doc/index.html#header-метаданные) о стандартном шаблоне
+ **id** - id шаблона
+ **name** - наименование шаблона
+ **type** - тип шаблона (entity - документ)
+ **content** - ссылка на скачивание

+ Parameters
  + type: `demand` (required, string) - тип сущности, для которой запрашиваются стандартные шаблоны

### Список стандартных шаблонов 
Запрос на получение информации о стандартных шаблонах печатных форм для указанного типа сущности.
+ Response 200 (application/json)
Успешный запрос. Результат - JSON представление списка стандартных шаблонов для данного типа сущности.
  + Body
        <!-- include(body/template/embeddedtemplate.json) -->


### Отдельный стандартный шаблон 
+ Parameters
  + type: `demand` (required, string) - тип сущности, для которой запрашивается стандартный шаблон
  + id: `7944ef04-f831-11e5-7a69-971500188b19` (required, string) - id отдельного шаблона

#### Отдельный стандартный шаблон 
Запрос на получение информации об отдельном стандартном шаблоне печатной формы для указанного типа сущности по его id.
+ Response 200 (application/json)
Успешный запрос. Результат - JSON представление стандартного шаблона для данного типа сущности.
  + Body
        <!-- include(body/template/embeddedtemplate_id.json) -->

#### Стандартные шаблоны для ценников и этикеток 
#### Атрибуты сущности
+ **meta** - [Метаданные](/api/remap/1.2/doc/index.html#header-метаданные) о стандартном шаблоне
+ **id** - id шаблона
+ **name** - наименование шаблона
+ **type** - тип шаблона (mxtemplate - новый тип шаблона для ценников и этикеток)
+ **content** - ссылка на скачивание

### Список стандартных ценников и этикеток 
Запрос на получение информации о стандартных шаблонах печатных форм для товаров, модификаций, услуг и комплектов.
+ Response 200 (application/json)
Успешный запрос. Результат - JSON представление списка стандартных шаблонов для товаров, модификаций, услуг и комплектов .
  + Body
        <!-- include(body/assortment/metadata/embeddedtemplate.json) -->

### Отдельный стандартный шаблон для ценников и этикеток 
+ Parameters
  + id: `7944ef04-f831-11e5-7a69-971500188b19` (required, string) - id отдельного шаблона

#### Отдельный стандартный ценник или этикетка 
Запрос на получение информации об отдельном стандартном шаблоне печатной формы для товаров, модификаций, услуг и комплектов по его id.
+ Response 200 (application/json)
Успешный запрос. Результат - JSON представление стандартного шаблона для товаров, модификаций, услуг и комплектов.
  + Body
        <!-- include(body/assortment/metadata/embeddedtemplate_id.json) -->


#### Пользовательские шаблоны 
#### Атрибуты сущности
+ **meta** - [Метаданные](/api/remap/1.2/doc/index.html#header-метаданные) о пользовательском шаблоне
+ **id** - id шаблона
+ **name** - наименование шаблона
+ **type** - тип шаблона (entity - документ)
+ **content** - ссылка на скачивание

+ Parameters
  + type: `customerorder` (required, string) - тип сущности, для которой запрашиваются пользовательские шаблоны

### Список пользовательских шаблонов 
Запрос на получение информации о пользовательских шаблонах печатных форм для указанного типа сущности.
+ Response 200 (application/json)
Успешный запрос. Результат - JSON представление списка стандартных шаблонов для данного типа сущности.
  + Body
        <!-- include(body/template/customtemplate.json) -->


### Отдельный пользовательский шаблон 
+ Parameters
  + type: `customerorder` (required, string) - тип сущности, для которой запрашивается стандартный шаблон
  + id: `7944ef04-f831-11e5-7a69-971500188b19` (required, string) - id отдельного шаблона

#### Отдельный пользовательский шаблон 
Запрос на получение информации об отдельном пользовательском шаблоне печатной формы для указанного типа сущности по его id.
+ Response 200 (application/json)
Успешный запрос. Результат - JSON представление пользовательского шаблона для данного типа сущности.
  + Body
        <!-- include(body/template/customtemplate_id.json) -->

#### Пользовательские шаблоны для ценников и этикеток 
#### Атрибуты сущности
+ **meta** - [Метаданные](/api/remap/1.2/doc/index.html#header-метаданные) о пользовательском шаблоне
+ **id** - id шаблона
+ **name** - наименование шаблона
+ **type** - тип шаблона (mxtemplate - тип шаблона для ценников и этикеток)
+ **content** - ссылка на скачивание

### Список пользовательских ценников и этикеток
Запрос на получение информации о пользовательских шаблонах печатных форм для товаров, модификаций, услуг и комплектов.
+ Response 200 (application/json)
Успешный запрос. Результат - JSON представление списка пользовательских шаблонов для товаров, модификаций, услуг и комплектов.
  + Body
        <!-- include(body/assortment/metadata/embeddedtemplate.json) -->

### Отдельный пользовательский шаблон для ценников и этикеток 
+ Parameters
  + id: `7944ef04-f831-11e5-7a69-971500188b19` (required, string) - id отдельного шаблона

#### Отдельный пользовательский ценник или этикетка 
Запрос на получение информации об отдельном пользовательском шаблоне печатной формы для товаров, модификаций, услуг и комплектов по его id.
+ Response 200 (application/json)
Успешный запрос. Результат - JSON представление пользовательского шаблона для товаров, модификаций, услуг и комплектов.
  + Body
        <!-- include(body/assortment/metadata/customtemplate_id.json) -->