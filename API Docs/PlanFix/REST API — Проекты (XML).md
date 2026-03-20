# REST API — Проекты (XML API)

← [[PlanFix API — Index]]

Функции XML API для управления проектами и группами проектов.
Auth: см. [[REST API — Авторизация]]

---

## Список функций

## Перейти

# ПланФикс API: Проекты

Функции позволяют управлять проектом:

1. project.add / Создать проект
2. project.update / Обновить данные по проекту
3. project.get / Получить информацию о проекте
4. project.getList / Список проектов

## Перейти

---

## project.add — Создать проект

## Перейти

# ПланФикс API project.add

Запрос на создание проекта имеет следующий вид:

```
<?xml version="1.0" encoding="UTF-8"?>
<request method="project.add">
  <account></account>
  <sid></sid>
  <project>
    <title></title>
    <description></description>
    <owner>
      <id></id>
    </owner>
    <client>
      <id></id>
    </client>
    <status></status>
    <hidden></hidden>
    <hasEndDate></hasEndDate>
    <endDate></endDate>
    <group>
      <id></id>
    </group>
    <parent>
      <id></id>
    </parent>
    <auditors>
      <id></id>
      <id></id>
      <!-- ... -->
    </auditors>
    <managers>
      <id></id>
      <id></id>
      <!-- ... -->
    </managers>
    <workers>
      <id></id>
      <id></id>
      <!-- ... -->
    </workers>
    <customData>
      <customValue>
        <id></id>
        <value></value>
      </customValue>
      <!-- ... -->
    </customData>
  </project>
  <signature></signature>
</request>
```

Если не передать автора (owner), или указать id=0, то будет использован в качестве автора пользователь текущей сессии. Контрагент не обязательный параметр. Вызывать функцию имеет право обычный пользователь (не контакт).

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| sid | string(32) | ключ сесии | выдается в результате прохождения аутентификации |
| title | string | Название проекта |  |
| description | string | Описание проекта которое задает пользователь |  |
| owner |  |  | Данное поле не обязательно. В этом случае будет назначен владельцем пользователь, от имени которого выполняется запрос (определяется по sid) |
| owner.id | int | идентификатор пользователя, который будет считаться создателем проекта. | допускается значение 0 (ноль). В этом случае будет назначен владельцем пользователь, от имени которого выполняется запрос (определяется по sid) |
| client |  |  | необязательный параметр |
| client.id | int | идентификатор контрагента | допускается значение 0 (ноль). |
| status | enum | статус создаваемого проекта | перечень допустимых значений для данного поля смотри в разделе статусы проектов |
| hidden | bool | скрытый |  |
| hasEndDate | bool | имеет ли дату окончания |  |
| endDate | DateTime |  | учитывается только в том случае, если параметр **hasEndDate** установлен в _true_ |
| group |  |  | необязательный параметр |
| group.id | int | идентификатор группы проектов |  |
| parent |  |  | необязательный параметр |
| parent.id | int | идентификатор надпроекта |  |
| auditors |  | аудиторы проекта | необязательный параметр |
| auditors.id | int | идентификатор аудитора проекта |  |
| managers |  | менеджеры проекта | необязательный параметр |
| managers.id | int | идентификатор менеджера проекта |  |
| workers |  | исполнители по умолчанию проекта | необязательный параметр |
| workers.id | int | идентификатор исполнителя по умолчанию проекта |  |
| customData |  | значения пользовательских полей проекта |  |
| customData.customValue.id |  | идентификатор пользовательского поля проекта |  |
| customData.customValue.value |  | значение пользовательского поля проекта | (для полей типа набор задач, список сотрудников, набор записей справочника \- идентификаторы через запятую в квадратных скобках) |

Ответ при успешном создании проекта:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="ok">
  <project>
    <id></id>
  </project>
</response>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| project.id | int | идентификатор созданного проекта |  |

В противном случае будет возвращен ответ с ошибкой:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="error">
  <code></code>
