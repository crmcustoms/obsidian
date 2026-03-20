# REST API — Контакты (XML API)

← [[PlanFix API — Index]]

Функции XML API для управления контактами.
Auth: см. [[REST API — Авторизация]]

---

## Список функций

## Перейти

# ПланФикс API: Контакты

Список функций для управления контактами в ПланФиксе

01. contact.add / Добавление контакта
02. contact.update / Обновление данных контакта
03. contact.updateCustomData / Обновление данных пользовательских полей контакта
04. contact.get / Получить информацию
05. contact.getList / Получить список контактов
06. contact.managePlanfixAccess / Разрешить/запретить доступ в ПланФикс
07. contact.updateUserInfo / Обновить информацию пользователя
08. contact.updateContractors / Изменить информацию о принадлежности контакта к компании
09. contact.getPhoneTypes/ Получить список типов телефонных номеров
10. contact.getGroupList / Получить список доступных групп контактов в ПланФикс
11. contact.delete / Удалить контакт

## Перейти

Список функций

---

## contact.add — Добавление контакта

## Перейти

# ПланФикс API contact.add

Функция добавления контакта. Формат запроса:

```
<?xml version="1.0" encoding="UTF-8"?>
<request method="contact.add">
  <account></account>
  <sid></sid>
  <contact>
    <template></template>
    <name></name>
    <midName></midName>
    <lastName></lastName>
    <post></post>
    <email></email>
    <phones>
        <phone>
            <number></number>
            <typeId></typeId>
            <typeName></typeName>
        </phone>
        <!-- ... -->
    </phones>
    <secondaryEmails>
        <email></email>
        <!-- ... -->
    </secondaryEmails>
    <address></address>
    <description></description>
    <sex></sex>
    <site></site>
    <skype></skype>
    <facebook></facebook>
    <vk></vk>
    <icq></icq>
    <birthdate></birthdate>
    <lang></lang>
    <isCompany></isCompany>
    <canBeWorker></canBeWorker>
    <canBeClient></canBeClient>
    <group>
      <id></id>
    </group>
    <customData>
      <customValue>
        <id></id>
        <value></value>
      </customValue>
      <!-- ... -->
    </customData>
    <responsible>
      <users>
        <id></id>
        <id></id>
        <!-- ... -->
      </users>
      <groups>
        <id></id>
        <id></id>
        <!-- ... -->
      </groups>
    </responsible>
  </contact>
  <signature></signature>
</request>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| template | int | номер шаблона контакта, (general в результатах contact.getList) |  |
| Реализация создания контакта по шаблону в API на текущий момент неполна.<br>Из шаблона устанавливается форма создания контакта (и соответственно имеющиеся в контакте пользовательские поля) и часть её свойств. <br>Из шаблона на текущий момент не устанавливаются: аналитики, файлы, чек-листы, напоминания |
| name | string | Имя |  |
| midName | string | Отчество |  |
| lastName | string | Фамилия |  |
| post | string | Должность |  |
| email | string | адрес электронной почты |  |
| phones | string | телефоны |  |
| phone.number | string | номер телефона |  |
| phone.typeId | int | идентификатор типа номера | допустимые значения можно получить функцией contact.getPhoneTypes |
| phone.typeName | string | название типа номера |  |
| secondaryEmails | string | дополнительные адреса email |  |
| secondaryEmails.email | string | email |  |
| address | string | Адрес |  |
| description | string | Дополнительная информация |  |
| sex | enum | пол | допустимые значения смотри в разделе пол клиента |
| site | string | веб-сайт |  |
| skype | string | skype-контакт |  |
| facebook | string | facebook |  |
| vk | string | вконтакте |  |
| icq | string | номер-icq |  |
| birthdate | DateTime | дата рождения |  |
| lang | string | язык: Ru, En |  |
| isCompany | boolean | является компанией |  |
| canBeWorker | boolean | отображается в списке участников задачи |  |
| canBeClient | boolean | отображается в списке контрагентов задачи |  |
| group.id | int | идентификатор группы контактов | допустимые значения можно получить функцией contact.getGroupList |
| customData |  | значения пользовательских полей контакта |  |
| customData.customValue.id |  | идентификатор пользовательского поля контакта | можно получив вызвав contact.get шаблона, по которому создается контакт |
| customData.customValue.value |  | значение пользовательского поля контакта | (для полей типа набор задач, список сотрудников, набор записей справочника \- идентификаторы через запятую в квадратных скобках) |
| responsible |  | корневой элемент списка ответственных |  |
| responsible .users |  | корневой элемент списка ответственных пользователей |  |
| responsible .users.id | int | идентификатор пользователя |  |
| responsible .groups |  | корневой элемент списка групп ответственных |  |
| responsible .groups.id | int | идентификатор группы |  |
| signature | string(32) | подпись |  |

Результат успешного выполнения функции:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="ok">
  <contact>
    <id></id>
    <userid></userid>
    <general></general>
  </contact>
  <actionid></actionid>
</response>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| contact.id | int | идентификатор добавленного контакта |  |
| contact.userid | int | идентификатор контакта для случаев, когда он используется в системе наравне с сотрудниками (исполнитель задачи и т.п.) |  |
| contact.general | int | номер добавленного контакта |  |
| actionid | int | идентификатор действия создания контакта |  |

В противном случае будет возвращен ответ с ошибкой:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="error">
  <code></code>
</response>
```

## Перейти

---

## contact.update — Обновление данных контакта

## Перейти

# ПланФикс API contact.update

Функция обновления информации о клиенте. Формат запроса:

