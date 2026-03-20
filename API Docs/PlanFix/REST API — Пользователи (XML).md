# REST API — Пользователи и сотрудники (XML API)

← [[PlanFix API — Index]]

Функции XML API для управления сотрудниками и группами пользователей.
Auth: см. [[REST API — Авторизация]]

---

## Список функций (Сотрудники)

Функции управления своим личным профилем, управление профилем сотрудников.

1. user.add / Создать нового сотрудника
2. user.update / Обновить данные пользователя
3. user.get / Получить информацию о пользователе
4. user.getList / Список пользователей
5. user.updateGroupMembership / Изменить принадлежность к группе
6. user.changeStatus / Изменить/установить статус пользователя
7. user.actionsAdded / Получить количество добавленных пользователем действий за период

Список функций

---

## user.add — Добавить сотрудника

Функция добавления нового пользователя. Выполнение данной функции разрешено пользователю с админ правами. Формат запроса:

```
<?xml version="1.0" encoding="UTF-8"?>
<request method="user.add">
  <account></account>
  <sid></sid>
  <user>
    <name>Имя</name>
    <midName>Отчество</midName>
    <lastName>Фамилия</lastName>
    <email></email>
    <role></role>
    <status></status>
    <post>
      <id></id>
    </post>
    <phones>
        <phone>
            <number></number>
            <typeId></typeId>
            <typeName></typeName>
        </phone>
        <!-- ... -->
    </phones>
  </user>
  <signature></signature>
</request>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| name | string | имя пользователя |  |
| midName | string | отчество пользователя |  |
| lastName | string | фамилия пользователя |  |
| email | string | email пользователя |  |
| role | enum | роль пользователя в системе | допустимые значения _ADMIN_, **USER**. полный список смотри в разделе роли пользователей |
| post.id | int | идентификатор должности | не обязательное поле |
| status | enum | статус пользователя | список допустимых значений смотри в разделе статусы пользователей |
| phones | string | телефоны |  |
| phone.number | string | номер телефона |  |
| phone.typeId | int | идентификатор типа номера | допустимые значения можно получить функцией contact.getPhoneTypes |
| phone.typeName | string | название типа номера |  |
| signature | string(32) | подпись |  |

Ответ:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="ok">
  <user>
    <id></id>
  </user>
</response>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| id | int | идентификатор созданного пользователя |  |

В противном случае будет возвращен ответ с ошибкой:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="error">
  <code></code>
</response>
```

---

## user.update — Обновить данные сотрудника

Функция обновления данных пользователя. Формат запроса:

```
<?xml version="1.0" encoding="UTF-8"?>
<request method="user.update">
  <account></account>
  <sid></sid>
  <user>
    <id></id>
    <name></name>
    <midName></midName>
    <lastName></lastName>
    <email></email>
    <role></role>
    <status></status>
    <password></password>
    <birthdate></birthdate>
    <sex></sex>
    <phones>
        <phone>
            <number></number>
            <typeId></typeId>
            <typeName></typeName>
        </phone>
        <!-- ... -->
    </phones>
    <isInvisibleOutOfGroup></isInvisibleOutOfGroup>
    <isBlindOutOfGroup></isBlindOutOfGroup>
    <userPic></userPic>
    <post>
        <id></id>
    </post>
  </user>
  <signature></signature>
</request>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| id | int |  |  |
| name | string | имя пользователя |  |
| midName | string | отчество пользователя | необязательное поле |
| lastName | string | фамилия пользователя | необязательное поле |
| email | string | электронный адрес почты | необязательное поле |
| role | enum | роль пользователя в системе | необязательное поле. доступно для изменения только пользователям с правами администратор. полный список смотри в разделе роли пользователей |
| status | enum | статус | необязательное поле. доступно для изменения только пользователям с правами администратор. список допустимых значений смотри в разделе статусы пользователей |
| password | string | пароль | необязательное поле |
| birthdate | DateTime | дата рождения | необязательное поле |
| sex | enum | пол сотрудника | необязательное поле. список допустимых значений смотри в разделе пол сотрудника |
| phones | string | телефоны |  |
| phone.number | string | номер телефона |  |
| phone.typeId | int | идентификатор типа номера | допустимые значения можно получить функцией contact.getPhoneTypes |
| phone.typeName | string | название типа номера |  |
| isInvisibleOutOfGroup | bool | true=Видит только членов своих групп; false=Видит всех сотрудников | необязательное поле. доступно для изменения только пользователям с правами администратор |
| isBlindOutOfGroup | bool | true=Его видят только члены его групп; false=Его видят все сотрудники | необязательное поле. доступно для изменения только пользователям с правами администратор |
| userPic | string | base64 закодированная картинка | необязательное поле |
| post.id | int | должность которую занимает пользователь | необязательное поле |

Результат успешного выполнения функции:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="ok">
  <user>
    <id></id>
  </user>
</response>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| user.id | int | идентификатор обновляемого пользователя |  |

В противном случае будет возвращен ответ с ошибкой:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="error">
  <code></code>
</response>
```

