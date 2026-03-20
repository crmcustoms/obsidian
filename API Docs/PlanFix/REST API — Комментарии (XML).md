# REST API — Комментарии (XML API)

← [[PlanFix API — Index]]

Функции XML API для управления комментариями (action = действие/комментарий).
Auth: см. [[REST API — Авторизация]]

---

## Список функций

Список функций для управления комментариями:

1. action.add / Добавить комментарий
2. action.update / Обновить комментарий
3. action.get / Получить комментарий
4. action.getList / Получить список комментариев
5. action.getListByPeriod / Получить список комментариев за заданный период
6. action.getListWithAnalitic / Получить список комментариев с указанной аналитикой
7. action.delete / Удалить комментарий

Список функций

---

## action.add — Добавить комментарий

Добавление комментария. Неполная версия функции, будет дорабатываться:

```
<?xml version="1.0" encoding="UTF-8"?>
<request method="action.add">
  <account></account>
  <sid></sid>
  <action>
    <description></description>
    <task>
      <id></id>
      <general></general>
    </task>
    <contact>
      <general></general>
    </contact>
    <taskNewStatus></taskNewStatus>
    <notifiedList>
      <user>
        <id></id>
        <id></id>
        <!-- ... -->
      </user>
    </notifiedList>
    <isHidden></isHidden>
    <owner>
      <id></id>
    </owner>
    <dateTime></dateTime>
    <analitics>
      <analitic>
        <id></id>
        <analiticData>
          <itemData>
            <fieldId></fieldId>
            <value></value>
          </itemData>
          <itemData>
            <fieldId></fieldId>
            <value></value>
          </itemData>
          <!-- ... -->
        </analiticData>
      </analitic>
      <analitic>
        <id></id>
        <analiticData>
          <itemData>
            <fieldId></fieldId>
            <value></value>
          </itemData>
          <itemData>
            <fieldId></fieldId>
            <value></value>
          </itemData>
          <!-- ... -->
        </analiticData>
      </analitic>
      <!-- ... -->
    </analitics>
  </action>
  <signature></signature>
</request>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| description | string | описание комментария |  |
| task (contact) |  | задача/контакт, к которым добавляется комментарий — должен присутствовать только один узел (или task или contact) |  |
| task.id | int | идентификатор задачи |  |
| task.general | int | номер задачи (если задан, используется вместо id) |  |
| contact.general | int | номер контакта |  |
| taskNewStatus | enum | этим комментарием меняется статус задачи на указанный | не обязательный параметр, перечень допустимых значений смотри в разделе статусы задач |
| notifiedList |  | этим комментарием необходимо уведомить следующих пользователей | необязательный параметр поле |
| notifiedList.user |  | список пользователей, которые получат уведомление |  |
| notifiedList.user.id | int | идентификатор пользователя |  |
| isHidden | bool | является ли комментарий скрытым от всех пользователей, за исключением списка уведомленных пользователей | не обязательное поле, по умолчанию равно 0 (false) |
| owner |  | автор комментария | необязательное поле. Если не указано — берется пользователь, от имени которого выполняется функция |
| owner.id | int | идентификатор автора комментария | если это контакт \- нужно использовать userid из ответа contact.get |
| dateTime | DateTime | дата/время создания — не обязательный, по умолчанию текущие | может заполняться, только если авторизация была сделана под сотрудником с правами администратора |
| analitics |  | задается (прикрепляется) список аналитики | не обязательный параметр |
| analitics.analitic |  | узел, содержащий данные по прикрепляемой аналитике |  |
| analitic.id | int | идентификатор аналитики | список доступных аналитик можно получить при помощи функции analitic.getList |
| analitic.analiticData |  | список значений полей |  |
| analiticData.itemData |  | значение одного из параметров |  |
| itemData.fieldId | int | идентификатор параметра | идентификатор параметра равен field.id |
| itemData.value | string | значение |  |
|  | формат значения для разных типов аналитики: |
|  | DATE | дд-мм-гггг |
|  | TIME | чч:мм |
|  | TIMEPERIOD | <begin>чч:мм</begin><end>чч:мм</end> |
|  | HANDBOOK | int - key записи справочника |
|  | USER | int - идентификатор сотрудника |
|  | CLIENT | int - идентификатор контрагента |
|  | LOGINLIST | <id></id>...<id></id> - где каждый из узлов (узел может быть один) содержит идентификатор сотрудника, к которому относится аналитика. |

Результат удачного выполнения запроса:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="ok">
  <action>
    <id></id>
  </action>
</response>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| action.id | int | идентификатор добавленного действия |  |

В противном случае будет возвращен ответ с ошибкой:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="error">
  <code></code>
</response>
```