```
<?xml version="1.0" encoding="UTF-8"?>
<request method="contact.update">
  <account></account>
  <sid></sid>
  <silent></silent>
  <contact>
    <id></id>
    <userid></userid>
    <general></general>
    <name></name>
    <midName></midName>
    <lastName></lastName>
    <template></template>
    <post></post>
    <email></email>
    <phones>
        <phone>
            <number></number>
            <typeId></typeId>
            <typeName></typeName>
        </phone>
        <!-- ... -->
    </phones>
    <secondaryEmails>
        <email></email>
        <!-- ... -->
    </secondaryEmails>
    <address></address>
    <description></description>
    <sex></sex>
    <site></site>
    <skype></skype>
    <facebook></facebook>
    <vk></vk>
    <icq></icq>
    <birthdate></birthdate>
    <lang></lang>
    <canBeWorker></canBeWorker>
    <canBeClient></canBeClient>
    <group>
      <id></id>
    </group>
    <customData>
      <customValue>
        <id></id>
        <value></value>
      </customValue>
      <!-- ... -->
    </customData>
    <responsible>
      <users>
        <id></id>
        <id></id>
        <!-- ... -->
      </users>
      <groups>
        <id></id>
        <id></id>
        <!-- ... -->
      </groups>
    </responsible>
    <telegram>
      <id></id>
      <username></username>
    </telegram>
  </contact>
</request>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| silent | bool | при значении 1 - об изменении не рассылаются уведомления, не создаются действия и записи в логе контакта | обязательно значение 1 при массовых периодических обновлениях контактов |
| id | int | идентификатор обновляемого контакта |  |
| userid | int | идентификатор контакта, если он взят из данных, когда он используется в системе наравне с сотрудниками (исполнитель задачи и т.п., а также пользовательское поле типа контакт) или из переменных {{Контакт.Идентификатор}}, {{Задача.Контрагент.Идентификатор}} и им подобных |  |
| general | int | номер обновляемого контакта (используется если не задан id) |  |
| name | string | Имя |  |
| midName | string | Отчество |  |
| lastName | string | Фамилия |  |
| template | int | номер шаблона контакта, (general в результатах contact.getList) | необязательный, при отсутствии не изменяется |
| post | string | Должность |  |
| email | string | адрес электронной почты |  |
| phones | string | телефоны |  |
| phone.number | string | номер телефона |  |
| phone.typeId | int | идентификатор типа номера | допустимые значения можно получить функцией contact.getPhoneTypes |
| phone.typeName | string | название типа номера |  |
| secondaryEmails | string | дополнительные адреса email |  |
| secondaryEmails.email | string | email |  |
| address | string | Адрес |  |
| description | string | Дополнительная информация |  |
| sex | enum | пол | допустимые значения смотри в разделе пол клиента |
| site | string | веб-сайт |  |
| skype | string | skype-контакт |  |
| facebook | string | facebook |  |
| vk | string | вконтакте |  |
| icq | string | номер-icq |  |
| birthdate | DateTime | дата рождения |  |
| lang | string | язык: Ru, En |  |
| canBeWorker | boolean | отображается в списке участников задачи |  |
| canBeClient | boolean | отображается в списке контрагентов задачи |  |
| group.id | int | идентификатор группы контактов | допустимые значения можно получить функцией contact.getGroupList |
| customData |  | значения пользовательских полей контакта |  |
| customData.customValue.id |  | идентификатор пользовательского поля контакта |  |
| customData.customValue.value |  | значение пользовательского поля контакта | (для полей типа набор задач, список сотрудников, набор записей справочника \- идентификаторы через запятую в квадратных скобках) |
| responsible |  | корневой элемент списка ответственных |  |
| responsible .users |  | корневой элемент списка ответственных пользователей |  |
| responsible .users.id | int | идентификатор пользователя |  |
| responsible .groups |  | корневой элемент списка групп ответственных |  |
| responsible .groups.id | int | идентификатор группы |  |
| telegram.id | int | внутренний идентификатор в Telegram |  |
| telegram.username | string | username в Telegram |  |

Результат успешного выполнения функции:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="ok">
  <contact>
    <id></id>
    <general></general>
  </contact>
  <actionid></actionid>
</response>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| contact.id | int | идентификатор обновляемого контакта |  |
| contact.general | int | номер добавленного контакта |  |
| actionid | int | идентификатор действия об изменении |  |

В противном случае будет возвращен ответ с ошибкой:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="error">
  <code></code>
</response>
```

## Перейти

---

## contact.updateCustomData — Обновление пользовательских полей

## Перейти

# ПланФикс API contact.updateCustomData

Функция обновления пользовательских полей контакта. Может использоваться, если у пользователя нет прав на изменение контакта и поэтому использовать функцию contact.update он не может, но при этом есть права на изменение пользовательских полей.

Формат запроса:

```
<?xml version="1.0" encoding="UTF-8"?>
<request method="contact.updateCustomData">
  <account></account>
  <sid></sid>
  <contact>
    <id></id>
    <general></general>
    <customData>
      <customValue>
        <id></id>
        <value></value>
      </customValue>
      <!-- ... -->
    </customData>
  </contact>
  <signature></signature>
</request>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| id | int | идентификатор обновляемого контакта |  |
| general | int | номер обновляемого контакта (используется если не задан id) |  |
| customData |  | значения пользовательских полей контакта |  |
| customData.customValue.id |  | идентификатор пользовательского поля контакта |  |
| customData.customValue.value |  | значение пользовательского поля контакта | (для полей типа набор задач, список сотрудников, набор записей справочника \- идентификаторы через запятую в квадратных скобках) |
| signature | string(32) | подпись |  |

Результат успешного выполнения функции:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="ok">
  <contact>
    <id></id>
    <general></general>
  </contact>
  <actionid></actionid>
</response>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| contact.id | int | идентификатор обновляемого контакта |  |
| contact.general | int | номер добавленного контакта |  |
| actionid | int | идентификатор действия об изменении |  |

В противном случае будет возвращен ответ с ошибкой:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="error">
  <code></code>
</response>
```

## Перейти

---

## contact.get — Получить информацию о контакте

## Перейти

# ПланФикс API contact.get

Функция получения информации о клиенте. Формат запроса:

```
<?xml version="1.0" encoding="UTF-8"?>
<request method="contact.get">
  <account></account>
  <sid></sid>
  <contact>
    <id></id>
    <userid></userid>
    <general></general>
  </contact>
  <fields>
    <field>lastUpdateDate</field>
    ...
  </fields>
  <signature></signature>
</request>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| contact.id | int | идентификатор контакта |  |
| general | int | номер контакта | если задан, используется вместо id |
| userid | int | идентификатор контакта для случаев, когда он используется в системе наравне с сотрудниками (исполнитель задачи и т.п., а также пользовательское поле типа контакт) | если задан, используется вместо id при отсутствии general |
| fields |  | получить дополнительные поля |  |
| fields.field | string | поле, возможные значения:<br>- **lastUpdateDate** \- дата последнего изменения<br>- **lastCommentDate** \- дата последнего комментария | одноименное поле будет добавлено в ответ в узел contact |
| signature | string(32) | подпись |  |

Результат успешного выполнения функции:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="ok">
  <contact>
    <id></id>
    <userid></userid>
    <general></general>
    <template>
      <id></id>
    </template>
    <name></name>
    <midName></midName>
    <lastName></lastName>
    <isCompany></isCompany>
    <post></post>
    <email></email>
    <secondaryEmails>
        <email></email>
        <!-- ... -->
    </secondaryEmails>
    <phones>
        <phone>
            <number></number>
            <typeId><typeId>
            <typeName><typeName>
        </phone>
        ...
    </phones>
    <address></address>
    <description></description>
    <sex></sex>
    <site></site>
    <skype></skype>
    <icq></icq>
    <userPic></userPic>
    <birthdate></birthdate>
    <group>
      <id></id>
      <name></name>
    </group>
    <contractors>
      <client>
        <id></id>
        <name></name>
      </client>
      <client>
        <id></id>
        <name></name>
      </client>
      <!-- ... -->
    </contractors>
    <havePlanfixAccess>{true|false}</havePlanfixAccess>
    <telegram>
      <id></id>
    </telegram>
    <customData>
      <customValue>
        <field>
          <id></id>
          <name></name>
        </field>
        <value></value>
        <text></text>
      </customValue>
      <customValue>
        <!-- ... -->
      </customValue>
      <!-- ... -->
    </customData>
    <responsible>
      <users>
        <user>
          <id></id>
          <name></name>
        </user>
        <!-- ... -->
      </users>
      <groups>
        <group>
          <id></id>
          <name></name>
        </group>
        <!-- ... -->
      </groups>
    </responsible>
  </contact>
</response>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| id | int | идентификатор контакта |  |
| userid | int | идентификатор контакта для случаев, когда он используется в системе наравне с сотрудниками (исполнитель задачи и т.п., а также пользовательское поле типа контакт) |  |
| general | int | номер контакта |  |
| template.id | int | номер шаблона контакта |  |
| name | string | Имя Отчество |  |
| midName | string | Отчество |  |
| lastName | string | Фамилия |  |
| isCompany | boolean | Является компанией |  |
| post | string | Должность |  |
| email | string | адрес электронной почты |  |
| secondaryEmails.email |  | дополнительные адреса email, если есть |  |
| phones |  | список телефонов |  |
| phones.phone.number | string | номер телефона |  |
| phones.phone.typeId | int | идентификатор типа номера телефона |  |
| phones.phone.typeName | string | название типа номера телефона |  |
| address | string | Адрес |  |
| description | string | Дополнительная информация |  |
| sex | enum | пол | допустимые значения смотри в разделе пол клиента, если значение не установлено, то значение пусто |
| site | string | веб-сайт |  |
| skype | string | skype-контакт |  |
| icq | string | номер-icq |  |
| userPic | string | ссылка на изображение |  |
| birthdate | DateTime | дата рождения |  |
| group |  | группа контакта |  |
| group.id | int | идентификатор группы |  |
| group.name | string | название группы |  |
| signature | string(32) | подпись |  |
| contractors |  | список компаний, к которым он относится |  |
| contractors.client |  | описание компании |  |
| contractors.client.id | int | идентификатор компании |  |
| contractors.client.name | string | имя/название компании |  |
| havePlanfixAccess | bool | имеет ли контакт доступ к ПланФикс | данный параметр возвращается только пользователю с правами администратор |
| telegram.id | int | внутренний идентификатор в Telegram | возвращается только для контактов из Telegram |
| customData |  | значения пользовательских полей задачи |  |
| customData.customValue.field.id |  | идентификатор пользовательского поля |  |
| customData.customValue.field.name |  | название пользовательского поля |  |
| customData.customValue.value |  | значение пользовательского поля |  |
| customData.customValue.text |  | текстовое значение пользовательского поля |  |
| responsible |  | корневой элемент списка ответственных |  |
| responsible.users |  | корневой элемент списка ответственных |  |
| responsible.users.user | node | пользователь |  |
| responsible.users.user.id | int | идентификатор пользователя |  |
| responsible.users.user.name | string | имя пользователя |  |
| responsible.groups |  | корневой элемент списка групп ответственных |  |
| responsible.groups.group | node | группа |  |
| responsible.groups.group.id | int | идентификатор группы |  |
| responsible.groups.group.name | string | название группы |  |

В противном случае будет возвращен ответ с ошибкой:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="error">
  <code></code>
</response>
```

## Перейти

---

## contact.getList — Список контактов

## Перейти

# ПланФикс API contact.getList

Функция получения списка контактов. Формат запроса:

```
<?xml version="1.0" encoding="UTF-8"?>
<request method="contact.getList">
  <account></account>
  <sid></sid>
  <pageCurrent></pageCurrent>
  <pageSize></pageSize>
  <target></target>
  <company></company>
  <search></search>
  <filters>
    <filter>
      <type></type>
      <operator></operator>
      <value></value>
      <field></field>
      ...
    </filter>
    ...
  </filters>
  <fields>
    <field>lastUpdateDate</field>
    ...
  </fields>
  <signature></signature>
</request>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| pageCurrent | int | запрашиваемая страница |  |
| pageSize | int | размер запрашиваемого списка |  |
| target | enum / int | контакты, компании или заданный фильтр задач | допустимые значения смотри ниже |
| company | int | идентификатор компании, контакты которой надо выбрать | необязательный, при отсутствии узла \- выборка не ограничивается одной компанией |
| search | string | строка для поиска контактов, если задана будут возвращены контакты, в которых данная строка встречается в имени, фамилии или email |  |
| filters |  | дополнительные сложные фильтры | перечень и формат допустимых значений смотри в разделе фильтры контактов |
| fields |  | получить дополнительные поля |  |
| fields.field | string | поле, возможные значения:<br>- **lastUpdateDate** \- дата последнего изменения<br>- **lastCommentDate** \- дата последнего комментария | одноименное поле будет добавлено в ответ в узел contact |
| signature | string(32) | подпись |  |

### Допустимые значения параметра target

| Значение | Описание | Примечание |
| --- | --- | --- |
| contact | контакты | значение по умолчанию |
| company | компании |  |
| template | шаблоны |  |
| идентификатор фильтра контактов | доступные фильтры можно получить функцией contact.getFilterList |  |

Результат успешного выполнения запроса:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="ok">
  <contacts count="count" totalCount="totalCount">
    <contact>
      <id></id>
      <userid></userid>
      <general></general>
      <template>
        <id></id>
      </template>
      <name></name>
      <lastName></lastName>
      <isCompany></isCompany>
      <post></post>
      <email></email>
      <phones>
          <phone>
              <number></number>
              <typeId><typeId>
              <typeName><typeName>
          </phone>
          ...
      </phones>
      <address></address>
      <description></description>
      <sex></sex>
      <skype></skype>
      <facebook></facebook>
      <vk></vk>
      <telegramId></telegramId>
      <icq></icq>
      <userPic></userPic>
      <birthdate></birthdate>
      <havePlanfixAccess>{true|false}</havePlanfixAccess>
      <user>
        <login></login>
        <role></role>
        <status></status>
        <email></email>
      </user>
      <contractors>
        <client>
          <id></id>
          <name></name>
        </client>
        <client>
          <id></id>
          <name></name>
        </client>
        <!-- ... -->
      </contractors>
      <customData>
        <customValue>
          <field>
            <id></id>
            <name></name>
          </field>
          <value></value>
          <text></text>
        </customValue>
        <customValue>
          <!-- ... -->
        </customValue>
        <!-- ... -->
      </customData>
    </contact>
    <!-- ... -->
  </contacts>
</response>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| **contacts** |  | список контактов |  |
| **contacts** count | int | количество контактов в списке |  |
| **contacts** totalCount | int | количество контактов удовлетворяющих условию запроса |  |
| contact |  | узел, описывающий контакт |  |
| id | int | идентификатор контакта |  |
| userid | int | идентификатор контакта для случаев, когда он используется в системе наравне с сотрудниками (исполнитель задачи и т.п., а также пользовательское поле типа контакт) |  |
| general | int | номер контакта |  |
| template.id | int | номер шаблона контакта |  |
| name | string | Имя Отчество |  |
| lastName | string | Фамилия |  |
| isCompany | boolean | Является компанией |  |
| post | string | Должность |  |
| email | string | адрес электронной почты |  |
| phones |  | список телефонов |  |
| phones.phone.number | string | номер телефона |  |
| phones.phone.typeId | int | идентификатор типа номера телефона |  |
| phones.phone.typeName | string | название типа номера телефона |  |
| address | string | Адрес |  |
| description | string | Дополнительная информация |  |
| sex | enum | пол | допустимые значения смотри в разделе пол клиента |
| skype | string | skype-контакт |  |
| facebook | string | facebook |  |
| vk | string | vk |  |
| telegramId | string | telegramId |  |
| icq | string | номер-icq |  |
| userPic | string | ссылка на изображение |  |
| birthdate | DateTime | дата рождения |  |
| signature | string(32) | подпись |  |
| contractors |  | список контрагентов, к которым он относится |  |
| contractors.client |  | описание контрагента |  |
| contractors.client.id | int | идентификатор клиента/контрагента |  |
| contractors.client.name | string | имя/название контрагента |  |
| havePlanfixAccess | bool | имеет ли контакт доступ к ПланФикс | данный параметр возвращается только пользователю с правами администратор |
| user |  | учетные данные контакта | данный параметр возвращается только пользователю с правами администратор |
| user.login | string | логин в системе |  |
| user.role | string | роль |  |
| user.status | enum | статус |  |
| user.email | string | адрес электронной почты |  |
| customData |  | значения пользовательских полей задачи |  |
| customData.customValue.field.id |  | идентификатор пользовательского поля |  |
| customData.customValue.field.name |  | название пользовательского поля |  |
| customData.customValue.value |  | значение пользовательского поля |  |
| customData.customValue.text |  | текстовое значение пользовательского поля |  |

В противном случае будет возвращен ответ с ошибкой:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="error">
  <code></code>
</response>
```

## Перейти

---

## contact.getFilterList — Список фильтров

## Перейти

# ПланФикс API contact.getFilterList

Функция позволяет получить список доступных текущему сотруднику фильтров контактов для функции contact.getList. Формат вызова функции:

```
<?xml version="1.0" encoding="UTF-8"?>
<request method="contact.getFilterList">
  <account></account>
  <sid></sid>
  <signature></signature>
</request>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| signature | string(32) | md5 от имени функции, значений всех полей, исключая _signature_ | подробное описание алгоритма формирования подписи смотри в разделе ПланФикс API:Формирование цифровой подписи |

Ответ:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="ok">
  <contactFilterList totalCount="x">
    <contactFilter>
      <ID></ID>
      <Name></Name>
    </contactFilter>
    <contactFilter>
      <ID></ID>
      <Name></Name>
    </contactFilter>
    <!-- -->
  </contactFilterList>
</response>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| **contactFilterList** |  | список фильтров контактов |  |
| **contactFilterList** totalCount | int | количество элементов в списке |  |
| contactFilter |  | корневой элемент описывающий фильтр контактов |  |
| contactFilter.ID | int\|string | идентификатор фильтра (числовые идентификаторы у пользовательских фильтров и текстовые идентификаторы, начинающиеся с ":" \- у системных фильтров Контакты и Компании) |  |
| contactFilter.Name | string | название фильтра |  |

В противном случае будет возвращен ответ с ошибкой:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="error">
  <code></code>
</response>
```

