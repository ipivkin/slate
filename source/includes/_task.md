<!-- include(metadata.apib) -->

# Задача
Средствами JSON API можно создавать и обновлять сведения о задачах, запрашивать списки задач и сведения по отдельным задачам. Кодом сущности для задачи в составе JSON API является ключевое слово **task**. Больше о задачах и работе с ними в основном интерфейсе вы можете прочитать в нашей службе поддержки по  [этой ссылке](https://support.moysklad.ru/hc/ru/articles/203392263-%D0%97%D0%B0%D0%B4%D0%B0%D1%87%D0%B8).
## Задачи [/entity/task]
### Атрибуты сущности
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


### Комментарии задачи
Объект комментария к задаче содержит следующие поля:
+ **author** - Сотрудник создавший комментарий. В формате [Метаданных](/api/remap/1.2/doc/index.html#header-метаданные) `Только для чтения`
+ **moment** - Момент создания комментария `Только для чтения`
+ **description** - Текст комментария `Необходимое`


### Отображение списка по умолчанию
#### Для администратора
Если текущий сотрудник обладает правами администратора, то при запросе списка задач ему будут выведены все активные  (**done** = false) задачи, как те,
 что относятся к нему (сотрудник является создателем или исполнителем задачи), так и те, что относятся к другим сотрудникам.
#### Для сотрудника
Для сотрудника, не являющегося администратором, но имеющего право на просмотр всех задач, список задач по умолчанию будет аналогичен списку задач, выводимому для администратора. В противном случае, при запросе списка задач без каких-либо фильтров, будут выведены активные (**done** = false) задачи, которые создал текущий сотрудник и задачи, за которые ответственен текущий сотрудник.

### Фильтры из web-интерфейса
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

### Права доступа
| Операция                                                                         | Доступ                    |
| :------------------------------------------------------------------------------: |:--------------------------|
| Просмотр задач в которых текущий пользователь является ответственным или автором |	Все                      |
| Просмотр любых задач                                                             |	пользователь с правом на просмотр всех задач или администратор |
| Создание задачи	                                                                 |  необходима тарифная опция CRM, все
| Редактирование задачи                                                            |	необходима тарифная опция CRM, администратор, создатель задачи |
| Выполнение задачи	                                                               |  необходима тарифная опция CRM, администратор, создатель задачи, ответственный |
| Отмена выполнения задачи                                                         |	необходима тарифная опция CRM, администратор, создатель задачи, ответственный |
| Удаление задачи	                                                                 |  Администратор, создатель задачи                                                |



### Получить задачи [GET]
Получить список задач.
Результат: Объект JSON, включающий в себя поля:
- **meta** [Метаданные](/api/remap/1.2/doc/index.html#header-метаданные) о выдаче,
- **context** - [Метаданные](/api/remap/1.2/doc/index.html#header-метаданные) о сотруднике, выполнившем запрос.
- **rows** - Массив JSON объектов, представляющих собой [Задачи](#задача).
+ Parameters
  + limit: 1000 (optional, enum[number])
  Максимальное количество сущностей для извлечения.
  <p>
    <code>Допустимые значения 1 - 1000</code>
  </p>
      + Default: `1000`
  + offset: 40 (optional, number)
    Отступ в выдаваемом списке сущностей
      + Default: `0`

+ Response 200 (application/json)
Успешный запрос. Результат - JSON представление списка задач.
  + Body
        <!-- include(body/task/get_list.json) -->

### Создать Задачу [POST]
Создать новую задачу. Для создания новых задач необходима активная тарифная опция [CRM](https://support.moysklad.ru/hc/ru/articles/216649748-CRM-%D0%A0%D0%B0%D0%B1%D0%BE%D1%82%D0%B0-%D1%81-%D0%BA%D0%BB%D0%B8%D0%B5%D0%BD%D1%82%D1%81%D0%BA%D0%BE%D0%B9-%D0%B1%D0%B0%D0%B7%D0%BE%D0%B9).

+ Request Пример (application/json)
Пример запроса на создание новой задачи.
  + Body
        <!-- include(body/task/post_request.json) -->
+ Response 200 (application/json)
Успешный запрос. Результат - JSON представление созданной задачи.
  + Body
        <!-- include(body/task/post_response.json) -->

### Массовое создание и обновление Задач [POST]
[Массовое создание и обновление](/api/remap/1.2/doc/index.html#header-создание-и-обновление-нескольких-объектов) Задач.
В теле запроса нужно передать массив, содержащий JSON представления Задач, которые вы хотите создать или обновить.
Обновляемые Задачи должны содержать идентификатор в виде метаданных.

+ Request Пример (application/json)
Пример создания и обновления нескольких Задач
  + Body
        <!-- include(body/task/post_massive_request.json) -->

+ Response 200 (application/json)
Успешный запрос. Результат - массив JSON представлений созданных и обновленных Задач.
  + Body
        <!-- include(body/task/post_massive_response.json) -->

### Удалить задачу [DELETE /entity/task/{id}]
+ Parameters
  + id: `7944ef04-f831-11e5-7a69-971500188b19` (required, string) - id задачи

Запрос на удаление задачи с указанным id. Для удаления задач необходима активная тарифная опция [CRM](https://support.moysklad.ru/hc/ru/articles/216649748-CRM-%D0%A0%D0%B0%D0%B1%D0%BE%D1%82%D0%B0-%D1%81-%D0%BA%D0%BB%D0%B8%D0%B5%D0%BD%D1%82%D1%81%D0%BA%D0%BE%D0%B9-%D0%B1%D0%B0%D0%B7%D0%BE%D0%B9).
Также нельзя удалить задачи, созданные другими сотрудниками, без прав администратора.

+ Response 200 (application/json)
Успешное удаление задачи.


## Задача [/entity/task/{id}]
+ Parameters
  + id: `7944ef04-f831-11e5-7a69-971500188b19` (required, string) - id задачи.
### Получить задачу [GET]
Запрос на получение отдельной задачи с указанным id.
+ Response 200 (application/json)
Успешный запрос. Результат - JSON представление задачи с указанным id.
  + Body
        <!-- include(body/task/get_by_id.json) -->

### Изменить задачу [PUT]
### Описание
Изменить задачу с указанным id. Для изменения задач необходима активная тарифная опция [CRM](https://support.moysklad.ru/hc/ru/articles/216649748-CRM-%D0%A0%D0%B0%D0%B1%D0%BE%D1%82%D0%B0-%D1%81-%D0%BA%D0%BB%D0%B8%D0%B5%D0%BD%D1%82%D1%81%D0%BA%D0%BE%D0%B9-%D0%B1%D0%B0%D0%B7%D0%BE%D0%B9).
Также нельзя изменять задачи, созданные другими сотрудниками, без прав администратора.

+ Request Пример (application/json)
Пример запроса на обновление существующей задачи.
  + Body
        <!-- include(body/task/put_request.json) -->
+ Response 200 (application/json)
Успешный запрос. Результат - JSON представление обновлённой задачи.
  + Body
        <!-- include(body/task/put_response.json) -->


## Комментарии Задачи [/entity/task/{id}/notes]
Отдельный ресурс для управления комментариями Задачи. С его помощью вы можете управлять комментариями задачи, в которой количество комментариев превышает лимит на количество комментариев, сохраняемых вместе с задачей. Этот лимит равен 100.
+ Parameters
  + id: `7944ef04-f831-11e5-7a69-971500188b19` (required, string) - id Задачи

### Получить комментарии Задачи [GET]
Запрос на получение списка всех комментариев данной Задачи.
- **meta** [Метаданные](#header-метаданные) о выдаче,
- **context** - [Метаданные](#header-метаданные) о сотруднике, выполнившем запрос.
- **rows** - Массив JSON объектов, представляющих собой комментарии Задачи.

+ Parameters
  + limit: 100 (optional, enum[number])
  Максимальное количество сущностей для извлечения.
  <p>
    <code>Допустимые значения 1 - 100</code>
  </p>
      + Default: `25`
  + offset: 40 (optional, number)
    Отступ в выдаваемом списке сущностей
      + Default: `0`

  + updatedFrom: `2016-04-15 15:48:46` (optional, string)
    Один из [параметров фильтрации выборки](#header-параметры-фильтрации-выборки).
    Формат строки : `ГГГГ-ММ-ДД ЧЧ:ММ:СС[.ммм]`, Часовой пояс: `MSK` (Московское время)

  + updatedTo: `2016-04-15 15:48:46` (optional, string)
    Один из [параметров фильтрации выборки](#header-параметры-фильтрации-выборки).
    Формат строки : `ГГГГ-ММ-ДД ЧЧ:ММ:СС[.ммм]`, Часовой пояс: `MSK` (Московское время)

  + updatedBy: `admin@admin` (optional, string)
    Один из [параметров фильтрации выборки](#header-параметры-фильтрации-выборки).
    Формат строки : `uid`

+ Response 200 (application/json)
Успешный запрос. Результат - JSON представление списка комментариев отдельной Задачи.
  + Body
        <!-- include(body/task/tasknotes_get_list.json) -->

### Создать комментарий Задачи [POST]
Запрос на создание нового комментария к Задаче.
Для успешного создания необходимо в теле запроса указать следующие поля:
+ **text** - Текст комментария. `Необходимое`

+ Request Пример с одним комментарием (application/json)
Пример создания одного комментария к Задаче.
  + Body
        <!-- include(body/task/tasknotes_post_one_request.json) -->
+ Response 200 (application/json)
Успешный запрос. Результат - JSON представление созданного комментария отдельной Задачи.
  + Body
        <!-- include(body/task/tasknotes_post_one_response.json) -->

+ Request Пример с несколькими комментариями (application/json)
Пример создания сразу нескольких комментариев к Задаче.
  + Body
        <!-- include(body/task/tasknotes_post_some_request.json) -->

+ Response 200 (application/json)
Успешный запрос. Результат - JSON представление списка созданных комментариев отдельной Задачи.
  + Body
        <!-- include(body/task/tasknotes_post_some_response.json) -->

## Комментарий к задаче [/entity/task/{id}/notes/{tasknoteID}]
Отдельный комментарий к Задаче с указанным id комментария.
+ Parameters
  + id: `7944ef04-f831-11e5-7a69-971500188b19` (required, string) - id Задачи
  + tasknoteID: `34f6344f-015e-11e6-9464-e4de0000006c` (required, string) - id комментария к Задаче

### Получить комментарий к Задаче [GET]
Запрос на получение отдельного комментарии к Задаче с указанным id.
+ Response 200 (application/json)
Успешный запрос. Результат - JSON представление отдельного комментария к Задаче.
  + Body
        <!-- include(body/task/tasknote_get_by_id.json) -->

### Изменить комментарий к Задаче [PUT]
Запрос на обновление отдельной комментарии к Задаче.
Для успешного создания необходимо в теле запроса указать следующие поля:
+ **text** - Текст комментария. `Необходимое`

+ Request Пример (application/json)
Пример запроса на обновление отдельного комментария к Задаче.
  + Body
        <!-- include(body/task/tasknote_put_request.json) -->

+ Response 200 (application/json)
Успешный запрос. Результат - JSON представление обновлённого комментария к Задаче.
  + Body
        <!-- include(body/task/tasknote_put_response.json) -->

### Удалить комментарий [DELETE]
Запрос на удаление отдельного комментария к Задаче с указанным id.

+ Response 200 (application/json)
Успешное удаление комментария к Задаче.
