# REST API — Задачи (XML API)

← [[PlanFix API — Index]]

Функции XML API для управления задачами.
Auth: см. [[REST API — Авторизация]]

---

## Список функций

Список функций для управления задачами:

01. task.add / Добавление задачи
02. task.update / Обновление задачи
03. task.updateCustomData / Обновление пользовательских полей задачи
04. task.get / Получение карточки задачи
05. task.getMulti / Получение множества карточек задач
06. task.getList / Список задач
07. task.accept / Принять задачу
08. task.reject / Отклонить задачу
09. task.changeExpectDate / Изменить дату выполнения задачи
10. task.changeStatus / Изменить статус задачи
11. task.getPossibleStatusToChange / Получить список возможных статусов для изменения
12. task.changeWorkers / Изменить (добавить/удалить) исполнителей
13. task.getFilterList/ Получить список доступных фильтров задач

Список функций

---

## task.add — Создание задачи

Функция позволяет создать новую задачу. Формат запроса:

```
<?xml version="1.0" encoding="UTF-8"?>
<request method="task.add">
  <account></account>
  <sid></sid>
  <task>
    <template></template>
    <title></title>
    <description></description>
    <importance></importance>
    <status></status>
    <statusSet></statusSet>
    <checkResult></checkResult>
    <owner>
      <id></id>
    </owner>
    <parent>
      <id></id>
    </parent>
    <project>
      <id></id>
    </project>
    <client>
      <id></id>
    </client>
    <beginDateTime></beginDateTime>
    <startDateIsSet></startDateIsSet>
    <startDate></startDate>
    <startTimeIsSet></startTimeIsSet>
    <startTime></startTime>
    <endDateIsSet></endDateIsSet>
    <endDate></endDate>
    <endTimeIsSet></endTimeIsSet>
    <endTime></endTime>
    <isSummary></isSummary>
    <duration></duration>
    <durationUnit></durationUnit>
    <durationIsSet></durationIsSet>
    <workers>
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
    </workers>
    <members>
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
    </members>
    <auditors>
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
    </auditors>
    <customData>
      <customValue>
        <id></id>
        <value></value>
      </customValue>
      <!-- ... -->
    </customData>
  </task>
  <signature></signature>
</request>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| template | id | идентификатор шаблона задачи | id в ответе task.getList с target = template |
| Реализация создания задачи по шаблону в API на текущий момент неполна.<br>Из шаблона устанавливается форма создания задачи (и соответственно имеющиеся в задаче пользовательские поля) и часть её свойств. <br>Из шаблона на текущий момент не устанавливаются: сроки задачи, аудиторы, аналитики, файлы, напоминания, чек-листы. Исполнители и участники устанавливаются из шаблона, если они отсутствуют в запросе. |
| title | string | название задачи |  |
| description | string | о чем задача, описание |  |
| importance | enum | срочность | перечень допустимых значений смотри в разделе срочность задач |
| status | enum/int | статус задачи | Допустимы значения из раздела Системные статусы задач или идентификаторы статусов, полученные в результате вызова функции taskStatus.getListOfSet |
| statusSet | int | процесс задачи | Идентификаторы процессов можно получить в результате вызова функции taskStatus.getSetList |
| checkResult | bool | является ли задача задачей с обязательной проверкой результата |  |
| owner |  | создатель задачи | необязательное поле. Если не указано \- берется пользователь от имени которого выполняется функция |
| owner.id | int | идентификатор пользователя | если это контакт \- нужно использовать userid из ответа contact.get |
| parent |  | над задача | необязательное поле |
| parent.id | int | идентификатор задачи, которая будет являться над задачей | допустимо значение 0 (ноль) |
| project |  | в рамках какого проекта поставлена задача |  |
| project.id | int | идентификатор проекта |  |
| client |  | контрагент | необязательный параметр |
| client.id | int | идентификатор контрагента (id из ответа contact.get) | допустимо значение 0 |
| beginDateTime | DateTime | дата/время создания \- не обязательный, по-умолчанию текущие | может заполняться, только если авторизация была сделана под сотрудником с правами администратора |
| startDateIsSet | bool (0/1) | задана ли дата начала работы |  |
| startDate | Date | дата начала работы | в интерфейсе ПланФикс поле _приступить к работе_ |
| startTimeIsSet | bool (0/1) | задано ли время начала работы |  |
| startTime | Time | время начала работы | в интерфейсе ПланФикс поле _приступить к работе_ |
| endDateIsSet | bool (0/1) | задана ли дата завершения работы |  |
| endDate | Date | дата завершения работы | в интерфейсе ПланФикс поле _закончить работу До_ |
| endTimeIsSet | bool (0/1) | задано ли время завершения работы |  |
| endTime | Time | время завершения работы | в интерфейсе ПланФикс поле _закончить работу До_ |
| isSummary | bool (0/1) | задача является суммарной |  |
| durationIsSet | bool | задана ли длительность |  |
| duration | int | длительность |  |
| durationUnit | int | 0 - минуты, 1 - часы, 2 - дни |  |
| workers |  | корневой элемент списка исполнителей задачи |  |
| workers.users |  | корневой элемент списка пользователей, которым поставлена задача |  |
| workers.users.id | int | идентификатор пользователя которому поставлена задача | для контактов \- нужно использовать userid из ответа contact.get |
| workers.groups |  | корневой элемент списка групп, которым поставлена задача |  |
| workers.groups.id | int | идентификатор группы |  |
| members |  | корневой элемент списка участников задачи |  |
| members.users |  | корневой элемент списка участников задачи |  |
| members.users.id | int | идентификатор участника задачи | для контактов \- нужно использовать userid из ответа contact.get |
| members.groups |  | корневой элемент списка групп участников |  |
| members.groups.id | int | идентификатор группы участников |  |
| auditors |  | корневой элемент списка аудиторов задачи, содержимое аналогично workers и members |  |
| customData |  | значения пользовательских полей задачи |  |
| customData.customValue.id |  | идентификатор пользовательского поля задачи |  |
| customData.customValue.value |  | значение пользовательского поля задачи | (для полей типа набор задач, список сотрудников, набор записей справочника \- идентификаторы через запятую в квадратных скобках) |

Добавляемые даты могут задаваться в двух форматах. Первый формат короткий, указывается только число, год и месяц. Второй формат \- полный, дополнительно указывается время начала/завершения, если того требует задача.

Ответ при удачном выполнении операции:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="ok">
  <task>
    <id></id>
    <general></general>
  </task>
</response>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| task.id | int | идентификатор созданной задачи |  |
| task.general | int | сквозной номер созданной задачи |  |

В противном случае будет возвращен ответ с ошибкой:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="error">
  <code></code>
</response>
```

---

## task.update — Обновление задачи

Функция обновления информации задачи. Формат запроса:

```
<?xml version="1.0" encoding="UTF-8"?>
<request method="task.update">
  <account></account>
  <sid></sid>
  <silent></silent>
  <task>
    <id></id>
    <general></general>
    <title></title>
    <description></description>
    <importance></importance>
    <status></status>
    <statusSet></statusSet>
    <checkResult></checkResult>
    <owner>
      <id></id>
    </owner>
    <parent>
      <id></id>
    </parent>
    <project>
      <id></id>
    </project>
    <client>
      <id></id>
    </client>
    <startDateIsSet></startDateIsSet>
    <startDate></startDate>
    <startTimeIsSet></startTimeIsSet>
    <startTime></startTime>
    <endDateIsSet></endDateIsSet>
    <endDate></endDate>
    <endTimeIsSet></endTimeIsSet>
    <endTime></endTime>
    <isSummary></isSummary>
    <duration></duration>
    <durationUnit></durationUnit>
    <durationIsSet></durationIsSet>
    <workers>
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
    </workers>
    <members>
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
    </members>
    <auditors>
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
    </auditors>
    <customData>
      <customValue>
        <id></id>
        <value></value>
      </customValue>
      <!-- ... -->
    </customData>
  </task>
  <signature></signature>
</request>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| silent | bool | при значении 1 - об изменении не рассылаются уведомления, не создаются действия и записи в логе задачи | обязательно значение 1 при массовых периодических обновлениях задач |
| id | int | идентификатор обновляемой задачи |  |
| general | int | номер задачи (если задан, используется вместо id) |  |
| title | string | название задачи |  |
| description | string | о чем задача, описание |  |
| importance | enum | срочность | перечень допустимых значений смотри в разделе срочность задач |
| status | enum/int | статус задачи | Допустимы значения из раздела Системные статусы задач или идентификаторы статусов, полученные в результате вызова функции taskStatus.getListOfSet |
| statusSet | int | процесс задачи | Идентификаторы процессов можно получить в результате вызова функции taskStatus.getSetList |
| checkResult | bool | является ли задача задачей с обязательной проверкой результата |  |
| owner |  | создатель задачи | необязательное поле. Если не указано \- берется пользователь от имени которого выполняется функция |
| owner.id | int | идентификатор пользователя |  |
| parent |  | над задача | необязательное поле |
| parent.id | int | идентификатор задачи, которая будет являться над задачей | допустимо значение 0 (ноль) |
| project |  | в рамках какого проекта поставлена задача |  |
| project.id | int | идентификатор проекта |  |
| client |  | контрагент | необязательный параметр |
| client.id | int | идентификатор контрагента | допустимо значение 0 |
| startDateIsSet | bool | задана ли дата начала работы |  |
| startDate | Date | дата начала работы | в интерфейсе ПланФикс поле _приступить к работе_ |
| startTimeIsSet | bool | задано ли время начала работы |  |
| startTime | Time | время начала работы | в интерфейсе ПланФикс поле _приступить к работе_ |
| endDateIsSet | bool | задана ли дата завершения работы |  |
| endDate | Date | дата завершения работы | в интерфейсе ПланФикс поле _закончить работу До_ |
| endTimeIsSet | bool | задано ли время завершения работы |  |
| endTime | Time | время завершения работы | в интерфейсе ПланФикс поле _закончить работу До_ |
| isSummary | bool - 0/1 | задача является суммарной |  |
| durationIsSet | bool - 0/1 | задана ли длительность |  |
| duration | int | длительность |  |
| durationUnit | int | 0 - минуты, 1 - часы, 2 - дни |  |
| workers |  | корневой элемент списка исполнителей задачи |  |
| workers.users |  | корневой элемент списка пользователей, которым поставлена задача |  |
| workers.users.id | int | идентификатор пользователя которому поставлена задача |  |
| workers.groups |  | корневой элемент списка групп, которым поставлена задача |  |
| workers.groups.id | int | идентификатор группы |  |
| members |  | корневой элемент списка участников задачи |  |
| members.users |  | корневой элемент списка участников задачи |  |
| members.users.id | int | идентификатор участника задачи |  |
| members.groups |  | корневой элемент списка групп участников |  |
| members.groups.id | int | идентификатор группы участников |  |
| auditors |  | корневой элемент списка аудиторов задачи, содержимое аналогично workers и members |  |
| customData |  | значения пользовательских полей задачи |  |
| customData.customValue.id |  | идентификатор пользовательского поля задачи |  |
| customData.customValue.value |  | значение пользовательского поля задачи | (для полей типа набор задач, список сотрудников, набор записей справочника \- идентификаторы через запятую в квадратных скобках) |

Добавляемые даты могут задаваться в двух форматах. Первый формат короткий, указывается только число, год и месяц. Второй формат \- полный, дополнительно указывается время начала/завершения, если того требует задача.

Не указанные параметры (за исключением **id**) заменяются значениями по умолчанию.
В результате выполнения запроса данные задачи обновляются на указанные в запросе.

Ответ при удачном выполнении функции:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="ok">
  <task>
    <id></id>
  </task>
</response>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| task.id | int | идентификатор обновляемой задачи |  |

В противном случае будет возвращен ответ с ошибкой:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="error">
  <code></code>
</response>
```

---

## task.updateCustomData — Обновление пользовательских полей

Функция обновления пользовательских полей задачи. Может использоваться, если у пользователя нет прав на изменение задачи и поэтому использовать функцию task.update он не может, но при этом есть права на изменение пользовательских полей.

Формат запроса:

```
<?xml version="1.0" encoding="UTF-8"?>
<request method="task.updateCustomData">
  <account></account>
  <sid></sid>
  <task>
    <id></id>
    <general></general>
    <customData>
      <customValue>
        <id></id>
        <value></value>
      </customValue>
      <!-- ... -->
    </customData>
  </task>
  <signature></signature>
</request>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| id | int | идентификатор обновляемой задачи |  |
| general | int | номер задачи (если задан, используется вместо id) |  |
| customData |  | значения пользовательских полей задачи |  |
| customData.customValue.id |  | идентификатор пользовательского поля задачи |  |
| customData.customValue.value |  | значение пользовательского поля задачи | Для полей типа набор задач, список сотрудников, набор записей справочника \- идентификаторы через запятую в квадратных скобках. Если необходимо задать несколько значений для поля типа набор значений \- \["значение\_1", "значение\_2"\]. |

Ответ при удачном выполнении функции:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="ok">
  <task>
    <id></id>
  </task>
</response>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| task.id | int | идентификатор обновляемой задачи |  |

В противном случае будет возвращен ответ с ошибкой:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="error">
  <code></code>
</response>
```

---

## task.get — Получение карточки задачи

Функция получения карточки задачи. Формат запроса:

```
<?xml version="1.0" encoding="UTF-8"?>
<request method="task.get">
  <account></account>
  <sid></sid>
  <task>
    <id></id>
    <general></general>
  </task>
  <signature></signature>
</request>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| task.id | int | идентификатор задачи, информацию которой хотим получить |  |
| general | int | номер задачи (если задан, используется вместо id) |  |
| signature | string(32) | подпись запроса |  |

Результат успешного выполнения запроса:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="ok">
  <account></account>
  <sid></sid>
  <task>
    <id></id>
    <title></title>
    <description></description>
    <importance></importance>
    <status></status>
    <checkResult></checkResult>
    <type></type>
    <owner>
      <id></id>
      <name></name>
    </owner>
    <parent>
      <id></id>
    </parent>
    <template>
      <id></id>
    </template>
    <project>
      <id></id>
      <title></title>
    </project>
    <client>
      <id></id>
      <name></name>
    </client>
    <beginDateTime></beginDateTime>
    <startTime></startTime>
    <endTime></endTime>
    <duration></duration>
    <durationUnit></durationUnit>

    <general></general>

    <isOverdued></isOverdued>
    <isCloseToDeadline></isCloseToDeadline>
    <isNotAcceptedInTime></isNotAcceptedInTime>
    <isSummary></isSummary>
    <starred></starred>
    <additionalDescriptionData></additionalDescriptionData>

    <workers>
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
    </workers>
    <members>
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
    </members>
    <auditors>
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
    </auditors>
    <periodicity>
      <!-- ежедневно -->
      <daily>
        <type></type>
        <shift></shift>
      </daily>
      <!-- еженедельно -->
      <weekly>
        <type></type>
        <shift></shift>
        <days></days>
      </weekly>
      <!-- ежемесячно -->
      <monthly>
        <type></type>
        <month></month>
        <day></day>
        <dayType></dayType>
      </monthly>

      <startDate></startDate>
      <endCondition>
        <type></type>
        <date></date>
        <repeatCount></repeatCount>
      </endCondition>
      <notify>
        <type></type>
        <day></day>
      </notify>
    </periodicity>
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
  </task>
  <signature></signature>
</response>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| id | int | идентификатор задачи |  |
| title | string | название задачи |  |
| description | string | о чем задача, описание |  |
| importance | enum | срочность | перечень допустимых значений смотри в разделе срочность задач |
| status | enum | статус задачи | перечень допустимых значений смотри в разделе статусы задач |
| statusSet | int | идентификатор процесса задачи |  |
| checkResult | bool | является ли задача задачей с обязательной проверкой результата |  |
| type | string | тип задачи: task / template / checklist item |  |
| owner |  | создатель задачи |  |
| owner.id | int | идентификатор пользователя |  |
| owner.name | string | имя пользователя |  |
| parent |  | надзадача |  |
| parent.id | int | идентификатор задачи, которая будет являться над задачей | 0 (ноль) - над задача отсутствует |
| template |  | шаблон |  |
| template.id | int | идентификатор шаблона задачи |  |
| project |  | в рамках какого проекта поставлена задача |  |
| project.id | int | идентификатор проекта |  |
| project.title | string | заголовок проекта |  |
| client |  | контрагент |  |
| client.id | int | идентификатор контрагента |  |
| client.name | string | имя контрагента |  |
| beginDateTime | DateTime | время создания задачи |  |
| startTime | DateTime | время начала работы | в интерфейсе ПланФикс поле _приступить к работе_ |
| endTime | DateTime | время окончания задачи | в интерфейсе ПланФикс поле _закончить работу До_ |
| duration | int | длительность задачи | в интерфейсе ПланФикс поле _Длительность_ |
| durationUnit | int | 0 - минуты, 1 - часы, 2 - дни |  |
| general | int | сквозной номер |  |
| isOverdued | bool | задача не выполнена в срок |  |
| isCloseToDeadline | bool | задача близка к дедлайну |  |
| isNotAcceptedInTime | bool | задача не принята вовремя |  |
| isSummary | bool | задача суммарная |  |
| starred | bool | помещена в избранные |  |
| additionalDescriptionData | string | дополнительные данные для задач из соц. сетей, которые отображаются в описании задачи |  |
| workers |  | корневой элемент списка исполнителей задачи |  |
| workers.users |  | корневой элемент списка пользователей, которым поставлена задача |  |
| workers.users.user | node | пользователь |  |
| workers.users.user.id | int | идентификатор пользователя, которому поставлена задача |  |
| workers.users.user.name | string | имя пользователя |  |
| workers.groups |  | корневой элемент списка групп, которым поставлена задача |  |
| workers.groups.group | node | группа |  |
| workers.groups.group.id | int | идентификатор группы |  |
| workers.groups.group.name | string | название группы |  |
| members |  | корневой элемент списка участников задачи, содержимое аналогично списку workers |  |
| auditors |  | корневой элемент списка аудиторов задачи, содержимое аналогично списку workers |  |
| periodicity | node | задает периодичность выполнения задачи | смотри описание структуры узлов _daily_, _weekly_ и _monthly_ ниже; отсутствие параметра, говорит о том что периодичность отсутствует |
| periodicity.startDate | DateTime | начиная с этой даты начинает работать повторение задачи |  |
| periodicity.endCondition |  | условия окончания повторения |  |
| periodicity.endCondition.type | enum | условие окончания | допустимые значения: **ENDLESS** \- нет конечной даты, **BYCOUNT** \- После _repeatCount_ повторений, **BYENDDATE** \- до даты определенной в **date** |
| periodicity.endCondition.date | DateTime | дата, после которой повторение задачи перестает работать | используется при type=BYENDDATE |
| periodicity.endCondition.repeatCount | int | количество повторений, после которого задача перестает повторяться | используется при type=BYCOUNT |
| periodicity.notify |  | уведомления |  |
| periodicity.notify.type | int | тип период, 0 - рабочий день, 1 - неделя |  |
| periodicity.notify.day | int | размер период | если type=0 и day=2, то уведомление прийдет за 2 рабочих дня до начала задачи |
| customData |  | значения пользовательских полей задачи |  |
| customData.customValue.field.id |  | идентификатор пользовательского поля |  |
| customData.customValue.field.name |  | название пользовательского поля |  |
| customData.customValue.value |  | значение пользовательского поля |  |
| customData.customValue.text |  | текстовое значение пользовательского поля |  |

Периодичность \- не обязательный параметр. Внутри тега **periodicity** может быть только один из перечисленных элементов: _daily_, _weekly_, _monthly_.

### описание параметра periodicity

Описание параметра _daily_, параметры _weekly_ и _monthly_ в этом случае не могут быть заданы. Указывает что задача должна повторяться ежедневно, согласно установленным критериям:

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| type | enum | определяет периодичность | допустимые значения EVERY или EVERY\_WORKING, или AFTER\_COMPLETE |
| shift | int | определяет сдвиг в днях | используется только при значениях type равным EVERY или AFTER\_COMPLETE |

Значение EVERY интерпретируется как каждый N-й день, заданный в параметре **shift**. EVERY\_WORKING - каждый рабочий день. Значение AFTER\_COMPLETE интерпретируется как ставить новую задачу через N-й день после каждого завершения, заданный в параметре **shift**.

Описание параметра _weekly_, параметры _daily_ и _monthly_ в этом случае не могут быть заданы. Указывает что задача должна повторяться еженедельно, согласно установленным критериям:

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| type | enum | определяет периодичность | допустимые значения EVERY или AFTER\_COMPLETE |
| shift | int | сдвиг в неделях |  |
| days | set/list | перечень дней недели, разделитель символ запятой (,). понедельник имеет индекс 1, воскресение - 7. | используется только при type=EVERY |

Значение AFTER\_COMPLETE интерпретируется как: ставить задачу через N-й неделю после каждого завершения, заданную в параметре **shift**.

Описание параметра _monthly_, параметры _daily_ и _weekly_ в этом случае не могут быть заданы. Указывает что задача должна повторяться ежемесяно, согласно установленным критериям:

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| type | enum | периодичность | допустимые значения AFTER\_COMPLETE или EVERY, или EXACT |
| month | int | задает месяц в/через который должно действие/задача повторяться |  |
| day | int | задает день в/через который должна задача повторяться | не используется при type=AFTER\_COMPLETE |
| dayType | enum | определяет тип дня | используется при type=EXACT. Допустимые значения смотри в разделе тип дня для повторяющейся задачи |

EXACT - ежемесячно в **day** день **dayType** каждого **month** месяца;

EVERY - ежемесячно повторять **day** числа каждого **month** месяца;

AFTER\_COMPLETE - ежемесячно ставить новую задачу через **month** месяц после каждого завершения.

В противном случае будет возвращен ответ с ошибкой:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="error">
  <code></code>
</response>
```