---

## user.get — Получить информацию о сотруднике

Функция получения информации о пользователе. Формат запроса:

```
<?xml version="1.0" encoding="UTF-8"?>
<request method="user.get">
  <account></account>
  <sid></sid>
  <user>
    <id></id>
    <general></general>
  </user>
  <signature></signature>
</request>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| user.id | int | идентификатор пользователя | при отсутствии данного параметра и параметра general, возвращаются данные сотрудника, от которого происходит запрос |
| user.general | int | номер сотрудника |  |
| signature | string(32) | подпись |  |

Ответ:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="ok">
  <user>
    <id></id>
    <general></general>
    <name></name>
    <lastName></lastName>
    <midName></midName>
    <login></login>
    <email></email>
    <secondaryEmails>
        <email></email>
        <!-- ... -->
    </secondaryEmails>
    <role></role>
    <status></status>
    <birthdate></birthdate>
    <sex></sex>
    <telegramId></telegramId>
    <phones>
        <phone>
            <number></number>
            <typeId><typeId>
            <typeName><typeName>
        </phone>
        ...
    </phones>
    <isInvisibleOutOfGroup></isInvisibleOutOfGroup>
    <isBlindOutOfGroup></isBlindOutOfGroup>
    <userPic></userPic>
    <isOnline></isOnline>
    <timezone></timezone>
    <post>
        <id></id>
        <name></name>
    </post>
    <userGroups>
      <userGroup>
        <id></id>
        <name></name>
      </userGroup>
      <userGroup>
        <id></id>
        <name></name>
      </userGroup>
      <!-- ... -->
    </userGroups>
  </user>
</response>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| id | int | идентификатор сотрудника |  |
| general | int | номер сотрудника |  |
| name | string | имя, отчество пользователя |  |
| lastName | string | фамилия пользователя |  |
| midName | string | отчество пользователя |  |
| login | string | имя учетной записи в системе |  |
| email | string | электронный адрес почты |  |
| secondaryEmails.email |  | дополнительные адреса email, если есть |  |
| role | enum | роль пользователя в системе |  |
| status | enum | статус | список допустимых значений смотри в разделе статусы пользователей |
| birthdate | DateTime | дата рождения | если значение не установлено, то значение пусто |
| sex | enum | пол сотрудника | список допустимых значений смотри в разделе пол сотрудника, если значение не установлено, то значение пусто |
| phones |  | список телефонов |  |
| phones.phone.number | string | номер телефона |  |
| phones.phone.typeId | int | идентификатор типа номера телефона |  |
| phones.phone.typeName | string | название типа номера телефона |  |
| isInvisibleOutOfGroup | bool | true=Видит только членов своих групп; false=Видит всех сотрудников | доступно для пользователей с правами администратор |
| isBlindOutOfGroup | bool | true=Его видят только члены его групп; false=Его видят все сотрудники | доступно для пользователей с правами администратор |
| userPic | string | возвращает полный URL к картинке | если не установлен \- узел пустой |
| timezone | string | часовой пояс сотрудника |  |
| post |  | должность пользователя |  |
| post.id | int | идентификатор должности |  |
| post.name | string | название должности |  |
| userGroups |  | список групп в которых состоит пользователь |  |
| userGroups.userGroup |  | группа |  |
| userGroups.userGroup.id | int | идентификатор группы |  |
| userGroups.userGroup.name | string | название группы |  |
| telegramId | int | внутренний идентификатор в Telegram | возвращается только если включены уведомления в Telegram |

В противном случае будет возвращен ответ с ошибкой:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="error">
  <code></code>
</response>
```

---

## user.getList — Список сотрудников

\* ПланФикс API:Сотрудники \* Коды ошибок \* Список функций

Функция получения списка пользователей. Формат запроса:

```
<?xml version="1.0" encoding="UTF-8"?>
<request method="user.getList">
  <account></account>
  <sid></sid>
  <status></status>
  <onlyOnline></onlyOnline>
  <userGroup>
    <id></id>
  </userGroup>
  <sortType></sortType>
  <pageCurrent></pageCurrent>
  <pageSize></pageSize>
  <filters>
    <filter>
      <type></type>
      <operator></operator>
      <value></value>
    </filter>
    ...
  </filters>
</request>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| status | enum | статус пользователя | список допустимых значений смотри в разделе статусы пользователей. Игнорируется если задана группа |
| onlyOnline | bool | показывать только сотрудников online |  |
| userGroup.id | int | идентификатор группы (фильтр по группе) |  |
| sortType | enum | тип сортировки | список допустимых значений смотри в разделе типы сортировок пользователей |
| pageCurrent | int | текущая страница | 0 - используется для получения количества сотрудников |
| pageSize | int | размер получаемого списка | не больше 100 |
| filters |  | дополнительные фильтры | перечень и формат допустимых значений смотри в разделе фильтры сотрудников |

Ответ:

```
<response status="ok">
  <users count="count" totalCount="totalCount">
    <user>
      <!-- ... -->
    </user>
    <user>
      <!-- ... -->
    </user>
    <!-- ... -->
  </users>
</response>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| **users** |  | список пользователей |  |
| **users** count | int | количество пользователей в списке |  |
| **users** totalCount | int | количество пользователей удовлетворяющих условию запроса |  |
| users.user |  | описание пользователя, смотри полное описание структуры в разделе user.get |  |

В противном случае будет возвращен ответ с ошибкой:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="error">
  <code></code>
</response>
```

---

## user.changeStatus — Изменение статуса сотрудника

\* ПланФикс API:Сотрудники \* Коды ошибок \* Список функций

Позволяет установить статус, если передан/задан параметр **status**. Если параметр опущен, то статус будет поменян на противоположный установленному. Функция доступна пользователю с правами администратор. Формат запроса:

```
<?xml version="1.0" encoding="UTF-8"?>
<request method="user.changeStatus">
  <account></account>
  <sid></sid>
  <user>
    <id></id>
    <status></status>
  </user>
  <signature></signature>
</request>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| id | int | идентификатор пользователя |  |
| status | enum | новый статус | необязательный параметр. Список доступных значений смотри в разделе статусы пользователей |
| signature | string(32) | подпись |  |

В ответе будет передан параметр статус с указанием установленного статуса.

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="ok">
  <user>
    <id></id>
  </user>
</response>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| user.id | int | идентификатор пользователя |  |

В противном случае будет возвращен ответ с ошибкой:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="error">
  <code></code>
</response>
```

---

## user.actionsAdded — Действия пользователя

Позволяет получить количество добавленных пользователем действий за заданный период.
Формат запроса:

```
<?xml version="1.0" encoding="UTF-8"?>
<request method="user.actionsAdded">
  <account></account>
  <sid></sid>
  <user>
    <id></id>
  </user>
  <period>
    <begin></begin>
    <end></end>
  </period>
  <actionType></actionType>
  <signature></signature>
</request>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| user.id | int | идентификатор пользователя |  |
| period.begin | DateTime | дата начала периода |  |
| period.end | DateTime | дата конца периода |  |
| actionType | enum | тип действия, необязательный параметр | список возможных значений смотри в разделе типы действий |
| signature | string(32) | подпись |  |

В ответе будет передано количество действий указанного типа (либо же любого типа), добавленных пользователем в заданный период.

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="ok">
  <count></count>
</response>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| count | int | количество действий |  |

В противном случае будет возвращен ответ с ошибкой:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="error">
  <code></code>
</response>
```

---

## user.updateGroupMembership — Членство в группах

Изменить принадлежность пользователя к группе. Вызывать имеет право пользователь с правами администратор. Формат запроса:

```
<?xml version="1.0" encoding="UTF-8"?>
<request method="user.updateGroupMembership ">
  <account></account>
  <sid></sid>
  <user>
    <id></id>
    <addUserGroup>
      <id></id>
      <id></id>
      <!-- ... -->
    </addUserGroup>

    <delUserGroup>
      <id></id>
      <id></id>
      <!-- ... -->
    </delUserGroup>
  </user>
  <signature></signature>