## Перейти

---

## contact.getGroupList — Список групп контактов

## Перейти

# ПланФикс API contact.getGroupList

Запрос на получение списка групп контактов имеет следующий вид:

```
<?xml version="1.0" encoding="UTF-8"?>
<request method="contact.getGroupList">
    <account></account>
    <sid></sid>
    <signature></signature>
</request>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| account | string | аккаунт, на котором выполняется запрос |  |
| sid | string(32) | ключ сесии | выдается в результате прохождения аутентификации |
| signature | string(32) | подпись пакета |  |

Ответ, при успешном выполнении запроса:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="ok">
    <contactGroups count="count" totalCount="totalCount">
        <group>
            <id></id>
            <name></name>
        </group>
        <group>
            <id></id>
            <name></name>
        </group>
        <!-- ... -->
    </contactGroups >
</response>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| **contactGroups** | int | список групп |  |
| **contactGroups** count | int | количество групп в списке |  |
| **contactGroups** totalCount | int | количество групп в списке |  |
| group |  | группа |  |
| group.id | int | идентификатор группы |  |
| group.name | string | название группы |  |

В противном случае будет возвращен ответ с ошибкой:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="error">
  <code></code>
</response>
```

## Перейти

---

## contact.delete — Удаление контакта

## Перейти

# ПланФикс API contact.delete

Функция позволяет удалить контакт. Выполнение этой функции требует наличие соответствующих прав. Формат запроса:

```
<?xml version="1.0" encoding="UTF-8"?>
<request method="contact.delete">
  <account></account>
  <sid></sid>
  <contact>
    <id></id>
    <userid></userid>
    <general></general>
  </contact>
  <signature></signature>
</request>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| id | int | идентификатор контакта |  |
| signature | string(32) | подпись |  |

Результат успешного выполнения функции:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="ok">
  <contact>
    <id></id>
  </contact>
</response>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| id | int | идентификатор контакта |  |
| userid | int | идентификатор контакта для случаев, когда он используется в системе наравне с сотрудниками (исполнитель задачи и т.п., а также пользовательское поле типа контакт) |  |
| general | int | номер контакта |  |

В противном случае будет возвращен ответ с ошибкой:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="error">
  <code></code>
</response>
```

## Перейти

---

## contact.getPhoneTypes — Типы телефонов

## Перейти

# ПланФикс API contact.getPhoneTypes

Функция позволяет получить список доступных типов телефонных номеров. Формат вызова функции:

```
<?xml version="1.0" encoding="UTF-8"?>
<request method="contact.getPhoneTypes">
  <account></account>
  <sid></sid>
  <signature></signature>
</request>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| signature | string(32) | md5 от имени функции, значений всех полей, исключая _signature_ | подробное описание алгоритма формирования подписи смотри в разделе ПланФикс API:Формирование цифровой подписи |

Ответ:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="ok">
  <phoneTypes totalCount="x">
    <phoneType>
      <id></id>
      <name></name>
    </phoneType>
    <phoneType>
      <id></id>
      <name></name>
    </phoneType>
    <!-- -->
  </phoneTypes>
</response>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| **phoneTypes** |  | списоктипов телефонных номеров |  |
| **phoneTypes** totalCount | int | количество элементов в списке |  |
| phoneType |  | корневой элемент описывающий тип телефонного номера |  |
| phoneType.id | int | идентификатор |  |
| phoneType.name | string | название |  |

В противном случае будет возвращен ответ с ошибкой:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="error">
  <code></code>
</response>
```

## Перейти

---

## contact.managePlanfixAccess — Управление доступом

## Перейти

# ПланФикс API contact.managePlanfixAccess

Функция позволяет разрешить или запретить доступ для контакта. Выполнение этой функции требует наличие админ прав. Формат запроса:

```
<?xml version="1.0" encoding="UTF-8"?>
<request method="contact.managePlanfixAccess">
  <account></account>
  <sid></sid>
  <contact>
    <id></id>
    <userid></userid>
    <general></general>
    <email></email>
    <havePlanfixAccess></havePlanfixAccess>
  </contact>
  <signature></signature>
</request>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| id | int | идентификатор контакта |  |
| userid | int | идентификатор контакта для случаев, когда он используется в системе наравне с сотрудниками (исполнитель задачи и т.п., а также пользовательское поле типа контакт) |  |
| general | int | номер контакта |  |
| email | string | использовать указанный e-mail для организации доступа в ПланФикс | не обязательный параметр, используется только в первый раз, при открытии доступа к ПланФик'су |
| havePlanfixAccess | bool | запретить (0) или открыть(1) доступ к ПланФикс |  |
| signature | string(32) | подпись |  |

Результат успешного выполнения функции:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="ok">
  <contact>
    <id></id>
    <status></status>
  </contact>
</response>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| id | int | идентификатор контакта |  |
| status | enum | результат выполнения операции | список допустимых значений смотри в разделе результат выполнения contact.managePlanfixAccess |

В противном случае будет возвращен ответ с ошибкой:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="error">
  <code></code>
</response>
```

## Перейти

---

## Результат contact.managePlanfixAccess

## Перейти

# ПланФикс API: Результат выполнения contact.managePlanfixAccess

Список результатов выполнения операции по управлению правами доступа контактов