---

## action.update — Обновить комментарий

Функция обновления данных в комментарии. Формат запроса:

```
<?xml version="1.0" encoding="UTF-8"?>
<request method="action.update">
  <account></account>
  <sid></sid>
  <action>
    <id></id>
    <description></description>
    <taskNewStatus></taskNewStatus>
    <notifiedList>
      <user>
        <id></id>
        <id></id>
        <!-- ... -->
      </user>
    </notifiedList>
    <isHidden></isHidden>
    <dateTime></dateTime>
    <analitics>
      <analitic>
        <id></id>
        <analiticData>
          <key></key>
          <itemData>
            <fieldId></fieldId>
            <value></value>
          </itemData>
          <itemData>
            <fieldId></fieldId>
            <value></value>
          </itemData>
          <!-- ... -->
        </analiticData>
      </analitic>
      <!-- ... -->
      <analitic>
        <id></id>
        <analiticData>
          <itemData>
            <fieldId></fieldId>
            <value></value>
          </itemData>
          <itemData>
            <fieldId></fieldId>
            <value></value>
          </itemData>
          <!-- ... -->
        </analiticData>
      </analitic>
      <!-- ... -->
    </analitics>
    <deletedAnalitics>
      <key></key>
      <key></key>
      <!-- ... -->
    </deletedAnalitics>
  </action>
  <signature></signature>
</request>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| id | int | идентификатор комментария |  |
| description | string | текст комментария |  |
| taskNewStatus | enum | этим комментарием меняется статус задачи на указанный | не обязательный параметр, перечень допустимых значений смотри в разделе статусы задач, попытка поменять на неправильный статус или поменять статус не последним комментарием приведет к ошибке |
| notifiedList |  | этим комментарием необходимо уведомить следующих пользователей |  |
| notifiedList.user |  | список пользователей, которые получат уведомление |  |
| notifiedList.user.id | int | идентификатор пользователя |  |
| isHidden | bool | является ли комментарий скрытым от всех пользователей, за исключением списка уведомленных пользователей | не обязательное поле, по умолчанию равно 0 (false) |
| dateTime | DateTime | дата/время создания \- не обязательный, по умолчанию \- не изменяется | может заполняться, только если авторизация была сделана под сотрудником с правами администратора |
| analitics |  | список обновляемых аналитик | не обязательный параметр |
| analitics.analitic |  | узел, содержащий данные по аналитике |  |
| analitic.id | int | идентификатор аналитики | список доступных аналитик можно получить при помощи функции analitic.getList |
| analitic.analiticData |  | список строк аналитики |  |
| analiticData.key | int | идентификатор строки аналитики, если в запросе этот атрибут присутствует \- происходит изменения аналитики с этим идентификатором, если нет \- добавление новой строки аналитики |  |
| analiticData.itemData |  | значение поля аналитики |  |
| itemData.fieldId | int | идентификатор поля аналитики | идентификаторы можно получить методом feild.id |
| itemData.value | string | значение поля аналитики |  |
| deletedAnalitics |  | список удаленных аналитик |  |
| deletedAnalitics.key | int | ключ данных удаляемой аналитики | данное поле становится доступным после выполнения функции analitic.getData |
| signature | string(32) | подпись |  |

Помните, можно обновлять комментарии с типом **ACTION** и **COMMENT**. Остальные попытки будут вызывать ошибку.

Результат удачного выполнения запроса:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="ok">
  <action>
    <id></id>
  </action>
</response>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| action.id | int | идентификатор обновляемого комментария |  |

В противном случае будет возвращен ответ с ошибкой:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="error">
  <code></code>
</response>
```