---

## task.getMulti — Получение нескольких задач

Функция получения множества карточек задач. Позволяет получить данные до 100 задач за запрос. Формат запроса:

```
<?xml version="1.0" encoding="UTF-8"?>
<request method="task.getMulti">
  <account></account>
  <sid></sid>
  <tasks>
    <id></id>
    <id></id>
    <general></general>
    <general></general>
    <!-- ... -->
  </tasks>
  <signature></signature>
</request>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| tasks.id | int | идентификаторы задач, информацию о которых хотим получить |  |
| tasks.general | int | номера задач, информацию о которых хотим получить |  |
| signature | string(32) | подпись запроса |  |

В случае удачного выполнения функции будет получен ответ следующего вида:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="ok">
  <tasks count="count">
    <task>
      <id></id>
      <!-- ... -->
    </task>
    <!-- ... -->
  </tasks>
</response>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| **tasks** |  | корневой элемент, содержит список задач |  |
| **tasks** count | int | количество задач возвращенных в результате выполнения функции |  |
| task |  | задача | описание данного параметра смотрите в секции ответ на получении карточки задачи |

В противном случае будет возвращен ответ с ошибкой:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="error">
  <code></code>
</response>
```

---

## task.getList — Список задач

Функция получения списка задач. В зависимости от значений параметров, можно получить список задач упорядоченных по разным признакам. Формат запроса:

```
<?xml version="1.0" encoding="UTF-8"?>
<request method="task.getList">
  <account></account>
  <sid></sid>
  <user>
    <id></id>
  </user>
  <target></target>
  <project>
    <id></id>
    <withSubprojects></withSubprojects>
  </project>
  <parent>
    <id></id>
  </parent>
  <sort></sort>
  <status></status>
  <pageCurrent></pageCurrent>
  <pageSize></pageSize>
  <filter></filter>
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
  <signature></signature>
</request>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| user |  | если указан этот параметр, то результатом будет список задач для указанного пользователя | допустим только для пользователей с правами администратора |
| user.id | int | идентификатор пользователя |  |
| target | enum / int | входящие, исходящие, все или заданный фильтр задач | допустимые значения смотри ниже |
| project |  | фильтр по проекту | необязательный параметр |
| project.id | int | идентификатор проекта |  |
| project.withSubprojects | bool - 0/1 | включая задачи подпроектов | необязательный, значение по\-умолчанию \- 0 |
| parent |  | надзадача | необязательный параметр, если задан выбор идёт из подзадач указанной задачи (из всего дерева вниз) |
| parent.id | int | идентификатор надзадачи |  |
| sort | enum | тип сортировка | список допустимых значений смотри в разделе типы сортировок задач |
| status | enum | статус | перечень допустимых значений смотри в разделе статусы задач |
| pageCurrent | int | текущая страница | 0 - используется для получения количества задач |
| pageSize | int | размер возвращаемого списка (максимум 100) | 0 - используется значение по умолчанию |
| filter | set | дополнительный фильтр, допустимые значения смотри ниже |  |
| filters |  | дополнительные сложные фильтры | перечень и формат допустимых значений смотри в разделе фильтры задач |

### Допустимые значения параметра target

| Значение | Описание | Примечание |
| --- | --- | --- |
| all | все | значение по умолчанию |
| in | входящие |  |
| out | исходящие |  |
| template | шаблоны |  |
| periodic | шаблоны повторяющихся задач |  |
| идентификатор фильтра задач | доступные фильтры задач можно получить функцией task.getFilterList |  |

### Допустимые значения для параметра filter

| Значение | Описание | Примечание |
| --- | --- | --- |
| ACTIVE | активные задачи |  |
| OVERDUE | просроченные задачи |  |
| MY | мои задачи |  |

Значение параметра может представлять комбинацию допустимых значений, например:

```
  <filter>ACTIVE MY</filter>
```

Результатом выполнения запроса будет список активных моих задач.

В случае удачного выполнения функции будет получен ответ следующего вида:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="ok">
  <tasks count="count" totalCount="totalCount">
    <task>
      <id></id>
      <!-- ... -->
    </task>
    <!-- ... -->
  </tasks>
</response>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| **tasks** |  | корневой элемент, содержит список задач |  |
| **tasks** count | int | количество задач возвращенных в результате выполнения функции |  |
| **tasks** totalCount | int | количество задач удовлетворяющих условиям запроса |  |
| task |  | задача, описание данного параметра смотрите в секции ответ на получении карточки задачи , с тем отличием, что функция task.getList не возвращает аудиторов и участников задачи |  |

В противном случае будет возвращен ответ с ошибкой:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="error">
  <code></code>
</response>
```

---

## task.getFilterList — Список фильтров

Функция позволяет получить список доступных текущему сотруднику фильтров задач для функции task.getList. Формат вызова функции:

```
<?xml version="1.0" encoding="UTF-8"?>
<request method="task.getFilterList">
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
  <taskFilterList totalCount="x">
    <taskFilter>
      <ID></ID>
      <Name></Name>
    </taskFilter>
    <taskFilter>
      <ID></ID>
      <Name></Name>
    </taskFilter>
    <!-- -->
  </taskFilterList>
</response>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| **taskFilterList** |  | список фильтров задач |  |
| **taskFilterList** totalCount | int | количество элементов в списке |  |
| taskFilter |  | корневой элемент описывающий фильтр задач |  |
| taskFilter.ID | int\|string | идентификатор фильтра (числовые идентификаторы у пользовательских фильтров и текстовые идентификаторы, начинающиеся с ":" \- у системных фильтров Все, Входящие и т.п.) |  |
| taskFilter.Name | string | название фильтра |  |

В противном случае будет возвращен ответ с ошибкой:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="error">
  <code></code>
</response>
```

---

## task.getPossibleStatusToChange — Доступные статусы для смены

Функция позволяет получить список доступных статусов для функции task.changeStatus. Формат вызова функции:

```
<?xml version="1.0" encoding="UTF-8"?>
<request method="task.getPossibleStatusToChange">
  <account></account>
  <sid></sid>
  <task>
    <id></id>
  </task>
  <signature></signature>
</request>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| task.id | int | идентификатор задачи |  |
| signature | string(32) | подпись |  |

Ответ:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="ok">
  <statusList totalCount="x">
    <status>
      <title></title>
      <value></value>
    </status>
    <status>
      <title></title>
      <value></value>
    </status>
    <!-- -->
  </statusList>
</response>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| **statusList** |  | список статусов |  |
| **statusList** totalCount | int | количество элементов в списке |  |
| status |  | корневой элемент описывающий статус |  |
| status.value | enum | значение | список принимаемых значений в разделе статусы задач |
| status.title | string | текстовое представление статуса |  |

В противном случае будет возвращен ответ с ошибкой:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="error">
  <code></code>
</response>
```

---

## task.changeStatus — Изменение статуса

Изменение статуса задачи. Формат вызова функции:

```
<?xml version="1.0" encoding="UTF-8"?>
<request method="task.changeStatus">
  <account></account>
  <sid></sid>
  <task>
    <id></id>
    <general></general>
  </task>
  <status></status>
  <dateTime></dateTime>
  <signature></signature>
</request>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| task.id | int | идентификатор задачи |  |
| general | int | номер задачи (если задан, используется вместо id) |  |
| status | enum | новый статус задачи | Допустимы значения из раздела Системные статусы задач или идентификаторы статусов, полученные в результате вызова функции taskStatus.getListOfSet. Недопустимое значение параметра: **REJECTED** (отклонить можно специальной функцией) |
| dateTime | DateTime | дата активации при переводе в статус _Отложенная_ | обязательное поле для статуса **DELAYED** |
| signature | string(32) | подпись |  |

Ответ:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="ok">
  <task>
    <id></id>
  </task>
</response>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| task.id | int | идентификатор задачи, у которой поменян статус |  |

В противном случае будет возвращен ответ с ошибкой:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="error">
  <code></code>
</response>
```

---

## task.accept — Принять задачу

Для дальнейшей работы с задачей, пользователь должен принять задачу. Вызов функции:

```
<?xml version="1.0" encoding="UTF-8"?>
<request method="task.accept">
  <account></account>
  <sid></sid>
  <task>
    <id></id>
  </task>
  <signature></signature>
</request>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| task.id | int | идентификатор задачи, которую принимает пользователь |  |
| signature | string(32) | подпись |  |

Результатом корректного выполнения запроса будет:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="ok">
  <task>
    <id></id>
  </task>
</response>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| task.id | int | идентификатор задачи, которую принял пользователь |  |

В противном случае будет возвращен ответ с ошибкой:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="error">
  <code></code>
</response>
```

---

## task.reject — Отклонить задачу

Для отклонения задачи, необходимо вызвать следующую функцию:

```
<?xml version="1.0" encoding="UTF-8"?>
<request method="task.reject">
  <account></account>
  <sid></sid>
  <task>
    <id></id>
  </task>
  <reason></reason>
  <signature></signature>
</request>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| task.id | int | идентификатор задачи, которую отклоняет пользователь |  |
| reason | string | причина | обязательное поле, не может быть пустым |
| signature | string(32) | подпись |  |

Результатом удачного выполнения функции будет следующий ответ:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="ok">
  <task>
    <id></id>
  </task>
</response>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| task.id | int | идентификатор задачи, которую отклонил пользователь |  |

В противном случае будет возвращен ответ с ошибкой:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="error">
  <code></code>
</response>
```

---

## task.changeWorkers — Изменение исполнителей

Функция используется для изменения списка исполнителей задачи. Формат вызова функции:

```
<?xml version="1.0" encoding="UTF-8"?>
<request method="task.changeWorkers">
  <account></account>
  <sid></sid>
  <task>
    <id></id>
    <workers>
      <addUsers>
        <id></id>
        <id></id>
        <!-- ... -->
      </addUsers>
      <addGroups>
        <id></id>
        <id></id>
        <!-- ... -->
      </addGroups>
      <delUsers>
        <id></id>
        <id></id>
        <!-- ... -->
      </delUsers>
      <delGroups>
        <id></id>
        <id></id>
        <!-- ... -->
      </delGroups>
    </workers>
  </task>
  <signature></signature>
</request>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| task.id | int | идентификатор задачи |  |
| workers |  | корневой элемент списка исполнителей задачи |  |
| workers.addUsers |  | корневой элемент списка пользователей, которые подключаются к задаче |  |
| workers.addUsers.id | int | идентификатор пользователя, который подключается к задаче |  |
| workers.addGroups |  | корневой элемент списка групп, которые подключаются к задаче |  |
| workers.addGroups.id | int | идентификатор группы |  |
| workers.delUsers |  | корневой элемент списка пользователей, которые удаляются из списка исполнителей |  |
| workers.delUsers.id | int | идентификатор пользователя, который удаляется из списка исполнителей |  |
| workers.delGroups |  | корневой элемент списка групп, которые удаляются из списка исполнителей |  |
| workers.delGroups.id | int | идентификатор группы |  |
| signature | string(32) | подпись |  |

Ответ:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="ok">
  <task>
    <id></id>
  </task>
</response>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| task.id | int | идентификатор задачи |  |

В противном случае будет возвращен ответ с ошибкой:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="error">
  <code></code>
</response>
```

---

## task.changeExpectDate — Изменение плановой даты

Если пользователь по какой-то причине не может выполнить в установленный срок задачу, он может перенести время выполнения её. Формат вызова функции:

```
<?xml version="1.0" encoding="UTF-8"?>
<request method="task.changeExpectDate">
  <account></account>
  <sid></sid>
  <task>
    <id></id>
  </task>
  <expectDate></expectDate>
  <signature></signature>
</request>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| task.id | int | идентификатор задачи |  |
| expectDate | DateTime | новая дата завершения задачи |  |
| signature | string(32) | подпись |  |

При успешном выполнении получим следующий ответ:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="ok">
  <task>
    <id></id>
    <endTime></endTime>
  </task>
</response>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| task.id | int | идентификатор задачи |  |
| task.endTime | DateTime |  | Если в ответе отсутствует параметр **endTime** \- это говорит о том, что был послан запрос постановщику с предложением о смене даты. |

В противном случае будет возвращен ответ с ошибкой:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="error">
  <code></code>
</response>
```

---

## taskStatus.getListOfSet

Функция для получения списка статусов процесса. Возвращает все статусы, которые присутствуют в наборе статусов процесса.
Для получения статусов в который можно перевести задачу в данный момент существует функция task.getPossibleStatusToChange

Формат запроса:

```
<?xml version="1.0" encoding="UTF-8"?>
<request method="taskStatus.getListOfSet">
  <account></account>
  <sid></sid>
  <taskStatusSet>
     <id></id>
  </taskStatusSet>
  <signature></signature>
</request>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| taskStatusSet.id | int | Идентификатор процесса |  |
| signature | string(32) | подпись |  |

Ответ:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="ok">
  <taskStatuses totalCount="totalCount">
    <taskStatus>
      <id></id>
      <name></name>
      <color></color>
      <isActive></isActive>
      <hasDeadline></hasDeadline>
      <texts>
        <text>
          <lang></lang>
          <name></name>
        </text>
        <!-- ... -->
      </texts>
    </taskStatus>
    <!-- ... -->
  </taskStatuses>
</response>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| **taskStatuses** |  | корневой элемент, содержит список статусов задач |  |
| **taskStatuses** totalCount | int | количество статусов в наборе процесса |  |
| taskStatus |  | корневой элемент, описывающий статус задачи |  |
| id | int | идентификатор статуса задачи |  |
| name | string | название статуса задачи |  |
| color | string | цвет |  |
| isActive | boolean | статус активен (1) или неактивен (0) |  |
| hasDeadline | boolean | отслеживаются (1) или не отслеживаются (0) сроки задач в этом статусе, если сроки в данном статусе отслеживаются и задача находится в данном статусе после даты планируемого завершения, она становится просроченной |  |
| texts |  | информация на доступных языках |  |
| text.lang | string | обозначение языка (на текущий момент Ru/En) |  |
| text.name | string | название статуса на этом языке |  |

Пустой ответ не **генерирует ошибку**. Если в результирующую выборку не попадают никакие статусы задач, то ответ будет иметь следующую форму:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="ok">
  <taskStatuses totalCount="0"></taskStatuses>
</response>
```

В противном случае будет возвращен ответ с ошибкой:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="error">
  <code></code>
</response>
```

---

## taskStatus.getSetList

Функция для получения списка процессов задач. Формат запроса:

```
<?xml version="1.0" encoding="UTF-8"?>
<request method="taskStatus.getSetList">
  <account></account>
  <sid></sid>
  <signature></signature>
</request>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| signature | string(32) | подпись |  |

Ответ:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="ok">
  <taskStatusSets totalCount="totalCount">
    <taskStatusSet>
      <id></id>
      <name></name>
    </taskStatusSet>
    <!-- ... -->
  </taskStatusSets>
</response>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| **taskStatusSets** |  | корневой элемент, содержит список процессов задач |  |
| **taskStatusSets** totalCount | int | количество процессов задач |  |
| taskStatusSet |  | корневой элемент, описывающий процесс в списке |  |
| id | int | идентификатор процесса |  |
| name | string | название процесса |  |

Пустой ответ не **генерирует ошибку**. Если в результирующую выборку не попадают никакие процессы, то ответ будет иметь следующую форму:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="ok">
  <taskStatusSets totalCount="0"></taskStatusSets>
</response>
```

В противном случае будет возвращен ответ с ошибкой:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="error">
  <code></code>
</response>
```

---

## Статусы задач

Функции для получения информации о доступных статусах задач и процессах задач:

1. taskStatus.getSetList/ Получить список процессов
2. taskStatus.getListOfSet/ Получить список статусов заданного процесса

---

## Системные статусы задач

Список системных значений для параметра **status** в задачах:

| Значение | Описание | Примечание |
| --- | --- | --- |
| **DRAFT** | Черновик | допустимый статус при создании |
| **ACTIVE** | Активный но еще не принятый | допустимый статус при создании |
| **ACCEPTED** | Принятый / В работу |  |
| **COMPLETED** | Завершенный |  |
| **DELAYED** | Отложенный | допустимый статус при создании |
| **REJECTED** | Отклоненный |  |
| **DONE** | Выполненный |  |
| **CANCELED** | Отмененный |  |

---

## Срочность задачи

Список допустимых значений для поля **importance** в задаче:

| Значение | Описание | Примечание |
| --- | --- | --- |
| **AVERAGE** | Обычная |  |
| **HIGH** | Срочная |  |

---

## Типы сортировок задач

Список допустимых значений для сортировки задач:

| Значение | Описание | Примечание |
| --- | --- | --- |
| NUMBER\_ASC | сортировка по номеру (возрастание) |  |
| NUMBER\_DESC | сортировка по номеру (убывание) |  |
| IMPORTANCE\_ASC | сортировка по приоритету (возрастание) |  |
| IMPORTANCE\_DESC | сортировка по приоритету (убывание) |  |
| DEADLINE\_ASC | сортировка по времени окончания (возрастание) |  |
| DEADLINE\_DESC | сортировка по времени окончания (убывание) |  |
| TASKTITLE\_ASC | сортировка по названию задачи (возрастание) |  |
| TASKTITLE\_DESC | сортировка по названию задачи (убывание) |  |
| PROJECT\_ASC | сортировка по названию проекта (возрастание) |  |
| PROJECT\_DESC | сортировка по названию проекта (убывание) |  |

---

## Фильтры задач (XML API)

Фильтры задач задаются следующим набором параметров:

- type - числовой идентификатор фильтра
- operator - оператор фильтра, одно из значений из списка (equal, notequal, gt, lt) у разных фильтров могут быть разные допустимые операторы.
- value - значение фильтра, может быть строкой, числом или сложным объектом, в зависимости от типа фильтра
- field - идентификатор пользовательского поля, для фильтров по пользовательским полям