| Значение | Описание | Примечание |
| --- | --- | --- |
| NOTICE\_SEND | для пользователя инициирована процедура получения доступа к ПланФиксу. В этот момент, на указанный e-mail отправляется письмо с инструкцией |  |
| NOTICE\_RESEND | пользователю повторно отправляется письмо с приглашением в ПланФикс |  |
| ACCESS\_CANCELED | пользователь не активировал доступ в ПланФикс. Ссылка отправленная в письме теряет силу. Доступ отменен. |  |
| ACTIVE | доступ для контакта разрешен |  |
| UNACTIVE | доступ для контакта запрещен |  |

## Перейти

---

## contact.updateContractors — Обновление контрагентов

## Перейти

# ПланФикс API contact.updateContractors

Функция изменение информации о принадлежности контакта к компании. Формат запроса:

```
<?xml version="1.0" encoding="UTF-8"?>
<request method="contact.updateContractors">
  <account></account>
  <sid></sid>
  <contact>
    <id></id>
    <contractors>
      <addClient>
        <id></id>
        <id></id>
        <!-- -->
      </addClient>
      <delClient>
        <id></id>
        <id></id>
        <!-- -->
      </delClient>
    </contractors>
  </contact>
  <signature></signature>
</request>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| id |  |  |  |
| contractors |  | корневой элемент компаний |  |
| contractors.addClient |  | список компаний, к которым относится контакт |  |
| contractors.addClient.id | id | идентификатор компании |  |
| contractors.delClient |  | список компаний, которые исключаются из списка компаний контакта |  |
| contractors.delClient.id | id | идентификатор компании |  |
| signature |  |  |  |

Результат успешного выполнения:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="ok">
  <contact>
    <id></id>
  </contact>
</response>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| contact.id | int | идентификатор контакта |  |

В противном случае будет возвращен ответ с ошибкой:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="error">
  <code></code>
</response>
```

## Перейти

---

## contact.updateUserInfo — Обновление данных пользователя

## Перейти

# ПланФикс API contact.updateUserInfo

Функция обновления информации относящейся к залогиниванию пользователя в системе. Выполнение этой функции требует наличие админ прав. Формат запроса:

```
<?xml version="1.0" encoding="UTF-8"?>
<request method="contact.updateUserInfo">
  <account></account>
  <sid></sid>
  <contact>
    <id></id>
    <userid></userid>
    <general></general>
    <user>
      <password></password>
      <status></status>
      <email></email>
      <pic></pic>
      <notifyByEmail></notifyByEmail>
    </user>
  </contact>
  <signature></signature>
</request>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| id | int | идентификатор контакта |  |
| userid | int | идентификатор контакта для случаев, когда он используется в системе наравне с сотрудниками (исполнитель задачи и т.п., а также пользовательское поле типа контакт) |  |
| general | int | номер контакта |  |
| user |  | информация о учетке используемой для залогинивания |  |
| user.password | string | пароль |  |
| user.status | enum | статус | список допустимых значений смотри в разделе статусы пользователей |
| user.email | string | адрес электронной почты |  |
| user.jabber | string | Jabber-аккаунт | используется в системе уведомлений |
| user.notifyByEmail | bool | получать уведомления по электронной почте | установка этого параметра в 1 (использовать) требует непустого значения для поля **email** |
| user.notifyByJabber | bool | получать уведомления по jabber | установка этого параметра в 1 (использовать) требует непустого значения для поля **jabber** |
| user.notifyByPlanfix | bool | получать уведомления по внутренней системе уведомлений ПланФикс |  |
| user.pic | string | base64 закодированное изображение |  |
| signature | string(32) | подпись |  |

Результат успешного выполнения функции:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="ok">
  <contact>
    <id></id>
  </contact>
</response>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| contact.id | int | идентификатор контакта |  |

В противном случае будет возвращен ответ с ошибкой:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="error">
  <code></code>
</response>
```

## Перейти

---

## Фильтры контактов

## Перейти

# ПланФикс API: Фильтры контактов

Фильтры контактов задаются следующим набором параметров:

- type - числовой идентификатор фильтра
- operator - оператор фильтра, одно из значений из списка (equal, notequal, gt, lt) у разных фильтров могут быть разные допустимые операторы.
- value - значение фильтра, может быть строкой, числом или сложным объектом, в зависимости от типа фильтра
- field - идентификатор пользовательского поля, для фильтров по пользовательским полям