---

## action.get — Получить комментарий

Функция получения информации о комментарии. Формат запроса:

```
<?xml version="1.0" encoding="UTF-8"?>
<request method="action.get">
  <account></account>
  <sid></sid>
  <action>
    <id></id>
  </action>
  <signature></signature>
</request>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| action.id | int | идентификатор комментария |  |
| signature | string(32) | подпись |  |

Ответ:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="ok">
  <action>
    <id></id>
    <description></description>
    <type></type>
    <statusChange>
      <oldStatus></oldStatus>
      <newStatus></newStatus>
    </statusChange>
    <isNotRead></isNotRead>
    <fromEmail></fromEmail>
    <dateTime></dateTime>
    <task>
      <id></id>
      <title></title>
    </task>
    <contact>
      <general></general>
      <name></name>
    </contact>
    <owner>
      <id></id>
      <name></name>
    </owner>
    <project>
      <id></id>
      <title></title>
    </project>
    <taskExpectDateChanged>
      <oldDate></oldDate>
      <newDate></newDate>
    </taskExpectDateChanged>
    <taskStartTimeChanged>
      <oldDate></oldDate>
      <newDate></newDate>
    </taskStartTimeChanged>
    <files>
      <file>
        <id></id>
        <name></name>
      </file>
      <file>
        <id></id>
        <name></name>
      </file>
      <!-- ... -->
    </files>
    <notifiedList>
      <user>
        <id></id>
        <name></name>
      </user>
      <user>
        <id></id>
        <name></name>
      </user>
      <!-- ... -->
    </notifiedList>
    <analitics>
      <analitic>
        <id></id>
        <key></key>
        <name></name>
      </analitic>
      <analitic>
        <id></id>
        <key></key>
        <name></name>
      </analitic>
      <!-- ... -->
    </analitics>
  </action>
</response>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| id | int | идентификатор комментария |  |
| description | string | описание комментария |  |
| type | enum | тип действия | список возможных значений смотри в разделе типы действий |
| statusChange |  | наличие данного узла свидетельствует о том, что этим комментарием был изменен статус задачи |  |
| statusChange.oldStatus | enum | старый статус | список допустимых значений смотри статусы задач |
| statusChange.newStatus | enum | новый статус | список допустимых значений смотри статусы задач |
| isNotRead | bool | комментарий не помечен как прочитанный |  |
| fromEmail | bool | комментарий создан из письма по электронной почте |  |
| dateTime | DateTime | дата добавления комментария |  |
| task |  | информация о задаче |  |
| task.id | int | идентификатор задачи |  |
| task.title | string | название задачи |  |
| contact |  | информация о контакте | присутствует, только если комментарий добавлен к контакту |
| contact.general | int | номер контакта |  |
| contact.name | string | имя контакта |  |
| owner |  | пользователь, который создал комментарий |  |
| owner.id | int | идентификатор пользователя |  |
| owner.name | string | имя пользователя |  |
| project |  | в рамках какого проекта был создан комментарий |  |
| project.id | int | идентификатор проекта |  |
| project.title | string | название проекта |  |
| taskExpectDateChanged |  | если задан данный узел, то этим комментарием было изменено время окончания задачи |  |
| taskExpectDateChanged.oldDate | DateTime | прежнее время |  |
| taskExpectDateChanged.newDate | DateTime | новое время |  |
| taskStartTimeChanged |  | если задан данный узел, то этим комментарием было изменено время начала задачи (приступить к работе) |  |
| taskStartTimeChanged.oldDate | DateTime | прежнее время |  |
| taskStartTimeChanged.newDate | DateTime | новое время |  |
| files |  | список файлов прикрепленных этим комментарием |  |
| files.file |  | узел, описывающий файл |  |
| files.file.id | int | идентификатор файла |  |
| files.file.name | string | имя файла |  |
| notifiedList |  | список пользователей, которых должны уведомить о комментарии |  |
| notifiedList.user |  | пользователь |  |
| notifiedList.user.id | int | идентификатор пользователя |  |
| notifiedList.user.name | string | имя пользователя |  |
| analitics |  | список прикрепленных аналитик к комментарию |  |
| analitics.analitic |  | аналитика |  |
| analitics.analitic.id | int | идентификатор аналитики |  |
| analitics.analitic.key | int | идентификатор строки данных аналитики |  |
| analitics.analitic.name | string | название аналитики |  |

В противном случае будет возвращен ответ с ошибкой:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="error">
  <code></code>
</response>
```