</request>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| user.id | int | идентификатор пользователя |  |
| user.addUserGroup |  | список групп к которым присоединяется пользователь |  |
| user.addUserGroup.id | int | идентификатор группы |  |
| user.delUserGroup |  | список групп от которых пользователь отсоединяется |  |
| user.delUserGroup.id | int | идентификатор группы |  |

Ответ, при успешном выполнении запроса:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="ok">
  <user>
    <id></id>
  </user>
</response>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| user.id | int | идентификатор пользователя |  |

В противном случае будет возвращен ответ с ошибкой:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="error">
  <code></code>
</response>
```

---

## Управление группами пользователей

Создание, правка и удаление групп.
Функции **userGroup.add** и **userGroup.update** доступны учетной записи с правами Администратор.

1. userGroup.add / Создать группу
2. userGroup.update / Обновить
3. userGroup.get / Получить
4. userGroup.getList / Получить список групп
5. userGroup.getHeads / Получить список руководителей группы

Список функций

---

## userGroup.add

Функция создания группы. Функция доступна пользователю с правами администратор. Формат запроса:

```
<?xml version="1.0" encoding="UTF-8"?>
<request method="userGroup.add">
  <account></account>
  <sid></sid>
  <userGroup>
    <name></name>
  </userGroup>
  <signature></signature>
</request>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| userGroup.name | string | название создаваемой группы пользователей |  |
| signature | string(32) | подпись |  |

Ответ при удачном создании группы:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="ok">
  <userGroup>
    <id></id>
  </userGroup>
</response>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| userGroup.id | int | идентификатор созданной группы |  |

В противном случае будет возвращен ответ с ошибкой:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="error">
  <code></code>
</response>
```

---

## userGroup.get

Функция получения информации о группе. Формат вызова:

```
<?xml version="1.0" encoding="UTF-8"?>
<request method="userGroup.get">
  <account></account>
  <sid></sid>
  <userGroup>
    <id></id>
  </userGroup>
  <signature></signature>
</request>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| id | int | идентификатор группы |  |
| signature | string(32) | подпись |  |

Ответ:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="ok">
  <userGroup>
    <id></id>
    <name></name>
    <userCount></userCount>
  </userGroup>
</response>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| id | int | идентификатор группы |  |
| name | string | название группы |  |
| userCount | int | количество пользователей в группе |  |

В противном случае будет возвращен ответ с ошибкой:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="error">
  <code></code>
</response>
```

---

## userGroup.getList

Получение полного списка групп пользователей на аккаунте. Не требует админ прав. Формат запроса:

```
<?xml version="1.0" encoding="UTF-8"?>
<request method="userGroup.getList">
  <account></account>
  <sid></sid>
  <sortType></sortType>
  <pageCurrent></pageCurrent>
  <pageSize></pageSize>
  <signature></signature>
</request>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| account | string | имя аккаунта, на котором выполняется функция |  |
| sid | string(32) | ключ сессии |  |
| sortType | enum | тип сортировки | не обязательный параметр. список допустимых значений смотри в разделе типы сортировок для групп |
| pageCurrent | int | постраничная навигация | не обязательный параметр. значения меньше 1 будут инициировать процедуру подсчета количества элементов. |
| pageSize | int | количество выдаваемых значений в результате. не может превышать значение **100** | не обязательный параметр. если будет опущен, возьмется значение по умолчанию. |
| signature | string(32) | подпись |  |

Отвте:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="ok">
  <userGroups count="count" totalCount="totalCount">
    <userGroup>
      <id></id>
      <name></name>
      <userCount></userCount>
    </userGroup>
    <userGroup>
      <id></id>
      <name></name>
      <userCount></userCount>
    </userGroup>
    <!-- ... -->
  </userGroups>
</response>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| **userGroups** |  | корневой элемент, содержащий список групп |  |
| **userGroups** count | int | количество групп возвращенных запросом |  |
| **userGroups** totalCount | int | количество групп удовлетворяющих запросу |  |
| userGroup |  | элемент группы |  |
| userGroup.id | int | идентификатор группы |  |
| userGroup.name | string | название группы |  |
| userGroup.userCount | int | количество пользоавателей в группе |  |

Для пользователей не с админ правами, значение поля **userCount** \- будет всегда рано **0** (ноль).

В противном случае будет возвращен ответ с ошибкой:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="error">
  <code></code>