</response>
```

## Перейти

---

## project.update — Обновить данные проекта

## Перейти

# ПланФикс API project.update

Функция обновления данных о проекте. Формат запроса:

```
<?xml version="1.0" encoding="UTF-8"?>
<request method="project.update">
  <account></account>
  <sid></sid>
  <project>
    <id></id>
    <general></general>
    <title></title>
    <description></description>
    <owner>
      <id></id>
    </owner>
    <client>
      <id></id>
    </client>
    <status></status>
    <hidden></hidden>
    <hasEndDate></hasEndDate>
    <endDate></endDate>
    <group>
      <id></id>
    </group>
    <parent>
      <id></id>
    </parent>
    <auditors>
      <id></id>
      <id></id>
      <!-- ... -->
    </auditors>
    <managers>
      <id></id>
      <id></id>
      <!-- ... -->
    </managers>
    <workers>
      <id></id>
      <id></id>
      <!-- ... -->
    </workers>
    <members>
      <id></id>
      <id></id>
      <!-- ... -->
    </members>
    <customData>
      <customValue>
        <id></id>
        <value></value>
      </customValue>
      <!-- ... -->
    </customData>
  </project>
  <signature></signature>
</request>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| sid | string(32) | ключ сесии | выдается в результате прохождения аутентификации |
| id | int | Идентификатор проекта который редактируется | можно получить из функций получения списка или в результате выполнения функции добавления |
| general | int | Номер проекта который редактируется | используется при отсутствии параметра id |
| title | string | Название проекта | не обязательный параметр |
| description | string | Описание проекта которое задает пользователь | не обязательный параметр |
| owner |  |  | не обязательный параметр |
| owner.id | int | идентификатор пользователя, который будет считаться создателем проекта. | допускается значение 0 (ноль). не обязательный параметр |
| client |  |  | необязательное параметр |
| client.id | int | идентификатор контрагента | допускается значение 0 (ноль). |
| status | enum | новый статус проекта | перечень допустимых значений для данного поля смотри в разделе константы статусы проектов. не обязательный параметр |
| hidden | bool | скрытый | не обязательный параметр |
| hasEndDate | bool | имеет ли дату окончания | не обязательный параметр |
| endDate | DateTime |  | учитывается только в том случае, если параметр **hasEndDate** установлен в _true_. не обязательный параметр |
| group |  |  | необязательный параметр |
| group.id | int | идентификатор группы проектов |  |
| parent |  |  | необязательный параметр |
| parent.id | int | идентификатор надпроекта |  |
| auditors |  | аудиторы проекта | необязательный параметр |
| auditors.id | int | идентификатор аудитора проекта |  |
| managers |  | менеджеры проекта | необязательный параметр |
| managers.id | int | идентификатор менеджера проекта |  |
| workers |  | исполнители по умолчанию проекта | необязательный параметр |
| workers.id | int | идентификатор исполнителя по умолчанию проекта |  |
| members |  | участники по умолчанию проекта | необязательный параметр |
| members.id | int | идентификатор участника по умолчанию проекта |  |
| customData |  | значения пользовательских полей проекта |  |
| customData.customValue.id |  | идентификатор пользовательского поля проекта |  |
| customData.customValue.value |  | значение пользовательского поля проекта |  |

Необязательные параметры можно не передавать в запросе. В этом случае сохраняется старое значение.

Результатом удачного выполнения запроса является следующий ответ:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="ok">
  <project>
    <id></id>
  </project>
</response>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| id | int | идентификатор | равен идентификатору переданному в запросе |

В противном случае будет возвращен ответ с ошибкой:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="error">
  <code></code>
</response>
```

## Перейти

---

## project.get — Получить информацию о проекте

## Перейти

# ПланФикс API project.get

Функция позволяет получить информацию о проекте. Формат запроса:

```
<?xml version="1.0" encoding="UTF-8"?>
<request method="project.get">
  <account></account>
  <sid></sid>
  <project>
    <id></id>
    <general></general>
  </project>
  <signature></signature>
</request>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| project.id | int | идентификатор запрашиваемого проекта |  |
| project.general | int | номер проекта | используется при отсутствии параметра id |
| signature | string(32) | подпись |  |

Ответ:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="ok">
  <project>
    <id></id>
    <title></title>
    <description></description>
    <owner>
      <id></id>
      <name></name>
    </owner>
    <client>
      <id></id>
      <name></name>
    </client>
    <group>
      <id></id>
      <name></name>
    </group>
    <parent>
      <id></id>
    </parent>
    <template>
      <id></id>
    </template>
    <status></status>
    <hidden></hidden>
    <hasEndDate></hasEndDate>
    <endDate></endDate>
    <beginDate></beginDate>
    <taskCount></taskCount>
    <isOverdued></isOverdued>
    <isCloseToDeadline></isCloseToDeadline>

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
    </auditors>
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
  </project>
