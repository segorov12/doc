## Задача
Средствами JSON API можно создавать и обновлять сведения о задачах, запрашивать списки задач и сведения по отдельным задачам. Кодом сущности для задачи в составе JSON API является ключевое слово **task**. Больше о задачах и работе с ними в основном интерфейсе вы можете прочитать в нашей службе поддержки по  [этой ссылке](https://support.moysklad.ru/hc/ru/articles/203392263-%D0%97%D0%B0%D0%B4%D0%B0%D1%87%D0%B8).
### Задачи 
#### Атрибуты сущности
+ **meta** - [Метаданные](/api/remap/1.2/doc/index.html#header-метаданные) задачи
+ **id** - ID в формате UUID `Только для чтения`
+ **accountId** - ID учетной записи `Только для чтения`
+ **author** - Сотрудник создавший задачу. В формате [Метаданных](/api/remap/1.2/doc/index.html#header-метаданные) `Только для чтения`
+ **created** - Момент создания задачи `Только для чтения`
+ **updated** - Момент последнего обновления сущности `Только для чтения`
+ **description** - Текст задачи `Необходимое`
+ **dueToDate** - Срок задачи
+ **assignee** - Сотрудник, ответственный за выполнение задачи. В формате [Метаданных](/api/remap/1.2/doc/index.html#header-метаданные) `Необходимое`
+ **done** - Отметка о выполнении задачи.
+ **completed** - Время выполнения задачи. `Только для чтения`
+ **implementer** - Сотрудник выполнивший задачу. В формате [Метаданных](/api/remap/1.2/doc/index.html#header-метаданные) `Только для чтения`
+ **agent** - Контрагент или юрлицо, связанное с задачей. Задача может быть привязана либо к конрагенту, либо к юрлицу, либо к документу. В формате [Метаданных](/api/remap/1.2/doc/index.html#header-метаданные)
+ **operation** - Документ, связанный с задачей. Задача может быть привязана либо к конрагенту, либо к юрлицу, либо к документу. В формате [Метаданных](/api/remap/1.2/doc/index.html#header-метаданные)
+ **notes** - Ссылка на комментарии к задаче в формате [Метаданных](/api/remap/1.2/doc/index.html#header-метаданные)


#### Комментарии задачи
Объект комментария к задаче содержит следующие поля:
+ **author** - Сотрудник создавший комментарий. В формате [Метаданных](/api/remap/1.2/doc/index.html#header-метаданные) `Только для чтения`
+ **moment** - Момент создания комментария `Только для чтения`
+ **description** - Текст комментария `Необходимое`


#### Отображение списка по умолчанию
##### Для администратора
Если текущий сотрудник обладает правами администратора, то при запросе списка задач ему будут выведены все активные  (**done** = false) задачи, как те,
 что относятся к нему (сотрудник является создателем или исполнителем задачи), так и те, что относятся к другим сотрудникам.
 
##### Для сотрудника
Для сотрудника, не являющегося администратором, но имеющего право на просмотр всех задач, список задач по умолчанию будет аналогичен списку задач, выводимому для администратора. В противном случае, при запросе списка задач без каких-либо фильтров, будут выведены активные (**done** = false) задачи, которые создал текущий сотрудник и задачи, за которые ответственен текущий сотрудник.

#### Фильтры из web-интерфейса
В основном интерфейсе МоегоСклада для отображения списка задач существует 2 группы фильтров:
+ Фильтр по связанности с текущим сотрудником: `Поручено мне`, `Я поручил`, `Все задачи` (отображается только у администраторов)
+ Фильтр по готовности задачи: `Активные`, `Выполненные`.

Чтобы реализовать подобную фильтрацию списка для JSON API, нужно использовать следующие фильтры для списка задач:
+ **Поручено мне**: фильтр по полю **assignee** в значении которого указана ссылка на текущего сотрудника<br>
`https://online.moysklad.ru/api/remap/1.2/entity/task?filter=assignee=http://online.moysklad.ru/api/remap/1.2/entity/employee/<id текущего сотрудника>`
+ **Я поручил**: фильтр по полю **author** в значении которого указана ссылка на текущего сотрудника<br>
`https://online.moysklad.ru/api/remap/1.2/entity/task?filter=author=http://online.moysklad.ru/api/remap/1.2/entity/employee/<id текущего сотрудника>`
+ **Все задачи**: не требует фильтрации. Обратите внимание на пункт [Отображение списка по умолчанию](#header-отображение-списка-по-умолчанию)
+ **Активные**: фильтр по полю **done** со значением false<br>
`https://online.moysklad.ru/api/remap/1.2/entity/task?filter=done=false`
+ **Выполненные**: фильтр по полю **done** со значением true<br>
`https://online.moysklad.ru/api/remap/1.2/entity/task?filter=done=true`

#### Права доступа
| Операция                                                                         | Доступ                    |
| :------------------------------------------------------------------------------: |:--------------------------|
| Просмотр задач в которых текущий пользователь является ответственным или автором |	Все                      |
| Просмотр любых задач                                                             |	пользователь с правом на просмотр всех задач или администратор |
| Создание задачи	                                                                 |  необходима тарифная опция CRM, все
| Редактирование задачи                                                            |	необходима тарифная опция CRM, администратор, создатель задачи |
| Выполнение задачи	                                                               |  необходима тарифная опция CRM, администратор, создатель задачи, ответственный |
| Отмена выполнения задачи                                                         |	необходима тарифная опция CRM, администратор, создатель задачи, ответственный |
| Удаление задачи	                                                                 |  Администратор, создатель задачи                                                |



### Получить задачи 
Получить список задач.
Результат: Объект JSON, включающий в себя поля:
- **meta** [Метаданные](/api/remap/1.2/doc/index.html#header-метаданные) о выдаче,
- **context** - [Метаданные](/api/remap/1.2/doc/index.html#header-метаданные) о сотруднике, выполнившем запрос.
- **rows** - Массив JSON объектов, представляющих собой [Задачи](#задача).

**Параметры**

| Параметр                | Описание  |
| ------------------------------ |:---------------------------|
|limit |  `number` (optional) **Default: 1000** *Example: 1000* Максимальное количество сущностей для извлечения.`Допустимые значения 1 - 1000`.|
|offset |  `number` (optional) **Default: 0** *Example: 40* Отступ в выдаваемом списке сущностей.|

> Получить задачи 

```shell
curl -X GET
  "https://online.moysklad.ru/api/remap/1.2/entity/task"
  -H "Authorization: Basic <Access-Token>"
```

> Response 200 (application/json)
Успешный запрос. Результат - JSON представление списка задач.

```json
{
  "context": {
    "employee": {
      "meta": {
        "href": "https://online.moysklad.ru/api/remap/1.2/context/employee",
        "metadataHref": "https://online.moysklad.ru/api/remap/1.2/entity/employee/metadata",
        "type": "employee",
        "mediaType": "application/json"
      }
    }
  },
  "meta": {
    "href": "https://online.moysklad.ru/api/remap/1.2/entity/task",
    "type": "task",
    "mediaType": "application/json",
    "size": 2,
    "limit": 1000,
    "offset": 0
  },
  "rows": [
    {
      "meta": {
        "href": "https://online.moysklad.ru/api/remap/1.2/entity/task/47bc1d9b-8b87-11e8-d9ce-84d900000017",
        "metadataHref": "https://online.moysklad.ru/api/remap/1.2/entity/task/metadata",
        "type": "task",
        "mediaType": "application/json"
      },
      "id": "47bc1d9b-8b87-11e8-d9ce-84d900000017",
      "accountId": "98182760-8aa1-11e8-7210-075e00000001",
      "author": {
        "meta": {
          "href": "https://online.moysklad.ru/api/remap/1.2/entity/employee/98fa7086-8aa1-11e8-7210-075e0000002c",
          "metadataHref": "https://online.moysklad.ru/api/remap/1.2/entity/employee/metadata",
          "type": "employee",
          "mediaType": "application/json",
          "uuidHref": "https://online.moysklad.ru/app/#employee/edit?id=98fa7086-8aa1-11e8-7210-075e0000002c"
        }
      },
      "created": "2018-07-19 22:09:37",
      "updated": "2018-07-19 22:10:01",
      "description": "task3",
      "dueToDate": "2018-07-20 22:09:00",
      "assignee": {
        "meta": {
          "href": "https://online.moysklad.ru/api/remap/1.2/entity/employee/98fa7086-8aa1-11e8-7210-075e0000002c",
          "metadataHref": "https://online.moysklad.ru/api/remap/1.2/entity/employee/metadata",
          "type": "employee",
          "mediaType": "application/json",
          "uuidHref": "https://online.moysklad.ru/app/#employee/edit?id=98fa7086-8aa1-11e8-7210-075e0000002c"
        }
      },
      "done": false,
      "agent": {
        "meta": {
          "href": "https://online.moysklad.ru/api/remap/1.2/entity/counterparty/992b6965-8aa1-11e8-7210-075e00000057",
          "metadataHref": "https://online.moysklad.ru/api/remap/1.2/entity/counterparty/metadata",
          "type": "counterparty",
          "mediaType": "application/json",
          "uuidHref": "https://online.moysklad.ru/app/#company/edit?id=992b6965-8aa1-11e8-7210-075e00000057"
        }
      },
      "notes": {
        "meta": {
          "href": "https://online.moysklad.ru/api/remap/1.2/entity/task/47bc1d9b-8b87-11e8-d9ce-84d900000017/notes",
          "type": "tasknote",
          "mediaType": "application/json",
          "size": 2,
          "limit": 1000,
          "offset": 0
        }
      }
    },
    {
      "meta": {
        "href": "https://online.moysklad.ru/api/remap/1.2/entity/task/aaca57a2-8b86-11e8-d9ce-84d900000007",
        "metadataHref": "https://online.moysklad.ru/api/remap/1.2/entity/task/metadata",
        "type": "task",
        "mediaType": "application/json"
      },
      "id": "aaca57a2-8b86-11e8-d9ce-84d900000007",
      "accountId": "98182760-8aa1-11e8-7210-075e00000001",
      "author": {
        "meta": {
          "href": "https://online.moysklad.ru/api/remap/1.2/entity/employee/98fa7086-8aa1-11e8-7210-075e0000002c",
          "metadataHref": "https://online.moysklad.ru/api/remap/1.2/entity/employee/metadata",
          "type": "employee",
          "mediaType": "application/json",
          "uuidHref": "https://online.moysklad.ru/app/#employee/edit?id=98fa7086-8aa1-11e8-7210-075e0000002c"
        }
      },
      "created": "2018-07-19 22:05:14",
      "updated": "2018-07-19 22:09:42",
      "description": "task1",
      "dueToDate": "2018-07-20 22:05:00",
      "assignee": {
        "meta": {
          "href": "https://online.moysklad.ru/api/remap/1.2/entity/employee/98fa7086-8aa1-11e8-7210-075e0000002c",
          "metadataHref": "https://online.moysklad.ru/api/remap/1.2/entity/employee/metadata",
          "type": "employee",
          "mediaType": "application/json",
          "uuidHref": "https://online.moysklad.ru/app/#employee/edit?id=98fa7086-8aa1-11e8-7210-075e0000002c"
        }
      },
      "done": false,
      "operation": {
        "meta": {
          "href": "https://online.moysklad.ru/api/remap/1.2/entity/supply/7eb5b552-8aa3-11e8-7210-075e000000f1",
          "metadataHref": "https://online.moysklad.ru/api/remap/1.2/entity/supply/metadata",
          "type": "supply",
          "mediaType": "application/json",
          "uuidHref": "https://online.moysklad.ru/app/#supply/edit?id=7eb5b552-8aa3-11e8-7210-075e000000f1"
        }
      },
      "notes": {
        "meta": {
          "href": "https://online.moysklad.ru/api/remap/1.2/entity/task/aaca57a2-8b86-11e8-d9ce-84d900000007/notes",
          "type": "tasknote",
          "mediaType": "application/json",
          "size": 0,
          "limit": 1000,
          "offset": 0
        }
      }
    }
  ]
}
```

### Создать Задачу 
Создать новую задачу. Для создания новых задач необходима активная тарифная опция (https://support.moysklad.ru/hc/ru/articles/216649748-CRM-%D0%A0%D0%B0%D0%B1%D0%BE%D1%82%D0%B0-%D1%81-%D0%BA%D0%BB%D0%B8%D0%B5%D0%BD%D1%82%D1%81%D0%BA%D0%BE%D0%B9-%D0%B1%D0%B0%D0%B7%D0%BE%D0%B9).

> Пример запроса на создание новой задачи.

```shell
  curl -X POST
    "https://online.moysklad.ru/api/remap/1.2/entity/task"
    -H "Authorization: Basic <Access-Token>"
    -H "Content-Type: application/json"
      -d '{
            "description": "Исправить реквизиты юрлица",
            "dueToDate": "2017-05-12 17:12:00",
            "assignee": {
              "meta": {
                "href": "https://online.moysklad.ru/api/remap/1.2/entity/employee/98fa7086-8aa1-11e8-7210-075e0000002c",
                "metadataHref": "https://online.moysklad.ru/api/remap/1.2/entity/employee/metadata",
                "type": "employee",
                "mediaType": "application/json"
              }
            },
            "done": false,
            "agent": {
              "meta": {
                "href": "https://online.moysklad.ru/api/remap/1.2/entity/organization/9927bf0d-8aa1-11e8-7210-075e00000054",
                "metadataHref": "https://online.moysklad.ru/api/remap/1.2/entity/organization/metadata",
                "type": "organization",
                "mediaType": "application/json"
              }
            }
          }'  
```

> Response 200 (application/json)
Успешный запрос. Результат - JSON представление созданной задачи.

```json
{
  "meta": {
    "href": "https://online.moysklad.ru/api/remap/1.2/entity/task/cfe5a6ae-8b87-11e8-d9ce-84d900000000",
    "metadataHref": "https://online.moysklad.ru/api/remap/1.2/entity/task/metadata",
    "type": "task",
    "mediaType": "application/json"
  },
  "id": "cfe5a6ae-8b87-11e8-d9ce-84d900000000",
  "accountId": "98182760-8aa1-11e8-7210-075e00000001",
  "author": {
    "meta": {
      "href": "https://online.moysklad.ru/api/remap/1.2/entity/employee/98fa7086-8aa1-11e8-7210-075e0000002c",
      "metadataHref": "https://online.moysklad.ru/api/remap/1.2/entity/employee/metadata",
      "type": "employee",
      "mediaType": "application/json",
      "uuidHref": "https://online.moysklad.ru/app/#employee/edit?id=98fa7086-8aa1-11e8-7210-075e0000002c"
    }
  },
  "created": "2018-07-19 22:13:25",
  "updated": "2018-07-19 22:13:25",
  "description": "Исправить реквизиты юрлица",
  "dueToDate": "2017-05-12 17:12:00",
  "assignee": {
    "meta": {
      "href": "https://online.moysklad.ru/api/remap/1.2/entity/employee/98fa7086-8aa1-11e8-7210-075e0000002c",
      "metadataHref": "https://online.moysklad.ru/api/remap/1.2/entity/employee/metadata",
      "type": "employee",
      "mediaType": "application/json",
      "uuidHref": "https://online.moysklad.ru/app/#employee/edit?id=98fa7086-8aa1-11e8-7210-075e0000002c"
    }
  },
  "done": false,
  "agent": {
    "meta": {
      "href": "https://online.moysklad.ru/api/remap/1.2/entity/organization/9927bf0d-8aa1-11e8-7210-075e00000054",
      "metadataHref": "https://online.moysklad.ru/api/remap/1.2/entity/organization/metadata",
      "type": "organization",
      "mediaType": "application/json",
      "uuidHref": "https://online.moysklad.ru/app/#mycompany/edit?id=9927bf0d-8aa1-11e8-7210-075e00000054"
    }
  },
  "notes": {
    "meta": {
      "href": "https://online.moysklad.ru/api/remap/1.2/entity/task/cfe5a6ae-8b87-11e8-d9ce-84d900000000/notes",
      "type": "tasknote",
      "mediaType": "application/json",
      "size": 0,
      "limit": 1000,
      "offset": 0
    }
  }
}
```

### Массовое создание и обновление Задач 
[Массовое создание и обновление](/api/remap/1.2/doc/index.html#header-создание-и-обновление-нескольких-объектов) Задач.
В теле запроса нужно передать массив, содержащий JSON представления Задач, которые вы хотите создать или обновить.
Обновляемые Задачи должны содержать идентификатор в виде метаданных.

> Пример создания и обновления нескольких Задач

```shell
  curl -X POST
    "https://online.moysklad.ru/api/remap/1.2/entity/task"
    -H "Authorization: Basic <Access-Token>"
    -H "Content-Type: application/json"
      -d '[
            {
              "description": "Исправить реквизиты юрлица",
              "dueToDate": "2017-05-12 17:12:00",
              "assignee": {
                "meta": {
                  "href": "https://online.moysklad.ru/api/remap/1.2/entity/employee/98fa7086-8aa1-11e8-7210-075e0000002c",
                  "metadataHref": "https://online.moysklad.ru/api/remap/1.2/entity/employee/metadata",
                  "type": "employee",
                  "mediaType": "application/json"
                }
              },
              "done": false,
              "agent": {
                "meta": {
                  "href": "https://online.moysklad.ru/api/remap/1.2/entity/organization/9927bf0d-8aa1-11e8-7210-075e00000054",
                  "metadataHref": "https://online.moysklad.ru/api/remap/1.2/entity/organization/metadata",
                  "type": "organization",
                  "mediaType": "application/json"
                }
              }
            },
            {
              "meta": {
                "href": "https://online.moysklad.ru/api/remap/1.2/entity/task/cfe5a6ae-8b87-11e8-d9ce-84d900000000",
                "metadataHref": "https://online.moysklad.ru/api/remap/1.2/entity/task/metadata",
                "type": "task",
                "mediaType": "application/json"
              },
              "description": "Уточнить контактные лица",
              "assignee": {
                "meta": {
                  "href": "https://online.moysklad.ru/api/remap/1.2/entity/employee/98fa7086-8aa1-11e8-7210-075e0000002c",
                  "metadataHref": "https://online.moysklad.ru/api/remap/1.2/entity/employee/metadata",
                  "type": "employee",
                  "mediaType": "application/json"
                }
              },
              "done": true,
              "agent": {
                "meta": {
                  "href": "https://online.moysklad.ru/api/remap/1.2/entity/counterparty/992b6965-8aa1-11e8-7210-075e00000057",
                  "metadataHref": "https://online.moysklad.ru/api/remap/1.2/entity/counterparty/metadata",
                  "type": "counterparty",
                  "mediaType": "application/json"
                }
              }
            }
          ]'  
```

> Response 200 (application/json)
Успешный запрос. Результат - массив JSON представлений созданных и обновленных Задач.

```json
[
  {
    "meta": {
      "href": "https://online.moysklad.ru/api/remap/1.2/entity/task/9971ba00-8b88-11e8-d9ce-84d900000009",
      "metadataHref": "https://online.moysklad.ru/api/remap/1.2/entity/task/metadata",
      "type": "task",
      "mediaType": "application/json"
    },
    "id": "9971ba00-8b88-11e8-d9ce-84d900000009",
    "accountId": "98182760-8aa1-11e8-7210-075e00000001",
    "author": {
      "meta": {
        "href": "https://online.moysklad.ru/api/remap/1.2/entity/employee/98fa7086-8aa1-11e8-7210-075e0000002c",
        "metadataHref": "https://online.moysklad.ru/api/remap/1.2/entity/employee/metadata",
        "type": "employee",
        "mediaType": "application/json",
        "uuidHref": "https://online.moysklad.ru/app/#employee/edit?id=98fa7086-8aa1-11e8-7210-075e0000002c"
      }
    },
    "created": "2018-07-19 22:19:04",
    "updated": "2018-07-19 22:19:04",
    "description": "Исправить реквизиты юрлица",
    "dueToDate": "2017-05-12 17:12:00",
    "assignee": {
      "meta": {
        "href": "https://online.moysklad.ru/api/remap/1.2/entity/employee/98fa7086-8aa1-11e8-7210-075e0000002c",
        "metadataHref": "https://online.moysklad.ru/api/remap/1.2/entity/employee/metadata",
        "type": "employee",
        "mediaType": "application/json",
        "uuidHref": "https://online.moysklad.ru/app/#employee/edit?id=98fa7086-8aa1-11e8-7210-075e0000002c"
      }
    },
    "done": false,
    "agent": {
      "meta": {
        "href": "https://online.moysklad.ru/api/remap/1.2/entity/organization/9927bf0d-8aa1-11e8-7210-075e00000054",
        "metadataHref": "https://online.moysklad.ru/api/remap/1.2/entity/organization/metadata",
        "type": "organization",
        "mediaType": "application/json",
        "uuidHref": "https://online.moysklad.ru/app/#mycompany/edit?id=9927bf0d-8aa1-11e8-7210-075e00000054"
      }
    },
    "notes": {
      "meta": {
        "href": "https://online.moysklad.ru/api/remap/1.2/entity/task/9971ba00-8b88-11e8-d9ce-84d900000009/notes",
        "type": "tasknote",
        "mediaType": "application/json",
        "size": 0,
        "limit": 1000,
        "offset": 0
      }
    }
  },
  {
    "meta": {
      "href": "https://online.moysklad.ru/api/remap/1.2/entity/task/cfe5a6ae-8b87-11e8-d9ce-84d900000000",
      "metadataHref": "https://online.moysklad.ru/api/remap/1.2/entity/task/metadata",
      "type": "task",
      "mediaType": "application/json"
    },
    "id": "cfe5a6ae-8b87-11e8-d9ce-84d900000000",
    "accountId": "98182760-8aa1-11e8-7210-075e00000001",
    "author": {
      "meta": {
        "href": "https://online.moysklad.ru/api/remap/1.2/entity/employee/98fa7086-8aa1-11e8-7210-075e0000002c",
        "metadataHref": "https://online.moysklad.ru/api/remap/1.2/entity/employee/metadata",
        "type": "employee",
        "mediaType": "application/json",
        "uuidHref": "https://online.moysklad.ru/app/#employee/edit?id=98fa7086-8aa1-11e8-7210-075e0000002c"
      }
    },
    "created": "2018-07-19 22:13:25",
    "updated": "2018-07-19 22:19:04",
    "description": "Уточнить контактные лица",
    "dueToDate": "2017-05-12 17:12:00",
    "implementer": {
      "meta": {
        "href": "https://online.moysklad.ru/api/remap/1.2/entity/employee/98fa7086-8aa1-11e8-7210-075e0000002c",
        "metadataHref": "https://online.moysklad.ru/api/remap/1.2/entity/employee/metadata",
        "type": "employee",
        "mediaType": "application/json",
        "uuidHref": "https://online.moysklad.ru/app/#employee/edit?id=98fa7086-8aa1-11e8-7210-075e0000002c"
      }
    },
    "completed": "2018-07-19 22:19:04",
    "assignee": {
      "meta": {
        "href": "https://online.moysklad.ru/api/remap/1.2/entity/employee/98fa7086-8aa1-11e8-7210-075e0000002c",
        "metadataHref": "https://online.moysklad.ru/api/remap/1.2/entity/employee/metadata",
        "type": "employee",
        "mediaType": "application/json",
        "uuidHref": "https://online.moysklad.ru/app/#employee/edit?id=98fa7086-8aa1-11e8-7210-075e0000002c"
      }
    },
    "done": true,
    "agent": {
      "meta": {
        "href": "https://online.moysklad.ru/api/remap/1.2/entity/counterparty/992b6965-8aa1-11e8-7210-075e00000057",
        "metadataHref": "https://online.moysklad.ru/api/remap/1.2/entity/counterparty/metadata",
        "type": "counterparty",
        "mediaType": "application/json",
        "uuidHref": "https://online.moysklad.ru/app/#company/edit?id=992b6965-8aa1-11e8-7210-075e00000057"
      }
    },
    "notes": {
      "meta": {
        "href": "https://online.moysklad.ru/api/remap/1.2/entity/task/cfe5a6ae-8b87-11e8-d9ce-84d900000000/notes",
        "type": "tasknote",
        "mediaType": "application/json",
        "size": 0,
        "limit": 1000,
        "offset": 0
      }
    }
  }
]
```

### Удалить задачу

Запрос на удаление задачи с указанным id. Для удаления задач необходима активная тарифная опция (https://support.moysklad.ru/hc/ru/articles/216649748-CRM-%D0%A0%D0%B0%D0%B1%D0%BE%D1%82%D0%B0-%D1%81-%D0%BA%D0%BB%D0%B8%D0%B5%D0%BD%D1%82%D1%81%D0%BA%D0%BE%D0%B9-%D0%B1%D0%B0%D0%B7%D0%BE%D0%B9).
Также нельзя удалить задачи, созданные другими сотрудниками, без прав администратора.

**Параметры**

|Параметр   |Описание   | 
|---|---|
|id |  `string` (required) *Example: 7944ef04-f831-11e5-7a69-971500188b19* id задачи.|

> Запрос на удаление задачи с указанным id.
```shell
curl -X DELETE
  "https://online.moysklad.ru/api/remap/1.2/entity/task/7944ef04-f831-11e5-7a69-971500188b19"
  -H "Authorization: Basic <Access-Token>"
```

> Response 200 (application/json)
Успешное удаление задачи.

### Задача 

### Получить задачу

**Параметры**

|Параметр   |Описание   | 
|---|---|
|id |  `string` (required) *Example: 7944ef04-f831-11e5-7a69-971500188b19* id задачи.|
 
> Запрос на получение отдельной задачи с указанным id.

```shell
curl -X GET
  "https://online.moysklad.ru/api/remap/1.2/entity/task/7944ef04-f831-11e5-7a69-971500188b19"
  -H "Authorization: Basic <Access-Token>"
```

> Response 200 (application/json)
Успешный запрос. Результат - JSON представление задачи с указанным id.

```json
{
  "meta": {
    "href": "https://online.moysklad.ru/api/remap/1.2/entity/task/47bc1d9b-8b87-11e8-d9ce-84d900000017",
    "metadataHref": "https://online.moysklad.ru/api/remap/1.2/entity/task/metadata",
    "type": "task",
    "mediaType": "application/json"
  },
  "id": "47bc1d9b-8b87-11e8-d9ce-84d900000017",
  "accountId": "98182760-8aa1-11e8-7210-075e00000001",
  "author": {
    "meta": {
      "href": "https://online.moysklad.ru/api/remap/1.2/entity/employee/98fa7086-8aa1-11e8-7210-075e0000002c",
      "metadataHref": "https://online.moysklad.ru/api/remap/1.2/entity/employee/metadata",
      "type": "employee",
      "mediaType": "application/json",
      "uuidHref": "https://online.moysklad.ru/app/#employee/edit?id=98fa7086-8aa1-11e8-7210-075e0000002c"
    }
  },
  "created": "2018-07-19 22:09:37",
  "updated": "2018-07-19 22:13:10",
  "description": "task3",
  "dueToDate": "2018-07-20 22:09:00",
  "assignee": {
    "meta": {
      "href": "https://online.moysklad.ru/api/remap/1.2/entity/employee/98fa7086-8aa1-11e8-7210-075e0000002c",
      "metadataHref": "https://online.moysklad.ru/api/remap/1.2/entity/employee/metadata",
      "type": "employee",
      "mediaType": "application/json",
      "uuidHref": "https://online.moysklad.ru/app/#employee/edit?id=98fa7086-8aa1-11e8-7210-075e0000002c"
    }
  },
  "done": false,
  "agent": {
    "meta": {
      "href": "https://online.moysklad.ru/api/remap/1.2/entity/organization/9927bf0d-8aa1-11e8-7210-075e00000054",
      "metadataHref": "https://online.moysklad.ru/api/remap/1.2/entity/organization/metadata",
      "type": "organization",
      "mediaType": "application/json",
      "uuidHref": "https://online.moysklad.ru/app/#mycompany/edit?id=9927bf0d-8aa1-11e8-7210-075e00000054"
    }
  },
  "notes": {
    "meta": {
      "href": "https://online.moysklad.ru/api/remap/1.2/entity/task/47bc1d9b-8b87-11e8-d9ce-84d900000017/notes",
      "type": "tasknote",
      "mediaType": "application/json",
      "size": 2,
      "limit": 1000,
      "offset": 0
    }
  }
}
```

### Изменить задачу 
#### Описание
Изменить задачу с указанным id. Для изменения задач необходима активная тарифная опция (https://support.moysklad.ru/hc/ru/articles/216649748-CRM-%D0%A0%D0%B0%D0%B1%D0%BE%D1%82%D0%B0-%D1%81-%D0%BA%D0%BB%D0%B8%D0%B5%D0%BD%D1%82%D1%81%D0%BA%D0%BE%D0%B9-%D0%B1%D0%B0%D0%B7%D0%BE%D0%B9).
Также нельзя изменять задачи, созданные другими сотрудниками, без прав администратора.

**Параметры**

|Параметр   |Описание   | 
|---|---|
|id |  `string` (required) *Example: 7944ef04-f831-11e5-7a69-971500188b19* id задачи.|

> Пример запроса на обновление существующей задачи.

```shell
  curl -X PUT
    "https://online.moysklad.ru/api/remap/1.2/entity/task/7944ef04-f831-11e5-7a69-971500188b19"
    -H "Authorization: Basic <Access-Token>"
    -H "Content-Type: application/json"
      -d '{
            "description": "Уточнить контактные лица",
            "assignee": {
              "meta": {
                "href": "https://online.moysklad.ru/api/remap/1.2/entity/employee/98fa7086-8aa1-11e8-7210-075e0000002c",
                "metadataHref": "https://online.moysklad.ru/api/remap/1.2/entity/employee/metadata",
                "type": "employee",
                "mediaType": "application/json",
                "uuidHref": "https://online.moysklad.ru/app/#employee/edit?id=98fa7086-8aa1-11e8-7210-075e0000002c"
              }
            },
            "done": true,
            "agent": {
              "meta": {
                "href": "https://online.moysklad.ru/api/remap/1.2/entity/organization/9927bf0d-8aa1-11e8-7210-075e00000054",
                "metadataHref": "https://online.moysklad.ru/api/remap/1.2/entity/organization/metadata",
                "type": "organization",
                "mediaType": "application/json",
                "uuidHref": "https://online.moysklad.ru/app/#mycompany/edit?id=9927bf0d-8aa1-11e8-7210-075e00000054"
              }
            }
          }'  
```

> Response 200 (application/json)
Успешный запрос. Результат - JSON представление обновлённой задачи.

```json
{
  "meta": {
    "href": "https://online.moysklad.ru/api/remap/1.2/entity/task/47bc1d9b-8b87-11e8-d9ce-84d900000017",
    "metadataHref": "https://online.moysklad.ru/api/remap/1.2/entity/task/metadata",
    "type": "task",
    "mediaType": "application/json"
  },
  "id": "47bc1d9b-8b87-11e8-d9ce-84d900000017",
  "accountId": "98182760-8aa1-11e8-7210-075e00000001",
  "author": {
    "meta": {
      "href": "https://online.moysklad.ru/api/remap/1.2/entity/employee/98fa7086-8aa1-11e8-7210-075e0000002c",
      "metadataHref": "https://online.moysklad.ru/api/remap/1.2/entity/employee/metadata",
      "type": "employee",
      "mediaType": "application/json",
      "uuidHref": "https://online.moysklad.ru/app/#employee/edit?id=98fa7086-8aa1-11e8-7210-075e0000002c"
    }
  },
  "created": "2018-07-19 22:09:37",
  "updated": "2018-07-19 22:24:50",
  "description": "Уточнить контактные лица",
  "dueToDate": "2018-07-20 22:09:00",
  "implementer": {
    "meta": {
      "href": "https://online.moysklad.ru/api/remap/1.2/entity/employee/98fa7086-8aa1-11e8-7210-075e0000002c",
      "metadataHref": "https://online.moysklad.ru/api/remap/1.2/entity/employee/metadata",
      "type": "employee",
      "mediaType": "application/json",
      "uuidHref": "https://online.moysklad.ru/app/#employee/edit?id=98fa7086-8aa1-11e8-7210-075e0000002c"
    }
  },
  "completed": "2018-07-19 22:24:24",
  "assignee": {
    "meta": {
      "href": "https://online.moysklad.ru/api/remap/1.2/entity/employee/98fa7086-8aa1-11e8-7210-075e0000002c",
      "metadataHref": "https://online.moysklad.ru/api/remap/1.2/entity/employee/metadata",
      "type": "employee",
      "mediaType": "application/json",
      "uuidHref": "https://online.moysklad.ru/app/#employee/edit?id=98fa7086-8aa1-11e8-7210-075e0000002c"
    }
  },
  "done": true,
  "agent": {
    "meta": {
      "href": "https://online.moysklad.ru/api/remap/1.2/entity/organization/9927bf0d-8aa1-11e8-7210-075e00000054",
      "metadataHref": "https://online.moysklad.ru/api/remap/1.2/entity/organization/metadata",
      "type": "organization",
      "mediaType": "application/json",
      "uuidHref": "https://online.moysklad.ru/app/#mycompany/edit?id=9927bf0d-8aa1-11e8-7210-075e00000054"
    }
  },
  "notes": {
    "meta": {
      "href": "https://online.moysklad.ru/api/remap/1.2/entity/task/47bc1d9b-8b87-11e8-d9ce-84d900000017/notes",
      "type": "tasknote",
      "mediaType": "application/json",
      "size": 2,
      "limit": 1000,
      "offset": 0
    }
  }
}
```

### Комментарии Задачи 
Отдельный ресурс для управления комментариями Задачи. С его помощью вы можете управлять комментариями задачи, в которой количество комментариев превышает лимит на количество комментариев, сохраняемых вместе с задачей. Этот лимит равен 100.

### Получить комментарии Задачи 
Запрос на получение списка всех комментариев данной Задачи.
- **meta** [Метаданные](#header-метаданные) о выдаче,
- **context** - [Метаданные](#header-метаданные) о сотруднике, выполнившем запрос.
- **rows** - Массив JSON объектов, представляющих собой комментарии Задачи.

**Параметры**

|Параметр   |Описание   | 
|---|---|
|id |  `string` (required) *Example: 7944ef04-f831-11e5-7a69-971500188b19* id задачи.|
|   limit|   `number` (optional) **Default: 25** *Example: 100* Максимальное количество сущностей для извлечения.`Допустимые значения 1 - 100`.|
|   offset|   `number` (optional) **Default: 0** *Example: 40* Отступ в выдаваемом списке сущностей|
|   updatedFrom| `string` (optional) *Example: 2016-04-15 15:48:46* Один из [параметров фильтрации выборки](#header-параметры-фильтрации-выборки). Формат строки : `ГГГГ-ММ-ДД ЧЧ:ММ:СС[.ммм]`, Часовой пояс: `MSK` (Московское время)|
|   updatedTo|   `string` (optional) *Example: 2016-04-15 15:48:46* Один из [параметров фильтрации выборки](#header-параметры-фильтрации-выборки). Формат строки : `ГГГГ-ММ-ДД ЧЧ:ММ:СС[.ммм]`, Часовой пояс: `MSK` (Московское время)|
|   updatedBy|   `string` (optional) *Example: admin@admin* Один из [параметров фильтрации выборки](#header-параметры-фильтрации-выборки). Формат строки : `uid`|

> Получить комментарии Задачи

```shell
curl -X GET
  "https://online.moysklad.ru/api/remap/1.2/entity/task/7944ef04-f831-11e5-7a69-971500188b19/notes"
  -H "Authorization: Basic <Access-Token>"
```
 
> Response 200 (application/json)
Успешный запрос. Результат - JSON представление списка комментариев отдельной Задачи.

```json
{
  "context": {
    "employee": {
      "meta": {
        "href": "https://online.moysklad.ru/api/remap/1.2/context/employee",
        "metadataHref": "https://online.moysklad.ru/api/remap/1.2/entity/employee/metadata",
        "type": "employee",
        "mediaType": "application/json"
      }
    }
  },
  "meta": {
    "href": "https://online.moysklad.ru/api/remap/1.2/entity/task/47bc1d9b-8b87-11e8-d9ce-84d900000017/notes",
    "type": "tasknote",
    "mediaType": "application/json",
    "size": 2,
    "limit": 1000,
    "offset": 0
  },
  "rows": [
    {
      "meta": {
        "href": "https://online.moysklad.ru/api/remap/1.2/entity/task/47bc1d9b-8b87-11e8-d9ce-84d900000017/notes/55eaa2cc-8b87-11e8-d9ce-84d900000025",
        "type": "tasknote",
        "mediaType": "application/json"
      },
      "author": {
        "meta": {
          "href": "https://online.moysklad.ru/api/remap/1.2/entity/employee/98fa7086-8aa1-11e8-7210-075e0000002c",
          "metadataHref": "https://online.moysklad.ru/api/remap/1.2/entity/employee/metadata",
          "type": "employee",
          "mediaType": "application/json",
          "uuidHref": "https://online.moysklad.ru/app/#employee/edit?id=98fa7086-8aa1-11e8-7210-075e0000002c"
        }
      },
      "text": "текст комментария 1",
      "moment": "2018-07-19 22:10:01"
    },
    {
      "meta": {
        "href": "https://online.moysklad.ru/api/remap/1.2/entity/task/47bc1d9b-8b87-11e8-d9ce-84d900000017/notes/55eaac82-8b87-11e8-d9ce-84d900000026",
        "type": "tasknote",
        "mediaType": "application/json"
      },
      "author": {
        "meta": {
          "href": "https://online.moysklad.ru/api/remap/1.2/entity/employee/98fa7086-8aa1-11e8-7210-075e0000002c",
          "metadataHref": "https://online.moysklad.ru/api/remap/1.2/entity/employee/metadata",
          "type": "employee",
          "mediaType": "application/json",
          "uuidHref": "https://online.moysklad.ru/app/#employee/edit?id=98fa7086-8aa1-11e8-7210-075e0000002c"
        }
      },
      "text": "текст комментария 2",
      "moment": "2018-07-19 22:10:01"
    }
  ]
}
```

### Создать комментарий Задачи 
Запрос на создание нового комментария к Задаче.
Для успешного создания необходимо в теле запроса указать следующие поля:
+ **text** - Текст комментария. `Необходимое`

**Параметры**

|Параметр   |Описание   | 
|---|---|
|id |  `string` (required) *Example: 7944ef04-f831-11e5-7a69-971500188b19* id задачи.|

> Пример создания одного комментария к Задаче.

```shell
  curl -X POST
    "https://online.moysklad.ru/api/remap/1.2/entity/task/7944ef04-f831-11e5-7a69-971500188b19/notes"
    -H "Authorization: Basic <Access-Token>"
    -H "Content-Type: application/json"
      -d '{
            "text": "текст комментрания 3"
          }'  
```

> Response 200 (application/json)
Успешный запрос. Результат - JSON представление созданного комментария отдельной Задачи.

```json
[
  {
    "meta": {
      "href": "https://online.moysklad.ru/api/remap/1.2/entity/task/47bc1d9b-8b87-11e8-d9ce-84d900000017/notes/5528751f-8b8a-11e8-d9ce-84d90000000f",
      "type": "tasknote",
      "mediaType": "application/json"
    },
    "author": {
      "meta": {
        "href": "https://online.moysklad.ru/api/remap/1.2/entity/employee/98fa7086-8aa1-11e8-7210-075e0000002c",
        "metadataHref": "https://online.moysklad.ru/api/remap/1.2/entity/employee/metadata",
        "type": "employee",
        "mediaType": "application/json",
        "uuidHref": "https://online.moysklad.ru/app/#employee/edit?id=98fa7086-8aa1-11e8-7210-075e0000002c"
      }
    },
    "text": "текст комментрания 3",
    "moment": "2018-07-19 22:31:28"
  }
]
```

> Пример создания сразу нескольких комментариев к Задаче.

```shell
  curl -X POST
    "https://online.moysklad.ru/api/remap/1.2/entity/task/7944ef04-f831-11e5-7a69-971500188b19/notes"
    -H "Authorization: Basic <Access-Token>"
    -H "Content-Type: application/json"
      -d '[
            {
              "text": "текст комментрания 4"
            },
            {
              "text": "текст комментрания 5"
            }
          ]'  
```

> Response 200 (application/json)
Успешный запрос. Результат - JSON представление списка созданных комментариев отдельной Задачи.

```json
[
  {
    "meta": {
      "href": "https://online.moysklad.ru/api/remap/1.2/entity/task/47bc1d9b-8b87-11e8-d9ce-84d900000017/notes/8ba69d28-8b8a-11e8-d9ce-84d900000012",
      "type": "tasknote",
      "mediaType": "application/json"
    },
    "author": {
      "meta": {
        "href": "https://online.moysklad.ru/api/remap/1.2/entity/employee/98fa7086-8aa1-11e8-7210-075e0000002c",
        "metadataHref": "https://online.moysklad.ru/api/remap/1.2/entity/employee/metadata",
        "type": "employee",
        "mediaType": "application/json",
        "uuidHref": "https://online.moysklad.ru/app/#employee/edit?id=98fa7086-8aa1-11e8-7210-075e0000002c"
      }
    },
    "text": "текст комментрания 4",
    "moment": "2018-07-19 22:32:59"
  },
  {
    "meta": {
      "href": "https://online.moysklad.ru/api/remap/1.2/entity/task/47bc1d9b-8b87-11e8-d9ce-84d900000017/notes/8ba6a80c-8b8a-11e8-d9ce-84d900000013",
      "type": "tasknote",
      "mediaType": "application/json"
    },
    "author": {
      "meta": {
        "href": "https://online.moysklad.ru/api/remap/1.2/entity/employee/98fa7086-8aa1-11e8-7210-075e0000002c",
        "metadataHref": "https://online.moysklad.ru/api/remap/1.2/entity/employee/metadata",
        "type": "employee",
        "mediaType": "application/json",
        "uuidHref": "https://online.moysklad.ru/app/#employee/edit?id=98fa7086-8aa1-11e8-7210-075e0000002c"
      }
    },
    "text": "текст комментрания 5",
    "moment": "2018-07-19 22:32:59"
  }
]
```

### Комментарий к задаче 

### Получить комментарий к Задаче

Отдельный комментарий к Задаче с указанным id комментария.

**Параметры**

|Параметр   |Описание   | 
|---|---|
|id |  `string` (required) *Example: 7944ef04-f831-11e5-7a69-971500188b19* id задачи.|
|tasknoteID|  `string` (required) *Example: 34f6344f-015e-11e6-9464-e4de0000006c* id комментария к Задаче.|
 
> Запрос на получение отдельного комментарии к Задаче с указанным id.

```shell
curl -X GET
  "https://online.moysklad.ru/api/remap/1.2/entity/task/7944ef04-f831-11e5-7a69-971500188b19/notes/34f6344f-015e-11e6-9464-e4de0000006c"
  -H "Authorization: Basic <Access-Token>"
```

> Response 200 (application/json)
Успешный запрос. Результат - JSON представление отдельного комментария к Задаче.

```json
{
  "meta": {
    "href": "https://online.moysklad.ru/api/remap/1.2/entity/task/47bc1d9b-8b87-11e8-d9ce-84d900000017/notes/55eaa2cc-8b87-11e8-d9ce-84d900000025",
    "type": "tasknote",
    "mediaType": "application/json"
  },
  "author": {
    "meta": {
      "href": "https://online.moysklad.ru/api/remap/1.2/entity/employee/98fa7086-8aa1-11e8-7210-075e0000002c",
      "metadataHref": "https://online.moysklad.ru/api/remap/1.2/entity/employee/metadata",
      "type": "employee",
      "mediaType": "application/json",
      "uuidHref": "https://online.moysklad.ru/app/#employee/edit?id=98fa7086-8aa1-11e8-7210-075e0000002c"
    }
  },
  "text": "текст комментария 1",
  "moment": "2018-07-19 22:10:01"
}
```

### Изменить комментарий к Задаче 
Запрос на обновление отдельной комментарии к Задаче.
Для успешного создания необходимо в теле запроса указать следующие поля:
+ **text** - Текст комментария. `Необходимое`

**Параметры**

|Параметр   |Описание   | 
|---|---|
|id |  `string` (required) *Example: 7944ef04-f831-11e5-7a69-971500188b19* id задачи.|
|tasknoteID|  `string` (required) *Example: 34f6344f-015e-11e6-9464-e4de0000006c* id комментария к Задаче.|

> Пример запроса на обновление отдельного комментария к Задаче.

```shell
  curl -X PUT
    "https://online.moysklad.ru/api/remap/1.2/entity/task/7944ef04-f831-11e5-7a69-971500188b19/notes/34f6344f-015e-11e6-9464-e4de0000006c"
    -H "Authorization: Basic <Access-Token>"
    -H "Content-Type: application/json"
      -d '{
            "text": "новый текст комментария 1"
          }'  
```

> Response 200 (application/json)
Успешный запрос. Результат - JSON представление обновлённого комментария к Задаче.

```json
{
  "meta": {
    "href": "https://online.moysklad.ru/api/remap/1.2/entity/task/47bc1d9b-8b87-11e8-d9ce-84d900000017/notes/55eaa2cc-8b87-11e8-d9ce-84d900000025",
    "type": "tasknote",
    "mediaType": "application/json"
  },
  "author": {
    "meta": {
      "href": "https://online.moysklad.ru/api/remap/1.2/entity/employee/98fa7086-8aa1-11e8-7210-075e0000002c",
      "metadataHref": "https://online.moysklad.ru/api/remap/1.2/entity/employee/metadata",
      "type": "employee",
      "mediaType": "application/json",
      "uuidHref": "https://online.moysklad.ru/app/#employee/edit?id=98fa7086-8aa1-11e8-7210-075e0000002c"
    }
  },
  "text": "новый текст комментария 1",
  "moment": "2018-07-19 22:10:01"
}
```

### Удалить комментарий

**Параметры**

|Параметр   |Описание   | 
|---|---|
|id |  `string` (required) *Example: 7944ef04-f831-11e5-7a69-971500188b19* id задачи.|
|tasknoteID|  `string` (required) *Example: 34f6344f-015e-11e6-9464-e4de0000006c* id комментария к Задаче.|
 
> Запрос на удаление отдельного комментария к Задаче с указанным id.

```shell
curl -X DELETE
  "https://online.moysklad.ru/api/remap/1.2/entity/task/7944ef04-f831-11e5-7a69-971500188b19/notes/34f6344f-015e-11e6-9464-e4de0000006c"
  -H "Authorization: Basic <Access-Token>"
```

> Response 200 (application/json)
Успешное удаление комментария к Задаче.