| Тип | Название | Операторы | Формат value |
| --- | --- | --- | --- |
| 12 | Дата создания | - equal<br>- notequal<br>- gt<br>- lt | объект : <br>```<br><value><br>  <datetype></datetype><br>  <datevalue></datevalue><br>  <datefrom></datefrom><br>  <dateto></dateto><br></value><br>```<br>datetype принимает следующие значения:<br>- today - сегодня<br>- yesterday - вчера<br>- tomorrow - завтра<br>- thisweek - текущая неделя<br>- lastweek<br>- nextweek<br>- thismonth<br>- lastmonth<br>- nextmonth<br>- last - последние n дней, n передается в datevalue<br>- next - следующие n дней, n передается в datevalue<br>- in - через n дней, n передается в datevalue<br>- anotherdate - точная дата, дата передается в формате дд-мм-гггг в datefrom<br>- anotherperiod - точный период, даты передаются в формате дд-мм-гггг в datefrom и dateto<br>примеры:<br>```<br><value><br>  <datetype>thisweek</datetype><br></value><br>```<br>```<br><value><br>  <datetype>anotherperiod</datetype><br>  <datefrom>01-01-2015</datefrom><br>  <dateto>01-02-2015</dateto><br></value><br>``` |
| 4223 | Дата рождения (с учетом года) |
| 4011 | Дата рождения (без учета года) |
| 4213 | Контрагент в задачах с последней активностью |
| 4219 | Контрагент без задач с последней активностью |
| 4214 | Участвует в задачах с последней активностью |
| 4220 | Не участвует в задачах с последней активностью |
| 4103 | Пользовательское поле типа Дата |
| 1 | Добавил | - equal<br>- notequal | int : идентификатор сотрудника |
| 2 | Ответственный |
| 47 | Доступен пользователю |
| 48 | Может редактироваться пользователем |
| 4108 | Пользовательское поле типа Контакт |
| 4109 | Пользовательское поле типа Сотрудник |
| 4112 | Пользовательское поле типа Группа, сотрудник, контакт |
| 4113 | Пользовательское поле типа Список сотрудников |
| 4006 | Является компанией | - equal | int - 1 |
| 4007 | Является контактом |
| 4010 | С доступом в ПланФикс |
| 4012 | Может быть участником задач |
| 4017 | Не может быть участником задач |
| 4013 | Может быть контрагентом задач |
| 4018 | Не может быть контрагентом задач |
| 4201 | Контрагент без активных задач |
| 4202 | Не участвует в активных задачах |
| 4203 | Контрагент с активными задачами |
| 4204 | Участвует в активных задачах |
| 4205 | Контрагент в просроченных задачах |
| 4206 | Участвует в просроченных задачах |
| 4001 | Имя или фамилия контакта / название компании | - equal<br>- notequal | string - осуществляется фильтр содержит / не содержит |
| 4002 | Должность |
| 4003 | Телефон |
| 4004 | Адрес |
| 4005 | Email |
| 4221 | Дополнительный email |
| 4014 | Имя контакта / Название компании |
| 4015 | Фамилия контакта |
| 4101 | Пользовательское поле типа Строка |
| 4102 | Пользовательское поле типа Число | - equal<br>- notequal<br>- gt<br>- lt | int |
| 4105 | Пользовательское поле типа Чекбокс | - equal<br>- notequal | int - 1 / 0 |
| 4106 | Пользовательское поле типа Список | - equal<br>- notequal | string |
| 4107 | Пользовательское поле типа Справочник | - equal<br>- notequal | int - идентификатор записи |
| 4114 | Пользовательское поле типа Набор записей справочника | - equal<br>- notequal | int - идентификатор записи, для условия по нескольким записям - идентификаторы через ; (точку с запятой) |
| 4111 | Пользовательское поле типа Набор значений | - equal<br>- notequal | string - значение, для условия по нескольким значениям - значения через ; (точку с запятой) |
| 4008 | Группа контактов | - equal<br>- notequal | int - идентификатор группы, можно получить методом contact.getGroupList |
| 4016 | Шаблон контакта | - equal<br>- notequal | int - номер шаблона, general в ответе метода contact.getList с target = template |

## Перейти

---

## Сложные фильтры контактов (REST API)

## Перейти

- REST API

# REST API: Сложные фильтры контактов

Сложные фильтры в REST API ПланФикса применяются в методе «/contact/list» при получении списка контактов. Фильтры контактов задаются следующим набором параметров:

- **type** — числовой идентификатор фильтра.
- **operator** — оператор фильтра, одно из значений списка (equal, notequal, gt, lt). У разных фильтров могут быть разные допустимые операторы.
- **value** — значение фильтра, в зависимости от типа фильтра может быть строкой, числом или сложным объектом.
- **field** — идентификатор пользовательского поля, используется для фильтров по пользовательским полям.
- **subfilter** — вложенный фильтр для фильтрации по значениям полей записи аналитик.

```
{
    "type": 12,
    "operator": "equal",
    "value": {
        "dateType": "otherDate",
        "dateValue": "01-07-2022"
    }
}
```

Пример запроса получения списка контактов с передачей нескольких фильтров (используется логика И):

```
{
  "fields": "name",
  "filters": [{\
        "type": 4223,\
        "operator": "equal",\
        "value": {\
"dateType": "otherDate",\
"dateValue": "01-12-1990"\
}\
     },\
     {\
        "type": 1,\
        "operator": "equal",\
        "value": "user:5"\
     }\
  ]
}
```