---

## action.getList — Список комментариев

Получение списка комментариев в задаче:

```
<?xml version="1.0" encoding="UTF-8"?>
<request method="action.getList">
  <account></account>
  <sid></sid>
  <task>
    <id></id>
    <general></general>
  </task>
  <contact>
    <general></general>
  </contact>
  <pageCurrent></pageCurrent>
  <pageSize></pageSize>
  <sort></sort>
  <signature></signature>
</request>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| task (contact) |  | задача/контакт, из которого получаем комментарии — должен присутствовать только один узел (или task или contact) |  |
| task.id | int | идентификатор задачи |  |
| task.general | int | номер задачи (если задан, используется вместо id) |  |
| contact.general | int | номер контакта |  |
| pageCurrent | int | текущая страница |  |
| pageSize | int | размер запрашиваемой страницы | не может превышать 100 |
| sort | enum: {asc,desc} | сортировка списка | необязательный параметр, по умолчанию desc |
| signature | string(32) | подпись |  |

Ответ:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="ok">
  <actions count="count" totalCount="totalCount">
    <action><!-- ... --></action>
    <action><!-- ... --></action>
    <!-- ... -->
  </actions>
</response>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| **actions** |  | корневой узел содержащий список комментариев |  |
| **actions** count | int | количество комментариев в ответе |  |
| **actions** totalCount | int | количество комментариев удовлетворяющих запросу |  |
| action |  | комментарий, описание параметра смотри в разделе Получить комментарий |  |

В противном случае будет возвращен ответ с ошибкой:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="error">
  <code></code>
</response>
```

---

## action.getListByPeriod — Список за период

Получение списка всех комментариев за заданный период времени:

```
<?xml version="1.0" encoding="UTF-8"?>
<request method="action.getListByPeriod">
  <account></account>
  <sid></sid>
  <fromDate></fromDate>
  <toDate></toDate>
  <pageCurrent></pageCurrent>
  <pageSize></pageSize>
  <sort></sort>
  <task>
    <id></id>
  </task>
  <signature></signature>
</request>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| fromDate | DateTime | начало периода |  |
| toDate | DateTime | конец периода |  |
| pageCurrent | int | текущая страница |  |
| pageSize | int | размер запрашиваемой страницы | не может превышать 100 |
| sort | enum: {asc,desc} | сортировка списка | необязательный параметр, по умолчанию desc |
| task.id | int | идентификатор задачи | необязательный параметр, по умолчанию выбираются комментарии по всем задачам |
| signature | string(32) | подпись |  |

Ответ:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="ok">
  <actions count="count" totalCount="totalCount">
    <action><!-- ... --></action>
    <action><!-- ... --></action>
    <!-- ... -->
  </actions>
</response>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| **actions** |  | корневой узел содержащий список комментариев |  |
| **actions** count | int | количество комментариев в ответе |  |
| **actions** totalCount | int | количество комментариев, удовлетворяющих запросу |  |
| action |  | комментарий, описание параметра смотри в разделе Получить комментарий |  |

В противном случае будет возвращен ответ с ошибкой:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="error">
  <code></code>
</response>
```