</response>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| id | int | идентификатор проекта |  |
| title | string | название проекта |  |
| description | string | описание проекта |  |
| owner |  | создатель/владелец проекта |  |
| owner.id | int | идентификатор пользователя создавшего проект |  |
| owner.name | string | имя пользователя создавшего проект |  |
| client |  | контрагент |  |
| client.id | int | идентификатор контрагента |  |
| client.name | string | имя контрагента |  |
| group |  | группа проектов |  |
| group.id | int | идентификатор группы проектов |  |
| group.name | string | название группы проектов |  |
| parent |  | надпроект |  |
| parent.id | int | идентификатор надпроекта |  |
| template |  | шаблон |  |
| template.id | int | идентификатор шаблона |  |
| status | enum | статус проекта | перечень допустимых значений для данного поля смотри в разделе статусы проектов |
| hidden | bool | скрытый ли проект | отображается ли он в общем списке |
| hasEndDate | bool | true/false - имеет ли проект дату завершения |  |
| endDate | DateTime | дата завершения проекта | имеет значение только если установлен флаг **hasEndDate** |
| beginDate | DateTime | дата создания проекта |  |
| taskCount | int | количество задач в проекте |  |
| isOverdued | bool | true/false - является ли проект просроченным |  |
| isCloseToDeadline | bool | true/false | осталось 25% времени до завершения или прошло 75% отведенного времени на выполнение его |
| workers |  | корневой элемент списка исполнителей по умолчанию проекта |  |
| workers.users |  | корневой элемент списка пользователей из числа исполнителей по умолчанию проекта |  |
| workers.users.user | node | пользователь |  |
| workers.users.user.id | int | идентификатор пользователя из числа исполнителей по умолчанию проекта |  |
| workers.users.user.name | string | имя пользователя |  |
| workers.groups |  | корневой элемент списка групп из числа исполнителей по умолчанию проекта |  |
| workers.groups.group | node | группа |  |
| workers.groups.group.id | int | идентификатор группы |  |
| workers.groups.group.name | string | название группы |  |
| members |  | корневой элемент списка участников по умолчанию проекта |  |
| auditors |  | корневой элемент списка аудиторов проекта |  |
| customData |  | значения пользовательских полей проекта |  |
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

## project.getList — Список проектов

## Перейти

# ПланФикс API project.getList

Функция для получения списка проектов. Формат запроса:

```
<?xml version="1.0" encoding="UTF-8"?>
<request method="project.getList">
  <account></account>
  <sid></sid>
  <user>
    <id></id>
  </user>
  <target></target>
  <status></status>
  <sortType></sortType>
  <pageCurrent></pageCurrent>
  <pageSize></pageSize>
  <client>
    <id></id>
  </client>
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
| user |  | пользователь ПланФикса | не обязательный параметр. задается для того чтоб посмотреть проекты в которых принимает участие указанный пользователь. этот параметр должен задаваться только в том случае, если выполняется запрос от учетной записи с админ правами |
| user.id | int | идентификатор пользователя |  |
| target | enum | фильтр по вкладкам | не обязательный параметр. список допустимых значений смотри в разделе фильтр вкладки для проектов |
| status | enum | фильтр по статусу | не обязательный параметр. перечень допустимых значений для данного поля смотри в разделе статусы проектов |
| sortType | enum | тип сортировки | не обязательный параметр. список допустимых значений смотри в разделе типы сортировок для проектов |
| pageCurrent | int | постраничная навигация | не обязательный параметр. значение меньше 1 или отсутствие этого параметра будут инициировать процедуру подсчета количества элементов. |
| pageSize | int | количество выдаваемых значений в результате. не может превышать значение **100** | не обязательный параметр. если будет опущен, возьмется значение по умолчанию. |
| client |  |  | не обязательный параметр |
| client.id | int | идентификатор контрагента, выступает в качестве фильтра |  |
| filters |  | дополнительные сложные фильтры | перечень и формат допустимых значений смотри в разделе фильтры проектов |
| signature | string(32) | подпись |  |

Все параметры, за исключением **account**, **sid**, **signature** не являются обязательными. Если опустить параметр **user**, то будет получен список проектов доступных для текущего пользователя, необходимо помнить что только администраторы могут смотреть проекты других участников.

Ответ:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="ok">
  <projects count="count" totalCount="totalCount">
    <project>
      <id></id>
      <title></title>
      <description></description>
      <owner>
        <id></id>
        <name></name>
      </owner>
      <client>
        <id></id>
        <name></name>
      </client>
      <group>
        <id></id>
        <name></name>
      </group>
      <parent>
        <id></id>
      </parent>
      <template>
        <id></id>
      </template>
      <status></status>
      <hidden></hidden>
      <hasEndDate></hasEndDate>
      <endDate></endDate>
      <beginDate></beginDate>
      <taskCount></taskCount>
      <isOverdued></isOverdued>
      <isCloseToDeadline></isCloseToDeadline>
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
    </project>
    <!-- ... -->
  </projects>
</response>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| **projects** |  | корневой элемент, содержит список проектов |  |
| **projects** count | int | количество узлов/проектов возвращенных в результате выполнения запроса |  |
| **projects** totalCount | int | количество проектов удовлетворяющих запросу |  |
| project |  | корневой элемент, описывающий проект в списке |  |
| id | int | идентификатор проекта |  |
| title | string | название проекта |  |
| description | string | описание проекта |  |
| owner |  | владелец/создатель проекта |  |
| owner.id | int | идентификатор пользователя |  |
| owner.name | string | имя пользователя, создавшего проект |  |
| client |  | контрагент |  |
| client.id | int | идентификатор контрагента |  |
| client.name | string | имя контрагента |  |
| group |  | группа проектов |  |
| group.id | int | идентификатор группы проектов |  |
| group.name | string | название группы проектов |  |
| parent |  | надпроект |  |
| parent.id | int | идентификатор надпроекта |  |
| template |  | шаблон |  |
| template.id | int | идентификатор шаблона |  |
| status | enum | статус в котором находится сейчас проект | перечень допустимых значений для данного поля смотри в разделе статусы проектов |
| hidden | bool | скрытый |  |
| hasEndDate | bool | признак того, что для проекта установлена дата окончания |  |
| endDate | DateTime | дата окончания проекта | установлена, только при **hasEndDate** = _true_ |
| beginDate | DateTime | дата создания проекта |  |
| taskCount | int | количество задач в проекте |  |
| isOverdued | bool | признак того, что проект просрочен |  |
| isCloseToDeadline | bool | осталось 25% времени до завершения или прошло 75% отведенного времени на выполнение его |  |
| customData |  | значения пользовательских полей задачи |  |
| customData.customValue.field.id |  | идентификатор пользовательского поля |  |
| customData.customValue.field.name |  | название пользовательского поля |  |
| customData.customValue.value |  | значение пользовательского поля |  |
| customData.customValue.text |  | текстовое значение пользовательского поля |  |

Атрибут **totalCount** у элемента **projects** показывает сколько всего может быть возвращено данным запросом проектов. Это позволяет избежать дополнительный запрос на полный подсчет количества проектов. Если требуется узнать только количество элементов, достаточно послать запрос с параметром **pageCurrent** =0.

Пустой ответ не **генерирует ошибку**. Если в результирующую выборку не попадают никакие проекты, то ответ будет иметь следующую форму:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="ok">
  <projects count="0" totalCount="0"></projects>
</response>
```

В противном случае будет возвращен ответ с ошибкой:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="error">
  <code></code>
</response>
```

## Перейти

---

## Группы проектов

## Перейти

# ПланФикс API: Группы проектов

Функции позволяют управлять группами проектов:

1. projectGroup.add / Создать группу проектов
2. projectGroup.update / Обновить данные по группам проектов
3. projectGroup.move / Переместить группу проектов
4. projectGroup.get / Получить информацию о группе проектов
5. projectGroup.getList / Список групп проектов

## Перейти

---

## projectGroup.add

## Перейти

# ПланФикс API projectGroup.add

Запрос на создание группы проектов. Выполнение данной функции разрешено только пользователю с админ правами. Имеет следующий вид:

```
<?xml version="1.0" encoding="UTF-8"?>
<request method="projectGroup.add">
  <account></account>
  <sid></sid>
  <projectGroup>
    <name></name>
  </projectGroup>
  <signature></signature>