| Тип | Название | Операторы | Формат value |
| --- | --- | --- | --- |
| 12 | Дата создания | - equal<br>- notequal<br>- gt<br>- lt | объект : <br>```<br><value><br>  <datetype></datetype><br>  <datevalue></datevalue><br>  <datefrom></datefrom><br>  <dateto></dateto><br></value><br>```<br>datetype принимает следующие значения:<br>- today - сегодня<br>- yesterday - вчера<br>- tomorrow - завтра<br>- thisweek - текущая неделя<br>- lastweek<br>- nextweek<br>- thismonth<br>- lastmonth<br>- nextmonth<br>- last - последние n дней, n передается в datevalue<br>- next - следующие n дней, n передается в datevalue<br>- in - через n дней, n передается в datevalue<br>- anotherdate - точная дата, дата передается в формате дд-мм-гггг в datefrom<br>- anotherperiod - точный период, даты передаются в формате дд-мм-гггг в datefrom и dateto<br>- anotherdate\_withtime - точная дата-время, дата передается в формате "дд-мм-гггг чч:мм" в datefrom<br>- anotherperiod\_withtime - точный период с заданным временем, даты передаются в формате "дд-мм-гггг чч:мм" в datefrom и dateto<br>даты считаются переданными в часовом поясе сотрудника, от имени которого сделан запрос<br>примеры:<br>```<br><value><br>  <datetype>thisweek</datetype><br></value><br>```<br>```<br><value><br>  <datetype>anotherperiod</datetype><br>  <datefrom>01-01-2015</datefrom><br>  <dateto>01-02-2015</dateto><br></value><br>```<br>```<br><value><br>  <datetype>anotherdate_withtime</datetype><br>  <datefrom>01-01-2015 12:00</datefrom><br></value><br>``` |
| 13 | Дата планируемого начала |
| 14 | Дата планируемого завершения |
| 21 | Дата последней активности (последнего добавленного комментария) |
| 19 | Дата фактического завершения |
| 20 | Дата выполнения |
| 38 | Дата последнего изменения |
| 79 | Дата последнего изменения или комментария |
| 103 | Пользовательское поле типа Дата |
| 1 | Постановщик | - equal<br>- notequal | int : идентификатор сотрудника |
| 2 | Исполнитель |
| 39 | Участник |
| 3 | Аудитор задачи или проекта |
| 59 | Аудитор проекта |
| 60 | Аудитор задачи |
| 108 | Пользовательское поле типа Контакт |
| 109 | Пользовательское поле типа Сотрудник |
| 112 | Пользовательское поле типа Группа, сотрудник, контакт |
| 113 | Пользовательское поле типа Список сотрудников |
| 22 | Без даты начала | - equal | int - 1 |
| 23 | Без даты завершения |
| 25 | С датой начала |
| 26 | С датой завершения |
| 16 | Повторяющаяся |
| 28 | Не повторяющаяся |
| 17 | Просроченная |
| 29 | Не просроченная |
| 33 | Без исполнителей |
| 41 | Без участников |
| 34 | Постановщик \- сотрудник |
| 35 | Постановщик \- контакт |
| 71 | Исполнитель \- сотрудник |
| 69 | Исполнитель \- контакт |
| 72 | Участник \- сотрудник |
| 70 | Участник \- контакт |
| 8 | Название задачи | - equal<br>- notequal | string - осуществляется фильтр содержит / не содержит |
| 101 | Пользовательское поле типа Строка |
| 102 | Пользовательское поле типа Число | - equal<br>- notequal<br>- gt<br>- lt | int |
| 105 | Пользовательское поле типа Чекбокс | - equal<br>- notequal | int - 1 / 0 |
| 106 | Пользовательское поле типа Список | - equal<br>- notequal | string |
| 107 | Пользовательское поле типа Справочник | - equal<br>- notequal | int - идентификатор записи |
| 114 | Пользовательское поле типа Набор записей справочника | - equal<br>- notequal | int - идентификатор записи, для условия по нескольким записям - идентификаторы через ; (точку с запятой) |
| 152 | Содержит значение в пользовательском поле | - equal | int - идентификатор поля |
| 153 | Не содержит значение в пользовательском поле | - equal | int - идентификатор поля |
| 11 | Содержит аналитику | - equal | int - идентификатор аналитики |
| 18 | Не содержит аналитику | - equal | int - идентификатор аналитики |
| 73 | Непосредственная надзадача | - equal<br>- notequal | int - идентификатор надзадачи |
| 51 | Шаблон | - equal<br>- notequal | int - идентификатор шаблона |
| 10 | Статус | - equal<br>- notequal | int - идентификатор статуса, для условия по нескольким статусам - идентификаторы через ; (точку с запятой) |
| 7 | Контрагент | - equal<br>- notequal | int - идентификатор контрагента ( id в ответах contact.get / contact.getList ) |
| 24 | Процесс | - equal<br>- notequal | int - идентификатор процесса ( id в ответах taskStatus.getSetList ) |

---

## Сложные фильтры задач (REST API)

Сложные фильтры в REST API ПланФикса применяются в методе «/task/list» при получении списка задач. Фильтры задач задаются следующим набором параметров:

- **type** — числовой идентификатор типа фильтра. Он определяется типом поля, по которому выполняется отбор в фильтре. Колонка «Тип» в таблице ниже.
- **operator** — оператор фильтра, одно из значений из списка (equal, notequal, gt, lt), у разных фильтров могут быть разные допустимые операторы, для полей типа «Дата» можно также использовать операторы gtAndEqual, ltAndEqual.
- **value** — значение фильтра, может быть строкой, числом или сложным объектом, в зависимости от типа фильтра.
- **field** — идентификатор пользовательского поля, для фильтров по пользовательским полям.
- **subfilter** — вложенный фильтр для фильтрации по значениям полей записи аналитик.

```
{
    "type": 12,
    "operator": "equal",
    "value": {
        "dateType": "otherDate",
        "dateValue": "22-03-2022"
    }
}
```

Пример запроса получения списка задач с передачей нескольких фильтров (используется логика И):

```
{
  "fields": "name",
  "filters": [{\
        "type": 2,\
        "operator": "equal",\
        "value": "user:5"\
     },\
     {\
        "type": 2,\
        "operator": "equal",\
        "value": "contact:7"\
     },\
     {\
        "type": 2,\
        "operator": "equal",\
        "value": "group:8"\
     }\
  ]
}
```