---

## action.getListWithAnalitic — Список с аналитикой

Получение списка комментариев с указанной аналитикой:

```
<?xml version="1.0" encoding="UTF-8"?>
<request method="action.getListWithAnalitic">
  <account></account>
  <sid></sid>
  <analitic>
    <id></id>
  </analitic>
  <task>
    <id></id>
  </task>
  <pageCurrent></pageCurrent>
  <pageSize></pageSize>
  <signature></signature>
</request>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| analitic.id | int | идентификатор аналитики | список аналитик можно получить с помощью списка доступных аналитик в ПланФикс |
| task.id | int | если задано значение, то происходит выборка комментариев только из этой задачи | необязательный параметр |
| pageCurrent | int | текущая страница |  |
| pageSize | int | размер запрашиваемой страницы | не может превышать 100 |
| signature | string(32) | подпись |  |

Ответ:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="ok">
  <actions count="count" totalCount="totalCount">
    <action><!-- ... --></action>
    <action><!-- ... --></action>
    <!-- ... -->
  </actions>
</response>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| **actions** |  | корневой узел содержащий список комментариев |  |
| **actions** count | int | количество комментариев в ответе |  |
| **actions** totalCount | int | количество комментариев, удовлетворяющих запросу |  |
| action |  | комментарий, описание параметра смотри в разделе Получить комментарий |  |

В противном случае будет возвращен ответ с ошибкой:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="error">
  <code></code>
</response>
```

---

## action.delete — Удалить комментарий

Функция позволяет удалить комментарий. Её выполнение требует наличие соответствующих прав. Также не может быть удален комментарий, которым менялся статус задачи.

Формат запроса:

```
<?xml version="1.0" encoding="UTF-8"?>
<request method="action.delete">
  <account></account>
  <sid></sid>
  <action>
    <id></id>
  </action>
  <signature></signature>
</request>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| id | int | идентификатор комментария |  |
| signature | string(32) | подпись |  |

Результат успешного выполнения функции:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="ok">
  <action>
    <id></id>
  </action>
</response>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| id | int | идентификатор комментария |  |

В противном случае будет возвращен ответ с ошибкой:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="error">
  <code></code>
</response>
```

---

## Список типов действий

Список типов действий:

| Значение | Описание | Примечание |
| --- | --- | --- |
| ACTION | Действие |  |
| COMMENT | Комментарий |  |
| FILE | Файл |  |
| TASKCREATED | Задача создана |  |
| STATUSCHANGED | Статус изменен |  |
| TASKOVERDUED | Задача просрочена |  |
| TASKNOTACCEPTEDINTIME | Задача не принята вовремя |  |
| TASKREJECTED | Задача отклонена |  |
| TASKACCEPTED | Задача принята |  |
| WORKEREMPLOYED | К работе подключен сотрудник |  |
| TASKCLOSETODEADLINE | Задача близка к завершению |  |
| REMINDER | Напоминание |  |
| WORKERUNEMPLOYED | Сотрудник отстранен от работы |  |
| TASKEXPECTDATECHANGED | Изменена дата завершения задачи |  |
| CHANGEDATEREQUEST | Запрос на изменение даты завершения задачи, ожидающий реакции постановщика |  |
| CHANGEDATEREQUESTINACTIVE | Запрос на изменение даты завершения задачи, с которым уже согласился (или не согласился) постановщик |  |
| CHANGEDATEREQUESTRESULT | Результат запроса на изменение даты завершения задачи |  |
| TASKCHANGED | Данные задачи изменены |  |
| TASKCHECKCHANGED | Изменен чек-лист задачи |  |

---

## Связанные темы

- [[REST API — Задачи (XML)]]
- [[REST API — Авторизация]]
- [[PlanFix API — Index]]