</response>
```

---

## userGroup.update

\* ПланФикс API:Управление группами пользователей \* Коды ошибок \* Список функций

Функция обновление информации о группе. Доступна только для пользователя с правами администратора. Формат запроса:

```
<?xml version="1.0" encoding="UTF-8"?>
<request method="userGroup.update">
  <account></account>
  <sid></sid>
  <userGroup>
    <id></id>
    <name></name>
  </userGroup>
  <signature></signature>
</request>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| id | int | идентификатор обновляемой группы |  |
| name | string | новое название группы |  |
| signature | string(32) | подпись |  |

Ответ при удачном выполнении функции:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="ok">
  <userGroup>
    <id></id>
  </userGroup>
</response>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| userGroup.id | int | идентификатор обновляемой группы |  |

В противном случае будет возвращен ответ с ошибкой:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="error">
  <code></code>
</response>
```

---

## userGroup.getHeads — Руководители группы

Функция получения списка руководителей группы. Формат запроса:

```
<?xml version="1.0" encoding="UTF-8"?>
<request method="userGroup.getHeads">
  <account></account>
  <sid></sid>
  <userGroup>
    <id></id>
  </userGroup>
  <signature></signature>
</request>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| userGroup.id | int | идентификатор группы |  |
| signature | string(32) | подпись |  |

Ответ:

```
<response status="ok">
  <users count="count" totalCount="totalCount">
    <user>
      <!-- ... -->
    </user>
    <user>
      <!-- ... -->
    </user>
    <!-- ... -->
  </users>
</response>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| **users** |  | список руководителей |  |
| users.user |  | описание пользователя, смотри полное описание структуры в разделе user.get |  |

В противном случае будет возвращен ответ с ошибкой:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="error">
  <code></code>
</response>
```

---

## Роли пользователей

Список допустимых значений для параметра **role** (роль пользователя):

| Значение | Описание | Примечание |
| --- | --- | --- |
| ADMIN | Администратор |  |
| USER | Пользователь |  |
| TECHADMIN | Технический администратор |  |

---

## Статусы пользователей

Статусы пользователей:

| Значение | Описание | Примечание |
| --- | --- | --- |
| ACTIVE | пользователь активен |  |
| UNACTIVE | пользователь неактивен |  |

---

## Пол сотрудника

| Значение | Описание | Примечание |
| --- | --- | --- |
| MALE | муской пол |  |
| FEMALE | женский пол |  |

---

## Типы сортировок пользователей

Список допустимых значений для параметра **sortType** (тип сортировки списка пользователей):

| Значение | Описание | Примечание |
| --- | --- | --- |
| NAME\_ASC | по имени (алфавит) |  |
| NAME\_DESC | по имени (обратный порядок) |  |
| GROUP\_ASC | по имени группы (алфавит) |  |
| GROUP\_DESC | по имени группы (обратный порядок) |  |
| ISACTIVE\_ASC | неактивные, потом активные |  |
| ISACTIVE\_DESC | активные, потом неактивные |  |
| PROJECTS\_ASC | по проекту (алфавит) |  |
| PROJECTS\_DESC | по проекту (обратный порядок) |  |
| ROLE\_ASC | роль (возрастание) |  |
| ROLE\_DESC | роль (убывание) |  |

---

## Фильтры сотрудников

Фильтры сотрудников задаются следующим набором параметров:

- type - числовой идентификатор фильтра
- operator - оператор фильтра, одно из значений из списка (equal, notequal, gt, lt) у разных фильтров могут быть разные допустимые операторы.
- value - значение фильтра, может быть строкой, числом или сложным объектом, в зависимости от типа фильтра

| Тип | Название | Операторы | Формат value |
| --- | --- | --- | --- |
| 9003 | Email | - equal<br>- notequal | string - осуществляется фильтр содержит / не содержит |
| 9006 | Имя / фамилия |

---

## Сложные фильтры сотрудников (REST API)

Сложные фильтры в REST API ПланФикса применяются в методе «/user/list» при получении списка сотрудников. Фильтры сотрудников задаются следующим набором параметров:

- **type** — числовой идентификатор фильтра.
- **operator** — оператор фильтра, одно из значений списка (equal, notequal, gt, lt). У разных фильтров могут быть разные допустимые операторы.
- **value** — значение фильтра, в зависимости от типа фильтра может быть строкой, числом или сложным объектом.
- **field** — идентификатор пользовательского поля справочника, по которому выполняется фильтр.