| Тип | Название | Операторы | Формат value |
| --- | --- | --- | --- |
| 12 | Дата создания | - equal<br>- notequal<br>- gt<br>- lt | Объект :<br>```<br> <br>"value": {<br>    "dateType": string,<br>    "dateValue": string,<br>    "dateFrom": string,<br>    "dateTo": string<br>}<br>```<br>dateType принимает следующие значения:<br>- today - сегодня<br>- yesterday - вчера<br>- tomorrow - завтра<br>- thisWeek - текущая неделя<br>- lastWeek<br>- nextWeek<br>- thisMonth<br>- lastMonth<br>- nextMonth<br>- last - последние n дней, n передается в dateValue<br>- next - следующие n дней, n передается в dateValue<br>- in - через n дней, n передается в dateValue<br>- otherDate - точная дата, дата передается в формате дд-мм-гггг в dateFrom<br>- otherRange - точный период, даты передаются в формате дд-мм-гггг в dateFrom и dateTo<br>- otherDate\_withTime - точная дата-время, дата передается в формате "дд-мм-гггг чч:мм" в dateFrom<br>- otherRange\_withTime - точный период с заданным временем, даты передаются в формате "дд-мм-гггг чч:мм" в dateFrom и dateTo<br>Даты считаются переданными в часовом поясе сотрудника, от имени которого сделан запрос.<br>Примеры:<br>```<br>"value": {<br>    "dateType": "thisWeek"<br>}<br>```<br>```<br>"value": {<br>    "dateType": "otherRange",<br>    "dateFrom": "20-03-2022",<br>    "dateTo": "22-03-2022"<br>}<br>```<br>```<br>"value": {<br>    "dateType": "otherDate_withTime",<br>    "dateFrom": "20-03-2022 12:00",<br>}<br>``` |
| 13 | Дата планируемого начала |
| 14 | Дата планируемого завершения |
| 21 | Дата последней активности (последнего добавленного комментария) |
| 19 | Дата фактического завершения |
| 20 | Дата выполнения |
| 38 | Дата последнего изменения |
| 79 | Дата последнего изменения или комментария |
| 103 | Пользовательское поле типа Дата |
| 1 | Постановщик | - equal<br>- notequal | string - номер сотрудника/контакта/группы с префиксом.<br>Например: “user:1”, “contact:5”, “group:3” |
| 2 | Исполнитель |
| 97 | Исполнитель персонально (без учета групп, в которые он входит) |
| 39 | Участник |
| 3 | Аудитор задачи или проекта |
| 59 | Аудитор проекта |
| 60 | Аудитор задачи |
| 108 | Пользовательское поле типа Контакт |
| 109 | Пользовательское поле типа Сотрудник |
| 112 | Пользовательское поле типа Группа, сотрудник, контакт |
| 113 | Пользовательское поле типа Список сотрудников |
| 22 | Без даты начала | - equal | int - 1<br>boolean - true |
| 23 | Без даты завершения |
| 25 | С датой начала |
| 26 | С датой завершения |
| 16 | Повторяющаяся |
| 28 | Не повторяющаяся |
| 17 | Просроченная |
| 29 | Не просроченная |
| 33 | Без исполнителей |
| 41 | Без участников |
| 34 | Постановщик \- сотрудник |
| 35 | Постановщик \- контакт |
| 71 | Исполнитель \- сотрудник |
| 69 | Исполнитель \- контакт |
| 72 | Участник \- сотрудник |
| 70 | Участник \- контакт |
| 8 | Название задачи | - equal<br>- notequal | string - осуществляется фильтр содержит / не содержит |
| 101 | Пользовательское поле типа Строка | - equal<br>- notequal<br>- have<br>- nothave | string - осуществляется фильтр равно / не равно / содержит / не содержит |
| 102 | Пользовательское поле типа Число | - equal<br>- notequal<br>- gt<br>- lt | int |
| 105 | Пользовательское поле типа Чек-бокс | - equal<br>- notequal | int - 1 / 0<br>boolean |
| 106 | Пользовательское поле типа Список | - equal<br>- notequal | string |
| 107 | Пользовательское поле типа Запись справочника | - equal<br>- notequal | int - идентификатор записи |
| 114 | Пользовательское поле типа Набор записей справочника | - equal<br>- notequal | int - идентификатор записи, для условия по нескольким записям — идентификаторы через ; (точку с запятой) |
| 111 | Пользовательское поле типа Набор значений | - equal<br>- notequal | string - значение, для условия по нескольким значениям - значения через ; (точку с запятой) |
| 152 | Содержит значение в пользовательском поле | - equal | int - идентификатор поля |
| 153 | Не содержит значение в пользовательском поле | - equal | int - идентификатор поля |
| 11 | Содержит аналитику | - equal | int - идентификатор аналитики |
| 18 | Не содержит аналитику | - equal | int - идентификатор аналитики |
| 73 | Непосредственная надзадача | - equal<br>- notequal | int - номер надзадачи |
| 57 | Номер задачи | - equal<br>- notequal | int - номер задачи или массив номеров задач для условия по нескольким номерам (ИЛИ) |
| 115 | Пользовательское поле типа Задача |
| 116 | Пользовательское поле типа Набор задач |
| 117 | Пользовательское поле типа Проект | - equal<br>- notequal | int - номер проекта |
| 51 | Шаблон | - equal<br>- notequal | int - номер шаблона или массив номеров шаблонов для условия по нескольким шаблонам(ИЛИ) |
| 325 | Объект | - equal<br>- notequal | int - номер объекта или массив номеров объектов для условия по нескольким объектам (ИЛИ) |
| 5 | Проект | - equal<br>- notequal | int - номер проекта |
| 10 | Статус | - equal<br>- notequal | int - идентификатор статуса или массив идентификаторов статусов для условия по нескольким статусам (ИЛИ) |
| 7 | Контрагент | - equal<br>- notequal | int - номер контрагента<br>string - номер контрагента с префиксом, пример: “contact:1” |
| 110 | Пользовательское поле Контрагент |
| 24 | Процесс | - equal<br>- notequal | int - идентификатор процесса |
| 307 | Дерево подзадач | - equal<br>- notequal | int - номер надзадачи |
| 9 | Срочность | - equal<br>- notequal | string - Urgent или NotUrgent |
| 93 | Значение поля записи аналитики | (в зависимости от типа поля)<br>- equal<br>- notequal<br>- gt<br>- lt<br>- have<br>- nothave | Значение поля аналитики по которому выполняется фильтрация, дополнительно в структуре фильтра надо передать поле subfilter, пример для фильтра поля аналитики типа Строка:<br>```<br>{<br>  "offset": 0,<br>  "pageSize": 100,<br>  "filters": [<br>    {<br>      "type": 93,<br>      "operator": "equal",<br>      "value": "Test value",<br>      "subfilter": {<br>        "dataTagId": 6,<br>        "filter": {<br>          "type": 3108,<br>          "field": 20<br>        }<br>      }<br>    }<br>  ],<br>  "fields": "id,name,dataTags"<br>}<br>```<br>где:<br>- dataTagId — идентификатор аналитики.<br>- filter — объект фильтра по этой аналитике.<br>- type — тип фильтра, в данном случае сложный фильтр аналитики по полю типа Строка.<br>- field — идентификатор поля аналитики, по которому выполняется фильтр. |

- REST API

---

## Связанные темы

- [[REST API — Комментарии (XML)]]
- [[REST API — Проекты (XML)]]
- [[REST API — Авторизация]]
- [[PlanFix API — Index]]