</request>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| sid | string(32) | ключ сесии | выдается в результате прохождения аутентификации |
| name | string | Название группы проектов |  |

Ответ при успешном создании группы проектов:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="ok">
  <projectGroup>
    <id></id>
  </projectGroup>
</response>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| projectGroup.id | int | идентификатор созданной группы проектов |  |

В противном случае будет возвращен ответ с ошибкой:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="error">
  <code></code>
</response>
```

## Перейти

---

## projectGroup.get

## Перейти

# ПланФикс API projectGroup.get

Функция позволяет получить информацию о группе проектов. Формат запроса:

```
<?xml version="1.0" encoding="UTF-8"?>
<request method="projectGroup.get">
  <account></account>
  <sid></sid>
  <projectGroup>
    <id></id>
  </projectGroup>
  <signature></signature>
</request>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| projectGroup.id | int | идентификатор запрашиваемой группы проектов |  |
| signature | string(32) | подпись |  |

Ответ:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="ok">
  <projectGroup>
    <id></id>
    <name></name>
  </projectGroup>
</response>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| id | int | идентификатор группы проектов |  |
| name | string | название группы проектов |  |

В противном случае будет возвращен ответ с ошибкой:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="error">
  <code></code>
</response>
```

## Перейти

---

## projectGroup.getList

## Перейти

# ПланФикс API projectGroup.getList

Функция для получения списка групп проектов. Формат запроса:

```
<?xml version="1.0" encoding="UTF-8"?>
<request method="projectGroup.getList">
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
  <projectGroups totalCount="totalCount">
    <projectGroup>
      <id></id>
      <name></name>
    </projectGroup>
    <!-- ... -->
  </projectGroups>
</response>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| **projectGroups** |  | корневой элемент, содержит список групп проектов |  |
| **projectGroups** totalCount | int | количество групп проектов |  |
| projectGroup |  | корневой элемент, описывающий группу проектов в списке |  |
| id | int | идентификатор группы проектов |  |
| name | string | название группы проектов |  |

Пустой ответ не **генерирует ошибку**. Если в результирующую выборку не попадают никакие группы проектов, то ответ будет иметь следующую форму:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="ok">
  <projectGroups totalCount="0"></projectGroups>
</response>
```

В противном случае будет возвращен ответ с ошибкой:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="error">
  <code></code>
</response>
```

## Перейти

---

## projectGroup.update

## Перейти

# ПланФикс API projectGroup.update

Функция обновления данных о группе проектов:

```
<?xml version="1.0" encoding="UTF-8"?>
<request method="projectGroup.update">
  <account></account>
  <sid></sid>
  <projectGroup>
    <id></id>
    <name></name>
  </projectGroup>
  <signature></signature>
</request>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| sid | string(32) | ключ сесии | выдается в результате прохождения аутентификации |
| id | int | Идентификатор группы проектов |  |
| name | string | Название группы проектов |  |

Ответ при успешном изменении группы проектов:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="ok">
  <projectGroup>
    <id></id>
  </projectGroup>
</response>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| projectGroup.id | int | идентификатор измененной группы проектов |  |

В противном случае будет возвращен ответ с ошибкой:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="error">
  <code></code>
</response>
```

## Перейти

---

## projectGroup.move

## Перейти

# ПланФикс API projectGroup.move

Функция перемещения группы проектов в списке:

```
<?xml version="1.0" encoding="UTF-8"?>
<request method="projectGroup.move">
  <account></account>
  <sid></sid>
  <move>
    <projectGroup>
      <id></id>
    </projectGroup>
  </move>
  <after>
    <projectGroup>
      <id></id>
    </projectGroup>
  </after>
  <signature></signature>
</request>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| sid | string(32) | ключ сесии | выдается в результате прохождения аутентификации |
| move |  | Перемещаемая группа проектов |  |
| move.projectGroup.id | int | Идентификатор перемещаемой группы проектов |  |
| after |  | Группа, после которой в списке необходимо расположить перемещаемую группу проектов |  |
| after.projectGroup.id | int | Идентификатор группы проектов |  |

Ответ при успешном перемещении группы проектов:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="ok">
  <projectGroup>
    <id></id>
  </projectGroup>
</response>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| projectGroup.id | int | идентификатор перемещенной группы проектов |  |

В противном случае будет возвращен ответ с ошибкой:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="error">
  <code></code>
</response>
```

