<!-- include(metadata.apib) -->

# Комплект
## Комплекты [/entity/bundle]
Средствами JSON API можно создавать и обновлять сведения о Комплектах, запрашивать списки Комплектов и сведения по отдельным Комплектам. Кодом сущности для Комплекта в составе JSON API является ключевое слово **bundle**.
### Атрибуты сущности
+ **meta** - [Метаданные](/api/remap/1.2/doc/index.html#header-метаданные) объекта
+ **id** - ID Комплекта в формате UUID `Только для чтения`
+ **accountId** - ID учетной записи `Только для чтения`
+ **owner** - Ссылка на Владельца (Сотрудника) в формате [Метаданных](/api/remap/1.2/doc/index.html#header-метаданные)
+ **shared** - Общий доступ
+ **group** - Отдел сотрудника в формате [Метаданных](/api/remap/1.2/doc/index.html#header-метаданные)
+ **syncId** - ID синхронизации. После заполнения недоступен для изменения.
+ **updated** - Момент последнего обновления сущности `Только для чтения`
+ **name** - Наименование Комплекта `Необходимое`
+ **description** - Описание Комплекта
+ **code** - Код Комплекта
+ **externalCode** - Внешний код Комплекта
+ **archived** - Отметка о том, добавлен ли Комплект в архив
+ **pathName** - Наименование группы, в которую входит Комплект `Только для чтения`
+ **vat** - НДС %
+ **effectiveVat** - Реальный НДС % `Только для чтения`
+ **productFolder** - Ссылка на группу Комплекта
+ **uom** - Единицы измерения
+ **image** - Изображение Комплекта
+ **minPrice** - Минимальная цена
+ **salePrices** - Цены продажи
+ **attributes** - Коллекция доп. полей в формате [Метаданных](/api/remap/1.2/doc/index.html#header-метаданные)
+ **country** - Ссылка на страну в формате [Метаданных](/api/remap/1.2/doc/index.html#header-метаданные)
+ **article** - Артикул
+ **weight** - Вес
+ **volume** - Объём
+ **barcodes** - Массив штрихкодов Комплекта
+ **overhead** - Дополнительные расходы
  - **currency** - Валюта доп расходов
  - **value** - Значение доп расходов
+ **components** - Компоненты Комплекта

#### Минимальная цена
+ **value** - Значение цены
+ **currency** - Ссылка на валюту в формате [Метаданных](/api/remap/1.2/doc/index.html#header-метаданные)

### Компоненты Комплекта
Компоненты Комплекта - это список товаров/услуг/модификаций, который входят в состав комплекта. Компонентов у комплекта может быть от 1 до 50.
Объект компонента Комплекта содержит следующие поля:
+ **id** - ID компонента в формате UUID `Только для чтения`
+ **accountId** - ID учетной записи `Только для чтения`
+ **quantity** - Количество товаров/услуг данного вида в компоненте. Максимальное количество знаков 12, округляется до 4х знаков после запятой.
+ **assortment** - Ссылка на товар/услугу/серию, которую представляет собой компонент.


### Комплект как позиция документа
Комплект может выступать в роли позиции документа. Он также как и товары, услуги и модификации может быть передан в составе позиции в формате метаданных.<br>
Вот некоторые ограничения, связанные с использованием комплектов как позиций:
+ Количество комплектов должно быть целым числом
+ Комплекты не могут быть позициями в следующих типах документов:
  - Заказы поставщикам
  - Счета поставщиков
  - Приемки
  - Возвраты поставщикам
  - Выданные отчеты комиссионера
  - Списания
  - Оприходования
  - Перемещения
  - Инвентаризации
  - Тех. карты
  - Тех. операции
  - Внутренние заказы
+ Комплект не может быть позицией отгрузки по комиссионному договору:
  - нельзя добавить комплект в отгрузку по комиссионному договору
  - нельзя установить комиссионный договор в отгрузке с комплектами
  - нельзя изменить договор на комиссионный, если по нему есть отгрузки с комплектами


### Атрибуты вложенных сущностей
#### Метаданные Комплектов
Метаданные Комплектов содержат информацию о дополнительных полях.

Посмотреть все созданные в основном интерфейсе доп. поля Комплектов,
а также все типы цен можно с помощью запроса на получение метаданных Комплектов.
Ответ - объект, со следующей структурой:
+ **meta** - Метаданные
+ **attributes** - коллекция всех существующих доп. полей Комплектов в формате [Метаданных](/api/remap/1.2/doc/index.html#header-метаданные)

Структуры объектов отдельных коллекций:

#### Штрих коды:
При создании штрихкода требуется описать объект с полем, являющимся форматом представления штрихкода в нижнем регистре, со стороковым значением самого штрихкода. Наименование полей отдельного объекта, представляющего штрихкод:
+ **ean13** - штрихкод в формате EAN13, если требуется создать штрихкод в формате EAN13
+ **ean8** - штрихкод в формате EAN8, если требуется создать штрихкод в формате EAN8
+ **code128** - штрихкод в формате Code128, если требуется создать штрихкод в формате Code128

#### Изображение: структура и загрузка.
Структура поля **image**, которое вы получите при запросе Комплекта с изображением:
+ **meta** - Метаданные об изображении
+ **title** - Название изображения
+ **filename** - Имя файла
+ **size** - Размер файла в байтах
+ **updated** - Дата последнего изменения
+ **miniature** - Ссылка на миниатюру изображения в формате Метаданных
+ **tiny** - Ссылка на уменьшенное изображение в формате Метаданных

<h4>Загрузка</h4>
Для загрузки изображения нужно в теле запроса на [создание](#комплект-комплекты-post) или [обновление](#комплект-комплект-put) Комплекта
указать поле **image** со следующими атрибутами:
+ **filename** - имя файла с расширением. Например - "банан.png"
+ **content** - Изображение, закодированное в формате Base64.

Если в запросе на обновление не будет полей **filename** и **content**, то весь объект **image**, если он присутствует в Body,
будет проигнорирован, т.к. сервер посчитает, что его обновление не требуется.

О работе с доп. полями Комплектов можно прочитать [здесь](/api/remap/1.2/doc/index.html#header-работа-с-дополнительными-полями)

### Получить список комплектов [GET]
Запрос на получение всех комплектов для данной учётной записи.
Результат: Объект JSON, включающий в себя поля:
- **meta** [Метаданные](/api/remap/1.2/doc/index.html#header-метаданные) о выдаче,
- **context** - [Метаданные](/api/remap/1.2/doc/index.html#header-метаданные) о сотруднике, выполнившем запрос.
- **rows** - Массив JSON объектов, представляющих собой комплекты.

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
Успешный запрос. Результат - JSON представление списка комплектов.
  + Body
        <!-- include(body/bundle/get_list_response.json) -->

### Создать Комплект [POST]
Создать новый комплект.
### Описание
Комплект создаётся на основе переданного объекта JSON,
который содержит представление нового Комплекта.
Результат - JSON представление созданного Комплекта. Для создания нового Комплекта
необходимо и достаточно указать в переданном объекте не пустое поле `name` и не пустое множество компонентов `components`.
Максимальное число компонентов в Комплекте - 50.

+ Request Пример (application/json)
Пример наиболее полного по количеству полей запроса.
  + Body
        <!-- include(body/bundle/post_request.json) -->

+ Response 200 (application/json)
Успешный запрос. Результат - JSON представление созданного Комплекта.
  + Body
        <!-- include(body/bundle/post_response.json) -->

+ Request Пример Комплект с изображением (application/json)
Пример запроса на создание Комплекта с загрузкой изображения
  + Body
        <!-- include(body/bundle/post_with_image_request.json) -->

+ Response 200 (application/json)
Успешный запрос. Результат - JSON представление созданного Комплекта.
  + Body
        <!-- include(body/bundle/post_with_image_response.json) -->


### Массовое создание и обновление Комплектов [POST]
[Массовое создание и обновление](/api/remap/1.2/doc/index.html#header-создание-и-обновление-нескольких-объектов) Комплектов.
В теле запроса нужно передать массив, содержащий JSON представления Комплектов, которые вы хотите создать или обновить.
Обновляемые Комплекты должны содержать идентификатор в виде метаданных.

+ Request Пример (application/json)
Пример создания и обновления нескольких Комплектов
  + Body
        <!-- include(body/bundle/post_massive_request.json) -->

+ Response 200 (application/json)
Успешный запрос. Результат - массив JSON представлений созданных и обновленных Комплектов.
  + Body
        <!-- include(body/bundle/post_massive_response.json) -->


## Метаданные Комплектов [/entity/bundle/metadata]
### Метаданные Комплектов [GET]
Запрос на получение метаданных Комплектов. Результат - объект JSON, включающий в себя:
+ **meta** - Метаданные
+ **attributes** - коллекция всех существующих доп. полей Комплектов в формате [Метаданных](/api/remap/1.2/doc/index.html#header-метаданные)

Структура отдельного объекта, представляющего доп. поле подробно описана в разделе [Работа с дополнительными полями](/api/remap/1.2/doc/index.html#header-работа-с-дополнительными-полями).
+ Response 200 (application/json)
Успешный запрос. Результат - JSON представление доп. полей Комплектов.
  + Body
        <!-- include(body/bundle/get_metadata.json) -->

## Отдельное доп. поле [/entity/bundle/metadata/attributes/{id}]
+ Parameters
  + id: `7944ef04-f831-11e5-7a69-971500188b19` (required, string) - id Доп. поля
### Отдельное доп. поле [GET]
Запрос на получение информации по отдельному дополнительному полю.
+ Response 200 (application/json)
Успешный запрос. Результат - JSON представление отдельного доп. поля.
  + Body
        <!-- include(body/bundle/metadata_by_id.json) -->

## Комплект [/entity/bundle/{id}]
+ Parameters
  + id: `7944ef04-f831-11e5-7a69-971500188b19` (required, string) - id Комплекта

### Получить Комплект [GET]
Запрос на получение комплекта с указанным id.

+ Response 200 (application/json)
Успешный запрос. Результат - JSON представление комплекта.
  + Body
        <!-- include(body/bundle/get_by_id_response.json) -->

### Изменить Комплект [PUT]
Запрос на обновление существующего Комплекта.
Типы цен в ценах продаж, дополнительные поля и компоненты обновляются как элементы вложенных коллекций:
+ Если в текущем объекте у какого-то из доп. полей / типов цен / компонентов нет значения,
а в переданной коллекции оно есть - значение записывается в доп. поле / тип цены / компонент.
+ Если же у данного атрибута значение уже присутствует - оно перезаписывается на переданное.
+ Если у данного атрибута в составе объекта значение присутствует, однако оно отсутствует
в передаваемой в теле запроса коллекции (не передаётся совсем), то значение атрибута объекта не изменяется.

+ Request Пример (application/json)
Пример запроса на обновление Комплекта
  + Body
        <!-- include(body/bundle/put_request.json) -->

+ Response 200 (application/json)
Успешный запрос. Результат - JSON представление обновлённого Комплекта.
  + Body
        <!-- include(body/bundle/put_response.json) -->

### Удалить Комплект [DELETE]
+ Parameters
  + id: `7944ef04-f831-11e5-7a69-971500188b19` (required, string) - id Комплекта

Запрос на удаление Комплекта с указанным id.

+ Response 200 (application/json)
Успешное удаление Комплекта.

## Компоненты Комплекта [/entity/bundle/{id}/components]
Отдельный ресурс для управления компонентами Комплекта.
+ Parameters
  + id: `7944ef04-f831-11e5-7a69-971500188b19` (required, string) - id Комплекта

### Получить компоненты Комплекта [GET]
Запрос на получение списка всех компонентов данного Комплекта.
- **meta** [Метаданные](/api/remap/1.2/doc/index.html#header-метаданные) о выдаче,
- **context** - [Метаданные](/api/remap/1.2/doc/index.html#header-метаданные) о сотруднике, выполнившем запрос.
- **rows** - Массив JSON объектов, представляющих собой компоненты Комплекта.

+ Parameters
  + limit: 1000 (optional, enum[number])
  Максимальное количество сущностей для извлечения.
  <p>
    <code>Допустимые значения 1 - 1000</code>
  </p>
      + Default: `1000`
  + offset: 5 (optional, number)
    Отступ в выдаваемом списке сущностей
      + Default: `0`
+ Response 200 (application/json)
Успешный запрос. Результат - JSON представление списка компонентов отдельного Комплекта.
  + Body
        <!-- include(body/bundle/components_get_list.json) -->

### Добавить компонент Комплекта [POST]
+ Request Пример с одним компонентом (application/json)
Запрос на добавление компонента Комплекта.
  + Body
        <!-- include(body/bundle/post_component_request.json) -->

+ Response 200 (application/json)
Успешный запрос. Результат - JSON представление добавленного компонента.
  + Body
        <!-- include(body/bundle/post_component_response.json) -->

## Компонент Комплекта [/entity/bundle/{id}/components/{componentID}]
+ Parameters
  + id: `7944ef04-f831-11e5-7a69-971500188b19` (required, string) - id Комплекта
  + componentID: `34f6344f-015e-11e6-9464-e4de0000006c` (required, string) - id компонента Комплекта

### Получить компонент [GET]
Запрос на получение отдельного компонента Комплекта с указанным id.
+ Response 200 (application/json)
Успешный запрос. Результат - JSON представление отдельного компонента Комплекта.
  + Body
        <!-- include(body/bundle/component_get_by_id.json) -->

### Обновить компонент [PUT]
Запрос на обновление отдельного компонента Комплекта с указанным id.

+ Request Пример (application/json)
Пример запроса на обновление компонента Комплекта
  + Body
        <!-- include(body/bundle/component_put_request.json) -->

+ Response 200 (application/json)
Успешный запрос. Результат - JSON представление отдельного компонента Комплекта.
  + Body
        <!-- include(body/bundle/component_put_response.json) -->

### Удалить компонент [DELETE]
Запрос на удаление отдельного компонента Комплекта с указанным id.

+ Response 200 (application/json)
Успешное удаление компонента Комплекта.