Пример запроса получения списка сотрудников с передачей нескольких фильтров по пользовательскому полю типа «Дата» и пользовательскому полю типа «Сотрудник» (используется логика И):

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
| 9103 | Поле сотрудника типа Дата | - equal<br>- notequal<br>- gt<br>- lt | Объект :<br>```<br> <br>"value": {<br>    "dateType": string,<br>    "dateValue": string,<br>    "dateFrom": string,<br>    "dateTo": string<br>}<br>```<br>dateType принимает следующие значения:<br>- today — сегодня<br>- yesterday — вчера<br>- tomorrow — завтра<br>- thisWeek — текущая неделя<br>- lastWeek<br>- nextWeek<br>- thisMonth<br>- lastMonth<br>- nextMonth<br>- last — последние n дней, n передается в dateValue<br>- next — следующие n дней, n передается в dateValue<br>- in — через n дней, n передается в dateValue<br>- otherDate — точная дата, дата передается в формате дд-мм-гггг в dateFrom<br>- otherRange — точный период, даты передаются в формате дд-мм-гггг в dateFrom и dateTo<br>- otherDate\_withTime — точная дата-время, дата передается в формате "дд-мм-гггг чч:мм" в dateFrom<br>- otherRange\_withTime — точный период с заданным временем, даты передаются в формате "дд-мм-гггг чч:мм" в dateFrom и dateTo<br>Даты считаются переданными в часовом поясе сотрудника, от имени которого сделан запрос.<br>Примеры:<br>```<br>"value": {<br>    "dateType": "thisWeek"<br>}<br>```<br>```<br>"value": {<br>    "dateType": "otherRange",<br>    "dateFrom": "01-12-2022",<br>    "dateTo": "06-12-2022"<br>}<br>```<br>```<br>"value": {<br>    "dateType": "otherDate_withTime",<br>    "dateFrom": "30-12-2022 12:00",<br>}<br>``` |
| 9120 | Дата рождения сотрудника |
| 9108 | Поле сотрудника типа Контакт | - equal<br>- notequal | string — номер сотрудника/контакта/группы с префиксом.<br>Например: “user:1”, “contact:5”, “group:3” |
| 9109 | Поле сотрудника типа Сотрудник |
| 9110 | Поле сотрудника типа Контрагент |
| 9112 | Поле сотрудника типа Группа, сотрудник, контакт |
| 9113 | Поле сотрудника типа Список сотрудников |
| 9006 | ФИО сотрудника (поиск может выполняется по части ФИО) | - equal<br>- notequal | string — осуществляется фильтр содержит / не содержит |
| 9121 | Имя сотрудника |
| 9122 | Отчество сотрудника |
| 9123 | Фамилия сотрудника |
| 9101 | Поле сотрудника типа Строка | - equal<br>- notequal<br>- have<br>- nothave | string - осуществляется фильтр равно / не равно / содержит / не содержит |
| 9008 | Номер сотрудника | - equal<br>- notequal<br>- gt<br>- lt | int |
| 9102 | Поле сотрудника типа Число |
| 9105 | Поле сотрудника типа Чекбокс | - equal<br>- notequal | int — 1/0 <br> boolean |
| 9106 | Поле сотрудника типа Список | - equal<br>- notequal | string |
| 9107 | Поле сотрудника типа Запись справочника | - equal<br>- notequal | int — идентификатор записи |
| 9111 | Пользовательское поле типа Набор значений | - equal<br>- notequal | string - значение, для условия по нескольким значениям - значения через ; (точку с запятой) |
| 9115 | Поле сотрудника типа Задача | - equal<br>- notequal | int — номер задачи |
| 9117 | Поле сотрудника типа Проект | - equal<br>- notequal | int — номер проекта |
| 9001 | Группа сотрудников | - equal<br>- notequal | int — идентификатор группы |
| 9002 | Номер телефона сотрудника | - equal<br>- notequal | string — осуществляется фильтр содержит / не содержит |
| 9004 | Короткий номер сотрудника |
| 9003 | Внешний email сотрудника |
| 9005 | Должность сотрудника | - equal<br>- notequal | string — название должности |
| 9124 | Язык интерфейса сотрудника | - equal<br>- notequal | string — код языка, например: "En" |
| 9126 | Идентификатор Telegram | - equal<br>- notequal | int - идентификатор в Telegram |

- REST API

---

## Связанные темы

- [[REST API — Контакты (XML)]]
- [[REST API — Авторизация]]
- [[PlanFix API — Index]]