## Перейти

---

## Фильтры проектов

## Перейти

# ПланФикс API: Фильтры проектов

Фильтры проектов задаются следующим набором параметров:

- type - числовой идентификатор фильтра
- operator - оператор фильтра, одно из значений из списка (equal, notequal, gt, lt) у разных фильтров могут быть разные допустимые операторы.
- value - значение фильтра, может быть строкой, числом или сложным объектом, в зависимости от типа фильтра
- field - идентификатор пользовательского поля, для фильтров по пользовательским полям

| Тип | Название | Операторы | Формат value |
| --- | --- | --- | --- |
| 5005 | Дата завершения | - equal<br>- notequal<br>- gt<br>- lt | объект : <br>```<br><value><br>  <datetype></datetype><br>  <datevalue></datevalue><br>  <datefrom></datefrom><br>  <dateto></dateto><br></value><br>```<br>datetype принимает следующие значения:<br>- today - сегодня<br>- yesterday - вчера<br>- tomorrow - завтра<br>- thisweek - текущая неделя<br>- lastweek<br>- nextweek<br>- thismonth<br>- lastmonth<br>- nextmonth<br>- last - последние n дней, n передается в datevalue<br>- next - следующие n дней, n передается в datevalue<br>- in - через n дней, n передается в datevalue<br>- anotherdate - точная дата, дата передается в формате дд-мм-гггг в datefrom<br>- anotherperiod - точный период, даты передаются в формате дд-мм-гггг в datefrom и dateto<br>примеры:<br>```<br><value><br>  <datetype>thisweek</datetype><br></value><br>```<br>```<br><value><br>  <datetype>anotherperiod</datetype><br>  <datefrom>01-01-2015</datefrom><br>  <dateto>01-02-2015</dateto><br></value><br>``` |
| 5004 | Автор | - equal<br>- notequal | int : идентификатор сотрудника |
| 5008 | Клиент-менеджер |
| 5108 | Пользовательское поле типа Контакт |
| 5109 | Пользовательское поле типа Сотрудник |
| 5112 | Пользовательское поле типа Группа, сотрудник, контакт |
| 5113 | Пользовательское поле типа Список сотрудников |
| 5010 | Шаблон | - equal<br>- notequal | int : идентификатор шаблона проектов |
| 5014 | Надпроект | - equal<br>- notequal | int : идентификатор надпроекта |
| 5001 | Название проекта | - equal<br>- notequal | string - осуществляется фильтр содержит / не содержит |
| 5101 | Пользовательское поле типа Строка |
| 5102 | Пользовательское поле типа Число | - equal<br>- notequal<br>- gt<br>- lt | int |
| 5105 | Пользовательское поле типа Чекбокс | - equal<br>- notequal | int - 1 / 0 |
| 5106 | Пользовательское поле типа Список | - equal<br>- notequal | string |
| 5107 | Пользовательское поле типа Справочник | - equal<br>- notequal | int - идентификатор записи |
| 5114 | Пользовательское поле типа Набор записей справочника | - equal<br>- notequal | int - идентификатор записи, для условия по нескольким записям - идентификаторы через ; (точку с запятой) |

## Перейти

---

## Сложные фильтры проектов (REST API)

## Перейти

- REST API

# REST API: Сложные фильтры проектов

Сложные фильтры в REST API ПланФикса применяются в методе «/project/list» при получении списка проектов. Фильтры проектов задаются следующим набором параметров:

- **type** — числовой идентификатор фильтра.
- **operator** — оператор фильтра, одно из значений списка (equal, notequal, gt, lt). У разных фильтров могут быть разные допустимые операторы.
- **value** — значение фильтра, в зависимости от типа фильтра может быть строкой, числом или сложным объектом.
- **field** — идентификатор пользовательского поля аналитики, по которому выполняется фильтр.

Пример запроса получения списка проектов с передачей нескольких фильтров, по пользовательскому полю типа «Дата» и пользовательскому полю типа «Сотрудник» (используется логика И):