| Тип | Название | Операторы | Формат value |
| --- | --- | --- | --- |
| 12 | Дата создания | - equal<br>- notequal<br>- gt<br>- lt | Объект :<br>```<br> <br>"value": {<br>    "dateType": string,<br>    "dateValue": string,<br>    "dateFrom": string,<br>    "dateTo": string<br>}<br>```<br>dateType принимает следующие значения:<br>- today - сегодня<br>- yesterday - вчера<br>- tomorrow - завтра<br>- thisWeek - текущая неделя<br>- lastWeek<br>- nextWeek<br>- thisMonth<br>- lastMonth<br>- nextMonth<br>- last - последние n дней, n передается в dateValue<br>- next - следующие n дней, n передается в dateValue<br>- in - через n дней, n передается в dateValue<br>- otherDate - точная дата, дата передается в формате дд-мм-гггг в dateFrom<br>- otherRange - точный период, даты передаются в формате дд-мм-гггг в dateFrom и dateTo<br>- otherDate\_withTime - точная дата-время, дата передается в формате "дд-мм-гггг чч:мм" в dateFrom<br>- otherRange\_withTime - точный период с заданным временем, даты передаются в формате "дд-мм-гггг чч:мм" в dateFrom и dateTo<br>Даты считаются переданными в часовом поясе сотрудника, от имени которого сделан запрос.<br>Примеры:<br>```<br>"value": {<br>    "dateType": "thisWeek"<br>}<br>```<br>```<br>"value": {<br>    "dateType": "otherRange",<br>    "dateFrom": "02-07-2022",<br>    "dateTo": "06-07-2022"<br>}<br>```<br>```<br>"value": {<br>    "dateType": "otherDate_withTime",<br>    "dateFrom": "30-06-2022 12:00",<br>}<br>``` |
| 38 | Дата последнего изменения |
| 4223 | Дата рождения (с учетом года) |
| 4011 | Дата рождения (без учета года) |
| 4213 | Контрагент в задачах с последней активностью |
| 4219 | Контрагент без задач с последней активностью |
| 4214 | Участвует в задачах с последней активностью |
| 4220 | Не участвует в задачах с последней активностью |
| 4103 | Пользовательское поле типа Дата |
| 1 | Добавил | - equal<br>- notequal | string - номер сотрудника/контакта/группы с префиксом.<br>Например: “user:1”, “contact:5”, “group:3” |
| 2 | Ответственный |
| 47 | Доступен пользователю |
| 48 | Может редактироваться пользователем |
| 4108 | Пользовательское поле типа Контакт |
| 4109 | Пользовательское поле типа Сотрудник |
| 4112 | Пользовательское поле типа Группа, сотрудник, контакт |
| 4113 | Пользовательское поле типа Список сотрудников |
| 4006 | Является компанией | - equal | int - 1<br>boolean - true |
| 4007 | Является контактом |
| 4010 | С доступом в ПланФикс |
| 4012 | Может быть участником задач |
| 4017 | Не может быть участником задач |
| 4013 | Может быть контрагентом задач |
| 4018 | Не может быть контрагентом задач |
| 4201 | Контрагент без активных задач |
| 4202 | Не участвует в активных задачах |
| 4203 | Контрагент с активными задачами |
| 4204 | Участвует в активных задачах |
| 4205 | Контрагент в просроченных задачах |
| 4206 | Участвует в просроченных задачах |
| 4001 | Имя или фамилия контакта / название компании | - equal<br>- notequal | string - осуществляется фильтр содержит / не содержит |
| 4002 | Должность |
| 4003 | Телефон |
| 4004 | Адрес |
| 4005 | Email (частичное вхождение) |
| 4221 | Дополнительный email |
| 4014 | Имя контакта / Название компании |
| 4015 | Фамилия контакта |
| 4101 | Пользовательское поле типа Строка | - equal<br>- notequal<br>- have<br>- nothave | string - осуществляется фильтр равно / не равно / содержит / не содержит |
| 4102 | Пользовательское поле типа Число | - equal<br>- notequal<br>- gt<br>- lt | int |
| 4105 | Пользовательское поле типа Чек-бокс | - equal<br>- notequal | int - 1 / 0<br>boolean |
| 4106 | Пользовательское поле типа Список | - equal<br>- notequal | string |
| 4107 | Пользовательское поле типа Справочник | - equal<br>- notequal | int - идентификатор записи |
| 4114 | Пользовательское поле типа Набор записей справочника | - equal<br>- notequal | int - идентификатор записи, для условия по нескольким записям — идентификаторы через ; (точку с запятой) |
| 4111 | Пользовательское поле типа Набор значений | - equal<br>- notequal | string - значение, для условия по нескольким значениям - значения через ; (точку с запятой) |
| 4152 | Содержит значение в пользовательском поле | - equal | int - идентификатор поля |
| 4153 | Не содержит значение в пользовательском поле |
| 4008 | Группа контактов | - equal<br>- notequal | int - идентификатор группы, можно получить методом /contact/groups |
| 4016 | Шаблон контакта | - equal<br>- notequal | int - номер шаблона, список шаблонов контактов можно получить методом /contact/templates |
| 4019 | Пол контакта | - equal<br>- notequal | string - пол контакта (NotDefined, Female, Male) |
| 4231 | Номер контакта | - equal<br>- notequal | int - номер контакта |
| 4233 | Идентификатор контакта (из XML API) | - equal<br>- notequal | int - идентификатор контакта |
| 93 | Значение поля записи аналитики | (в зависимости от типа поля)<br>- equal<br>- notequal<br>- gt<br>- lt<br>- have<br>- nothave | Значение поля аналитики по которому выполняется фильтрация, дополнительно в структуре фильтра надо передать поле subfilter, пример для фильтра поля аналитики типа Строка:<br>```<br>{<br>  "offset": 0,<br>  "pageSize": 100,<br>  "filters": [<br>    {<br>      "type": 93,<br>      "operator": "equal",<br>      "value": "Test value",<br>      "subfilter": {<br>        "dataTagId": 6,<br>        "filter": {<br>          "type": 3108,<br>          "field": 20<br>        }<br>      }<br>    }<br>  ],<br>  "fields": "id,name,dataTags"<br>}<br>```<br>где:<br>- dataTagId — идентификатор аналитики.<br>- filter — объект фильтра по этой аналитике.<br>- type — тип фильтра, в данном случае сложный фильтр аналитики по полю типа Строка.<br>- field — идентификатор поля аналитики, по которому выполняется фильтр. |

## Сложные фильтры по интеграциям

| Тип | Название | Операторы | Формат value |
| --- | --- | --- | --- |
| 4226 | Имя пользователя Telegram | - have<br>- nothave | string - имя пользователя (username, @username, https://t.me/username) |
| 4234 | Идентификатор Telegram | - equal<br>- notequal | int - идентификатор в Telegram |
| 4227 | Имя пользователя Instagram | - have<br>- nothave | string - имя пользователя (username, @username, https://www.instagram.com/username) |
| 4229 | Имя пользователя Facebook | - have<br>- nothave | string - имя пользователя (username, https://www.facebook.com/username) |

## Перейти

- REST API

---

## Типы клиентов

## Перейти

# ПланФикс API: Типы клиентов

Список допустимых значений для параметра **type** (типы клиентов):

| Значение | Описание | Примечание |
| --- | --- | --- |
| PERSON | Клиент |  |
| COMPANY | Компания |  |

## Перейти

---

## Пол клиента

## Перейти

# ПланФикс API: Пол клиента

| Значение | Описание | Примечание |
| --- | --- | --- |
| MALE | Мужской |  |
| FEMALE | Женский |  |

## Перейти

---

## Типы сортировок клиентов

## Перейти

# ПланФикс API: Типы сортировок клиентов

Типы сортировок клиентов:

| Значение | Описание | Примечание |
| --- | --- | --- |
| NAME\_ASC | по имени (алфавит) |  |
| NAME\_DESC | по имени (обратный порядок) |  |

## Перейти

---

## Связанные темы

- [[REST API — Пользователи (XML)]]
- [[REST API — Задачи (XML)]]
- [[PlanFix API — Index]]