```
{
  "offset": 0,
  "pageSize": 100,
  "fields": "id,name,description,3,5",
  "filters": [\
    {\
      "type": 5103,\
      "field": 3,\
      "operator": "equal",\
      "value": {\
        "dateType": "otherRange",\
        "dateFrom": "15-12-2022",\
        "dateTo": "17-12-2022"\
      }\
    },\
    {\
      "type": 5109,\
      "field": 5,\
      "operator": "equal",\
      "value": "user:50"\
    }\
  ]
}
```

| Тип | Название | Операторы | Формат value |
| --- | --- | --- | --- |
| 5103 | Поле проекта типа «Дата» | - equal<br>- notequal<br>- gt<br>- lt | Объект :<br>```<br> <br>"value": {<br>    "dateType": string,<br>    "dateValue": string,<br>    "dateFrom": string,<br>    "dateTo": string<br>}<br>```<br>dateType принимает следующие значения:<br>- today - сегодня<br>- yesterday - вчера<br>- tomorrow - завтра<br>- thisWeek - текущая неделя<br>- lastWeek<br>- nextWeek<br>- thisMonth<br>- lastMonth<br>- nextMonth<br>- last - последние n дней, n передается в dateValue<br>- next - следующие n дней, n передается в dateValue<br>- in - через n дней, n передается в dateValue<br>- otherDate - точная дата, дата передается в формате дд-мм-гггг в dateFrom<br>- otherRange - точный период, даты передаются в формате дд-мм-гггг в dateFrom и dateTo<br>- otherDate\_withTime - точная дата-время, дата передается в формате "дд-мм-гггг чч:мм" в dateFrom<br>- otherRange\_withTime - точный период с заданным временем, даты передаются в формате "дд-мм-гггг чч:мм" в dateFrom и dateTo<br>Даты считаются переданными в часовом поясе сотрудника, от имени которого сделан запрос.<br>Примеры:<br>```<br>"value": {<br>    "dateType": "thisWeek"<br>}<br>```<br>```<br>"value": {<br>    "dateType": "otherRange",<br>    "dateFrom": "01-12-2022",<br>    "dateTo": "06-12-2022"<br>}<br>```<br>```<br>"value": {<br>    "dateType": "otherDate_withTime",<br>    "dateFrom": "30-12-2022 12:00",<br>}<br>``` |
| 5005 | Дата завершения |
| 5013 | Дата создания |
| 5004 | Автор | - equal<br>- notequal | string - номер сотрудника/контакта/группы с префиксом. <br>Например: “user:1”, “contact:5”, “group:3” |
| 5008 | Клиент-менеджер |
| 5012 | Аудитор |
| 5011 | Исполнитель |
| 5108 | Поле проекта типа «Контакт» |
| 5109 | Поле проекта типа «Сотрудник» |
| 5110 | Поле проекта типа «Контрагент» |
| 5112 | Поле проекта типа «Группа, сотрудник, контакт» |
| 5113 | Поле проекта типа «Список сотрудников» |
| 5001 | Название проекта | - equal<br>- notequal | string - осуществляется фильтр содержит / не содержит |
| 5101 | Поле проекта типа «Строка» | - equal<br>- notequal<br>- have<br>- nothave | string - осуществляется фильтр равно / не равно / содержит / не содержит |
| 5007 | Номер проекта | - equal<br>- notequal<br>- gt<br>- lt | int |
| 5102 | Поле проекта типа «Число» |
| 5105 | Поле проекта типа «Чек-бокс» | - equal<br>- notequal | int - 1 / 0<br>boolean |
| 5106 | Поле проекта типа «Список» | - equal<br>- notequal | string |
| 5107 | Поле проекта типа «Запись справочника» | - equal<br>- notequal | int - идентификатор записи |
| 5115 | Поле проекта типа «Задача» | - equal<br>- notequal | int - номер задачи |
| 5117 | Поле проекта типа «Проект» | - equal<br>- notequal | int - номер проекта |
| 5002 | Группа проекта | - equal<br>- notequal | int - идентификатор группы |
| 5006 | Статус проекта | - equal<br>- notequal | int - идентификатор статуса |
| 5010 | Шаблон | - equal<br>- notequal | int - номер шаблона |
| 5003 | Контрагент | - equal<br>- notequal | int - номер контрагента<br>string - номер контрагента с префиксом, например: "contact:1" |
| 5014 | Надпроект | - equal<br>- notequal | int - номер надпроекта |

## Перейти

- REST API

---

## Статусы проектов

## Перейти

# ПланФикс API: Статусы проектов

Список допустимых значений для статусов проекта:

| Значение | Описание | Примечание |
| --- | --- | --- |
| **DRAFT** | черновик |  |
| **COMPLETED** | завершен |  |
| **ACTIVE** | активен |  |

## Перейти

---

## Фильтр вкладки для проектов

## Перейти

# ПланФикс API: Фильтр вкладки для проектов

Список допустимых значений для параметра **target** (вкладки/цели):

| Значение | Описание | Примечание |
| --- | --- | --- |
| **all** | все | значение по умолчанию |
| **in** | проекты в которых участвует пользователь | вкладка "Я участвую" |
| **out** | проекты которые создал пользователь | вкладка "Я создал" |
| **template** | шаблоны проектов |  |

## Перейти

---

## Типы сортировок для проектов

## Перейти

# ПланФикс API: Типы сортировок для проектов

Список допустимых значений для параметра **sortType** (указывает в каком порядке будет произведена сортировка и по какому полю):

| Значение | Описание | Примечание |
| --- | --- | --- |
| **TITLE\_ASC** | по названию (алфавит) |  |
| **TITLE\_DESC** | по названию (обратный порядок) |  |
| **NUMBER\_ASC** | по номеру (по возрастанию) |  |
| **NUMBER\_DESC** | по номеру (по убыванию) |  |

## Перейти

---

## Типы сортировок для групп

## Перейти

# ПланФикс API: Типы сортировок для групп

Список допустимых значений для параметра **sortType** (указывает в каком порядке будет произведена сортировка и по какому полю):

| Значение | Описание | Примечание |
| --- | --- | --- |
| **NAME\_ASC** | по названию группы (алфавит) |  |
| **NAME\_DESC** | по названию группы (обратный порядок) |  |
| **USERCOUNT\_ASC** | по количеству участников в группе (возрастание) |  |
| **USERCOUNT\_DESC** | по количеству участников в группе (убывание) |  |

## Перейти

---

## Пример получения списка проектов на PHP

## Перейти

# ПланФикс API: Пример получения списка проектов на php

Простой пример получения списка проектов и вывод его.

```
<?php

$api_server = 'https://api.planfix.ru/xml/';
$api_key = 'APIKey-ВАШЕГО_ПРИЛОЖЕНИЯ';//
$api_token = 'ТОКЕН_АВТОРИЗАЦИИ';

/** получаем список доступных нам проектов и выводим его
 * используем функции на: http://goo.gl/E41Vv
 */
$requestXml = new SimpleXMLElement('<?xml version="1.0" encoding="UTF-8"?><request method="project.getList"><account></account></request>');

$requestXml->account = 'your_account';
$requestXml->pageCurrent = 1;
// остальные параметры являются необязательными, поэкспериментируйте сами

$ch = curl_init($api_server);

curl_setopt($ch, CURLOPT_FOLLOWLOCATION, true);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1); // не выводи ответ на stdout
curl_setopt($ch, CURLOPT_HEADER, 1);   // получаем заголовки
// не проверять SSL сертификат
curl_setopt($ch, CURLOPT_SSL_VERIFYPEER, 0);
// не проверять Host SSL сертификата
curl_setopt($ch, CURLOPT_SSL_VERIFYHOST, 0);

curl_setopt($ch, CURLOPT_HTTPAUTH, CURLAUTH_BASIC);
curl_setopt($ch, CURLOPT_USERPWD, $api_key . ':' . $api_token);

curl_setopt($ch, CURLOPT_POST, true);
curl_setopt($ch, CURLOPT_POSTFIELDS, $requestXml->asXML());
echo $requestXml->asXML();

$response = curl_exec($ch);
$error = curl_error($ch);
var_dump($error);

$header_size = curl_getinfo($ch, CURLINFO_HEADER_SIZE);
$responseBody = substr($response, $header_size);

curl_close($ch);

var_dump($responseBody);
?>
```

## Перейти

---

## Связанные темы

- [[REST API — Задачи (XML)]]
- [[REST API — Авторизация]]
- [[PlanFix API — Index]]
