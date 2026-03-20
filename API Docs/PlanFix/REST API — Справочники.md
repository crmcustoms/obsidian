# REST API — Справочники и аналитики (XML API)

← [[PlanFix API — Index]]

Функции XML API для работы со справочниками и аналитиками.
Auth: см. [[REST API — Авторизация]]

---

## Справочники — список функций

Список функций для работы со справочниками:

1. handbook.getGroupList / Получить список доступных групп справочников в ПланФикс
2. handbook.getList / Получить список доступных справочников в ПланФикс
3. handbook.getStructure / Получить структуру справочника
4. handbook.getRecords / Получить записи справочника
5. handbook.getRecord / Получить одну запись справочника
6. handbook.getRecordMulti / Получить несколько записей справочника по идентификаторам
7. handbook.addRecord / Добавить запись справочника
8. handbook.updateRecord / Изменить запись справочника

Список функций Справочники

---

## handbook.getList — Список справочников

Запрос на получение списка справочников имеет следующий вид:

```
<?xml version="1.0" encoding="UTF-8"?>
<request method="handbook.getList">
    <account></account>
    <sid></sid>
    <group>
      <id></id>
    </group>
    <signature></signature>
</request>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| account | string | аккаунт, на котором выполняется запрос |  |
| sid | string(32) | ключ сесии | выдается в результате прохождения аутентификации |
| group.id | int | идентификатор группы | необязательный параметр |
| signature | string(32) | подпись пакета |  |

Ответ, при успешном выполнении запроса:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="ok">
    <handbooks count="count" totalCount="totalCount">
        <handbook>
            <id></id>
            <name></name>
            <group>
                <id></id>
                <name></name>
            </group>
        </handbook>
        <handbook>
            <id></id>
            <name></name>
            <group>
                <id></id>
                <name></name>
            </group>
        </handbook>
        <!-- ... -->
    </handbooks>
</response>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| **handbooks** | int | список справочников |  |
| **handbooks** count | int | количество справочников в списке |  |
| **handbooks** totalCount | int | количество справочников в списке |  |
| handbook |  | справочник |  |
| handbook.id | int | идентификатор справочника |  |
| handbook.name | string | название справочника |  |
| handbook.group |  | группа |  |
| handbook.group.id | int | идентификатор группы |  |
| handbook.group.name | string | название группы |  |

В противном случае будет возвращен ответ с ошибкой:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="error">
  <code></code>
</response>
```

---

## handbook.getGroupList — Список групп справочников

Запрос на получение списка групп справочников имеет следующий вид:

```
<?xml version="1.0" encoding="UTF-8"?>
<request method="handbook.getGroupList">
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
    <handbookGroups count="count" totalCount="totalCount">
        <group>
            <id></id>
            <name></name>
        </group>
        <group>
            <id></id>
            <name></name>
        </group>
        <!-- ... -->
    </handbookGroups>
</response>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| **handbookGroups** | int | список групп |  |
| **handbookGroups** count | int | количество групп в списке |  |
| **handbookGroups** totalCount | int | количество групп в списке |  |
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

---

## handbook.getStructure — Структура справочника

Получение описания справочника \- полное содержимое полей и их типов значений. Формат запроса:

```
<?xml version="1.0" encoding="UTF-8"?>
<request method="handbook.getStructure">
  <account></account>
  <sid></sid>
  <handbook>
    <id></id>
  </handbook>
  <signature></signature>
</request>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| sid | string(32) | ключ сесии | выдается в результате прохождения аутентификации |
| handbook.id | int | идентификатор справочника |  |

Ответ при успешном выполнении запроса:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="ok">
  <handbook>
    <id></id>
    <name></name>
    <group>
      <id></id>
    </group>
    <fields>
      <field>
        <id></id>
        <num></num>
        <name></name>
        <type></type>
        <list>
          <value></value>
          <value></value>
          <!-- ... -->
        </list>
        <handbook>
          <id></id>
        </handbook>
      </field>
      <field>
        <id></id>
        <num></num>
        <name></name>
        <type></type>
        <list>
          <value></value>
          <value></value>
          <!-- ... -->
        </list>
        <handbook>
          <id></id>
        </handbook>
      </field>
      <!-- ... -->
    </fields>
  </handbook>
</response>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| id | int | идентификатор справочника |  |
| name | string | название справочника |  |
| group.id | int | идентификатор группы справочников |  |
| fields |  | узел, содержащий список полей справочника |  |
| fields.field |  | узел, описывающий поле справочника |  |
| field.id | int | идентификатор поля | требуется в запросах при добавлении записи |
| field.num | int | порядковый номер | для интерфейса |
| field.name | string | описание поля |  |
| field.type | enum | тип данных в поле | перечень допустимых значений для данного поля смотри в разделе типы данных полей аналитики |
| field.list |  | список допустимых значений для поля, если **type** = _LIST_ |  |
| field.list.value | string | значение поля |  |
| field.handbook.id | int | идентификатор справочника, если **type** = _HANDBOOK_ |  |

В противном случае будет возвращен ответ с ошибкой:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="error">
  <code></code>
</response>
```

---

## handbook.getRecord — Получить запись

Получение записи справочника. Формат запроса:

```
<?xml version="1.0" encoding="UTF-8"?>
<request method="handbook.getRecord">
  <account></account>
  <sid></sid>
  <handbook>
    <id></id>
  </handbook>
  <key></key>
  <signature></signature>
</request>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| sid | string(32) | ключ сесии | выдается в результате прохождения аутентификации |
| handbook.id | int | идентификатор справочника |  |
| key | int | идентификатор записи |  |

Ответ при успешном выполнении запроса:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="ok">
    <record>
      <parentKey></parentKey>
      <isGroup></isGroup>
      <key></key>
      <name></name>
      <archived></archived>
      <customData>
        <customValue>
          <field>
            <id></id>
          </field>
          <value></value>
          <text></text>
        </customValue>
        <!-- -->
      </customData>
    </record>
</response>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| parentKey | int | идентификатор группы записей |  |
| isGroup | bool | является ли запись группой |  |
| key | int | идентификатор записи |  |
| name | string | название, если запись является группой |  |
| archived | bool | является ли запись архивной |  |
| customData |  | данные записи |  |
| customData.customValue.field.id |  | идентификатор поля |  |
| customData.customValue.value |  | значение поля |  |
| customData.customValue.text |  | текстовое значение поля |  |

В противном случае будет возвращен ответ с ошибкой:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="error">
  <code></code>
</response>
```

---

## handbook.getRecordMulti — Получить несколько записей

Функция получения множества записей справочника по их идентификаторам. Позволяет получить данные до 100 записей за запрос. Формат запроса:

```
<?xml version="1.0" encoding="UTF-8"?>
<request method="handbook.getRecordMulti">
  <account></account>
  <sid></sid>
  <handbook>
    <id></id>
  </handbook>
  <records>
    <key></key>
    <key></key>
    <!-- -->
  </records>
  <signature></signature>
</request>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| sid | string(32) | ключ сесии | выдается в результате прохождения аутентификации |
| handbook.id | int | идентификатор справочника |  |
| records.key | int | идентификаторы записей |  |

Ответ при успешном выполнении запроса:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="ok">
  <records>
    <record>
      <parentKey></parentKey>
      <isGroup></isGroup>
      <key></key>
      <name></name>
      <archived></archived>
      <customData>
        <customValue>
          <field>
            <id></id>
          </field>
          <value></value>
          <text></text>
        </customValue>
        <!-- -->
      </customData>
    </record>
    <!--    -->
  </records>
</response>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| parentKey | int | идентификатор группы записей |  |
| isGroup | bool | является ли запись группой |  |
| key | int | идентификатор записи |  |
| name | string | название, если запись является группой |  |
| archived | bool | является ли запись архивной |  |
| customData |  | данные записи |  |
| customData.customValue.field.id |  | идентификатор поля |  |
| customData.customValue.value |  | значение поля |  |
| customData.customValue.text |  | текстовое значение поля |  |

В противном случае будет возвращен ответ с ошибкой:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="error">
  <code></code>
</response>
```

---

## handbook.getRecords — Список записей

Получение записей справочника. При наличии групп возвращает записи одного уровня.
Формат запроса:

```
<?xml version="1.0" encoding="UTF-8"?>
<request method="handbook.getRecords">
  <account></account>
  <sid></sid>
  <handbook>
    <id></id>
  </handbook>
  <parentKey></parentKey>
  <pageCurrent></pageCurrent>
  <pageSize></pageSize>
  <signature></signature>
</request>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| sid | string(32) | ключ сесии | выдается в результате прохождения аутентификации |
| handbook.id | int | идентификатор справочника |  |
| parentKey | int | идентификатор группы записей | необязательный параметр |
| pageCurrent | int | номер страницы | от 1 |
| pageSize | int | размер страницы | до 100 |

Ответ при успешном выполнении запроса:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="ok">
  <records>
    <record>
      <parentKey></parentKey>
      <isGroup></isGroup>
      <key></key>
      <name></name>
      <archived></archived>
      <customData>
        <customValue>
          <field>
            <id></id>
          </field>
          <value></value>
          <text></text>
        </customValue>
        <!-- -->
      </customData>
    </record>
    <!--    -->
  </records>
</response>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| parentKey | int | идентификатор группы записей |  |
| isGroup | bool | является ли запись группой |  |
| key | int | идентификатор записи |  |
| name | string | название, если запись является группой |  |
| archived | bool | является ли запись архивной |  |
| customData |  | данные записи |  |
| customData.customValue.field.id |  | идентификатор поля |  |
| customData.customValue.value |  | значение поля |  |
| customData.customValue.text |  | текстовое значение поля |  |

В противном случае будет возвращен ответ с ошибкой:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="error">
  <code></code>
</response>
```

---

## handbook.addRecord — Добавить запись

Создание записи справочника. Формат запроса:

```
<?xml version="1.0" encoding="UTF-8"?>
<request method="handbook.addRecord">
  <account></account>
  <sid></sid>
  <handbook>
    <id></id>
  </handbook>
  <parentKey></parentKey>
  <isGroup></isGroup>
  <name></name>
  <archived></archived>
  <customData>
    <customValue>
      <id></id>
      <value></value>
      <files>
        <file>
          <name></name>
          <sourceType></sourceType>
          <otherFile>
            <url></url>
          </otherFile>
          <body></body>
          <description></description>
          <newversion></newversion>
        </file>
      </files>
    </customValue>
    <!-- ... -->
  </customData>
  <signature></signature>
</request>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| sid | string(32) | ключ сесии | выдается в результате прохождения аутентификации |
| handbook.id | int | идентификатор справочника |  |
| parentKey | int | идентификатор группы записей | необязательный параметр |
| isGroup | bool | является ли запись группой |  |
| name | string | название, если запись является группой |  |
| archived | bool | является ли запись архивной |  |
| customData |  | значения полей |  |
| customData.customValue.id |  | идентификатор поля |  |
| customData.customValue.value |  | значение поля | (для полей типа набор задач, список сотрудников, набор записей справочника \- идентификаторы через запятую в квадратных скобках) |
| files |  | корневой элемент содержащий список сохраняемых файлов |  |
| file |  | сохраняемый файл |  |
| file.name | string | имя сохраняемого файла |  |
| file.sourceType | enum | тип источника | список допустимых значений смотри в типы источников файла, допустимые значения FILESYSTEM и INTERNET |
| file.otherFile |  | Использовать уже существующий файл | используется при sourceType: INTERNET |
| file.otherFile.url | string | URL фала в Интернет | используется только при sourceType=INTERNET |
| file.body | string | тело файла закодированное base64 | используется при sourceType=FILESYSTEM |
| file.description | string | краткое описание содержимого файла | не обязательный параметр |
| file.newversion | boolean | если значение равно "1", при совпадении имени файла с ранее загруженным, не выдаётся ошибка, а файл загружается, как новая версия существующего | не обязательный параметр |

Ответ при успешном выполнении запроса:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="ok">
  <key></key>
</response>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| key | int | идентификатор записи |  |

В противном случае будет возвращен ответ с ошибкой:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="error">
  <code></code>
</response>
```

---

## handbook.updateRecord — Обновить запись

Изменение записи справочника. Формат запроса:

```
<?xml version="1.0" encoding="UTF-8"?>
<request method="handbook.updateRecord">
  <account></account>
  <sid></sid>
  <handbook>
    <id></id>
  </handbook>
  <key></key>
  <parentKey></parentKey>
  <isGroup></isGroup>
  <name></name>
  <archived></archived>
  <customData>
    <customValue>
      <id></id>
      <value></value>
      <files>
        <file>
          <name></name>
          <sourceType></sourceType>
          <otherFile>
            <url></url>
          </otherFile>
          <body></body>
          <description></description>
          <newversion></newversion>
        </file>
      </files>
    </customValue>
    <!-- ... -->
  </customData>
  <signature></signature>
</request>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| sid | string(32) | ключ сесии | выдается в результате прохождения аутентификации |
| handbook.id | int | идентификатор справочника |  |
| key | int | идентификатор записи | в списке записей справочника находится в [столбце ID](https://p.pfx.so/pf/YM/FaBjKf.jpg) |
| parentKey | int | идентификатор группы записей | необязательный параметр |
| isGroup | bool | является ли запись группой |  |
| name | string | название, если запись является группой |  |
| archived | bool | является ли запись архивной |  |
| customData |  | значения полей |  |
| customData.customValue.id |  | идентификатор поля |  |
| customData.customValue.value |  | значение поля | (для полей типа набор задач, список сотрудников, набор записей справочника \- идентификаторы через запятую в квадратных скобках) |
| files |  | корневой элемент содержащий список сохраняемых файлов | Для поля типа Файлы |
| file |  | сохраняемый файл |  |
| file.name | string | имя сохраняемого файла |  |
| file.sourceType | enum | тип источника | список допустимых значений смотри в типы источников файла, допустимые значения FILESYSTEM и INTERNET |
| file.otherFile |  | Использовать уже существующий файл | используется при sourceType: INTERNET |
| file.otherFile.url | string | URL фала в Интернет | используется только при sourceType=INTERNET |
| file.body | string | тело файла закодированное base64 | используется при sourceType=FILESYSTEM |
| file.description | string | краткое описание содержимого файла | не обязательный параметр |
| file.newversion | boolean | если значение равно "1", при совпадении имени файла с ранее загруженным, не выдаётся ошибка, а файл загружается, как новая версия существующего | не обязательный параметр |

Ответ при успешном выполнении запроса:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="ok">
  <key></key>
</response>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| key | int | идентификатор записи |  |

В противном случае будет возвращен ответ с ошибкой:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="error">
  <code></code>
</response>
```

## Примечание

Работая с полем типа файл
customValue.value - содержит список идентификаторов файлов прикрепленных ранее через запятую.
Все прикрепленные ранее файлы, идентификаторы которых будут отсутствовать \- будут удалены.

---

## Аналитики — список функций

Список функций для работы с аналитикой:

1. analitic.getGroupList / Получить список доступных групп аналитик в ПланФикс
2. analitic.getList / Получить список доступных аналитик в ПланФикс
3. analitic.getOptions / Получить описание аналитики
4. analitic.getHandbook / Получить справочник аналитики
5. analitic.getData / Получить данные заполненной аналитики
6. analitic.getDataByCondition / Получить данные аналитики по условию

Список функций

---

## analitic.getList — Список аналитик

Запрос на получение списка аналитик имеет следующий вид:

```
<?xml version="1.0" encoding="UTF-8"?>
<request method="analitic.getList">
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
    <analitics  count="count" totalCount="totalCount">
        <analitic>
            <id></id>
            <name></name>
            <group>
                <id></id>
                <name></name>
            </group>
        </analitic>
        <analitic>
            <id></id>
            <name></name>
            <group>
                <id></id>
                <name></name>
            </group>
        </analitic>
        <!-- ... -->
    </analitics>
</response>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| **analitics** | int | список аналитик |  |
| **analitics** count | int | количество аналитик в списке |  |
| **analitics** totalCount | int | количество аналитик в списке |  |
| analitic |  | аналитика |  |
| analitic.id | int | идентификатор аналитики |  |
| analitic.name | string | название аналитики |  |
| analitic.group |  | группа |  |
| analitic.group.id | int | идентификатор группы |  |
| analitic.group.name | string | название группы |  |

В противном случае будет возвращен ответ с ошибкой:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="error">
  <code></code>
</response>
```

---

## analitic.getGroupList — Список групп аналитик

Запрос на получение списка групп аналитик имеет следующий вид:

```
<?xml version="1.0" encoding="UTF-8"?>
<request method="analitic.getGroupList">
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
    <analiticGroups count="count" totalCount="totalCount">
        <group>
            <id></id>
            <name></name>
        </group>
        <group>
            <id></id>
            <name></name>
        </group>
        <!-- ... -->
    </analiticGroups>
</response>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| **analiticGroups** | int | список групп |  |
| **analiticGroups** count | int | количество групп в списке |  |
| **analiticGroups** totalCount | int | количество групп в списке |  |
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

---

## analitic.getData — Данные аналитики

Получение данных аналитики прикрепленной действием. Список доступных аналитик получают из списка действий. Формат запроса:

```
<?xml version="1.0" encoding="UTF-8"?>
<request method="analitic.getData">
  <account></account>
  <sid></sid>
  <analiticKeys>
    <key></key>
    <key></key>
    <!-- ... -->
  </analiticKeys>
  <signature></signature>
</request>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| sid | string(32) | ключ сесии | выдается в результате прохождения аутентификации |
| analiticKeys.key | int | идентификатор строки данных аналитики | возвращается функцией action.get |

Допускается одним запросом получать данные сразу нескольких добавленных аналитик. Если в запросе был передан идентификатор несуществующей аналитики, то будет возвращена пустая аналитика, ошибка при этом не будет генерироваться.

Ответ при успешном выполнении запроса:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="ok">
  <analiticDatas>
    <analiticData>
      <key></key>
      <itemData>
        <id></id>
        <name></name>
        <value></value>
        <valueId></valueId>
      </itemData>
      <itemData>
        <id></id>
        <name></name>
        <value></value>
        <valueId></valueId>
      </itemData>
      <!-- ... -->
    </analiticData>
    <analiticData>
      <key></key>
      <itemData>
        <id></id>
        <name></name>
        <value></value>
        <valueId></valueId>
      </itemData>
      <itemData>
        <id></id>
        <name></name>
        <value></value>
        <valueId></valueId>
      </itemData>
      <!-- ... -->
    </analiticData>
    <!-- ... -->
  </analiticDatas>
</response>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| analiticDatas |  | список запрошенных аналитик |  |
| analiticData |  | узел описывающий данные, которые содержит аналитика |  |
| analiticData.key | int | идентификатор данных |  |
| analiticData.itemData |  | узел описывающие запись с данными в аналитике |  |
| itemData.id | int | идентификатор, он равен field.id |  |
| itemData.name | string | имя поля |  |
| itemData.value | string | значение (строковое представления) |  |
| itemData.valueId | string | значение (идентификатор для полей, содержащих объект) |  |

В противном случае будет возвращен ответ с ошибкой:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="error">
  <code></code>
</response>
```

---

## analitic.getDataByCondition — Данные с условием

Получение данных аналитик по заданному условию.

```
<?xml version="1.0" encoding="UTF-8"?>
<request method="analitic.getDataByCondition">
  <account></account>
  <sid></sid>
  <analitic>
    <id></id>
  </analitic>
  <task>
    <id></id>
    <general></general>
  </task>
  <filters>
    <filter>
      <field></field>
      <fromDate></fromDate>
      <toDate></toDate>
      <userid></userid>
    </filter>
  </filters>
  <pageSize></pageSize>
  <pageCurrent></pageCurrent>
  <signature></signature>
</request>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| sid | string(32) | ключ сесии | выдается в результате прохождения аутентификации |
| analitic.id | int | идентификатор аналитики |  |
| task.id | int | идентификатор задачи | необязательный, если отсутствует \- выбор по всем задачам |
| task.general | int | номер задачи |
| pageCurrent | int | страница |  |
| pageSize | int | размер страницы (максимум 100) |  |
| filters |  | условия |  |
| filter.field | int | идентификатор поля аналитики по которому делается условие |  |
| filter.fromDate | DateTime | для условия по дате \- от даты |  |
| filter.toDate | DateTime | для условия по дате \- до даты |  |
| filter.userid | int | для условия по полю типа Список пользователей / Сотрудник / Группа, сотрудник или контакт \- идентификатор сотрудника, как он возвращается функцией user.GetList |  |

Ответ при успешном выполнении запроса:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="ok">
  <analiticDatas>
    <analiticData>
      <key></key>
      <task>
        <id></id>
      </task>
      <action>
        <id></id>
      </action>
      <itemData>
        <id></id>
        <name></name>
        <value></value>
        <valueId></valueId>
      </itemData>
      <!-- ... -->
    </analiticData>
    <analiticData>
      <key></key>
      <itemData>
        <id></id>
        <name></name>
        <value></value>
        <valueId></valueId>
      </itemData>
      <!-- ... -->
    </analiticData>
    <!-- ... -->
  </analiticDatas>
</response>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| analiticDatas |  | список запрошенных аналитик |  |
| analiticData |  | узел описывающий данные, которые содержит аналитика |  |
| analiticData.key | int | идентификатор данных |  |
| analiticData.task.id | int | идентификатор задачи, к которой прикреплена аналитика |  |
| analiticData.action.id | int | идентификатор действия, к которому прикреплена аналитика |  |
| analiticData.itemData |  | узел описывающие запись с данными в аналитике |  |
| itemData.id | int | идентификатор, он равен field.id |  |
| itemData.name | string | имя поля |  |
| itemData.value | string | текстовое значение |  |
| itemData.valueId | string | значение идентификатор, для полей-объектов |  |

В противном случае будет возвращен ответ с ошибкой:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="error">
  <code></code>
</response>
```

---

## analitic.getHandbook — Связанный справочник

Получение справочника для аналитики. Формат запроса:

```
<?xml version="1.0" encoding="UTF-8"?>
<request method="analitic.getHandbook">
  <account></account>
  <sid></sid>
  <handbook>
    <id></id>
  </handbook>
  <signature></signature>
</request>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| sid | string(32) | ключ сесии | выдается в результате прохождения аутентификации |
| handbook.id | int | идентификатор справочника | получается в результате выполнения функции получить описание аналитики |

Ответ при успешном выполнении запроса:

```
<response status="ok">
  <records>
    <record>
      <key></key>
      <parentKey></parentKey>
      <isGroup></isGroup>
      <value value="" name="" isDisplayed="" />
      <value value="" name="" isDisplayed="" />
      <!-- ... -->
    </record>
    <record>
      <key></key>
      <parentKey></parentKey>
      <isGroup></isGroup>
      <value value="" name="" isDisplayed="" />
      <value value="" name="" isDisplayed="" />
      <!-- ... -->
    </record>
    <!-- ... -->
  </records>
</response>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| records |  | список записей в справочнике |  |
| record |  | узел описывающий запись в справочнике |  |
| record.key | int | идентификатор записи |  |
| record.parentKey | int | идентификатор родительской записи |  |
| record.isGroup | bool | является ли узел родителем/названием группы | записи данного типа не могут использоваться для аналитике, используются для группировки данных |
| record.value |  | узел описывающий данные в записи |  |
| value **value** | string | значение поля |  |
| value **name** | string | имя |  |
| value **isDisplayed** | bool | отображаемое поле или нет |  |

В противном случае будет возвращен ответ с ошибкой:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="error">
  <code></code>
</response>
```

---

## analitic.getOptions — Параметры аналитики

Получение описания аналитики \- полное содержимое полей и их типов значений. Формат запроса:

```
<?xml version="1.0" encoding="UTF-8"?>
<request method="analitic.getOptions">
  <account></account>
  <sid></sid>
  <analitic>
    <id></id>
  </analitic>
  <signature></signature>
</request>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| sid | string(32) | ключ сесии | выдается в результате прохождения аутентификации |
| analitic.id | int | идентификатор аналитики |  |

Ответ при успешном выполнении запроса:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="ok">
  <analitic>
    <id></id>
    <name></name>
    <group>
      <id></id>
    </group>
    <fields>
      <field>
        <id></id>
        <num></num>
        <name></name>
        <type></type>
        <list>
          <value></value>
          <value></value>
          <!-- ... -->
        </list>
        <handbook>
          <id></id>
        </handbook>
      </field>
      <field>
        <id></id>
        <num></num>
        <name></name>
        <type></type>
        <list>
          <value></value>
          <value></value>
          <!-- ... -->
        </list>
        <handbook>
          <id></id>
        </handbook>
      </field>
      <!-- ... -->
    </fields>
  </analitic>
</response>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| id | int | идентификатор аналитики |  |
| name | string | название аналитики |  |
| group.id | int | идентификатор группы аналитики |  |
| fields |  | узел, содержащий список полей аналитики |  |
| fields.field |  | узел, описывающий поле аналитики |  |
| field.id | int | идентификатор поля | требуется в запросах при добавлении аналитики к действию |
| field.num | int | порядковый номер | для интерфейса |
| field.name | string | описание поля |  |
| field.type | enum | тип данных в поле | перечень допустимых значений для данного поля смотри в разделе типы данных полей аналитики |
| field.list |  | список допустимых значений для поля, если **type** = _LIST_ |  |
| field.list.value | string | значение поля |  |
| field.handbook.id | int | идентификатор справочника, если **type** = _HANDBOOK_ |  |

В противном случае будет возвращен ответ с ошибкой:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="error">
  <code></code>
</response>
```

---

## Сложные фильтры справочников (REST API)

Сложные фильтры в REST API ПланФикса применяются в методе «/directory/{id}/entry/list» при получении записей справочников. Фильтры справочников задаются следующим набором параметров:

- **type** — числовой идентификатор фильтра.
- **operator** — оператор фильтра, одно из значений списка (equal, notequal, gt, lt). У разных фильтров могут быть разные допустимые операторы.
- **value** — значение фильтра, в зависимости от типа фильтра может быть строкой, числом или сложным объектом.
- **field** — идентификатор пользовательского поля справочника, по которому выполняется фильтр.

Пример запроса получения списка записей справочника с передачей нескольких фильтров, по пользовательскому полю типа «Дата» и пользовательскому полю типа «Сотрудник» (используется логика И):

```
{
  "offset": 0,
  "pageSize": 100,
  "fields": "directory,parentKey,key,3,5",
  "filters": [\
    {\
      "type": 6103,\
      "field": 3,\
      "operator": "equal",\
      "value": {\
        "dateType": "otherRange",\
        "dateFrom": "15-12-2022",\
        "dateTo": "17-12-2022"\
      }\
    },\
    {\
      "type": 6109,\
      "field": 5,\
      "operator": "equal",\
      "value": "user:50"\
    }\
  ]
}
```

| Тип | Название | Операторы | Формат value |
| --- | --- | --- | --- |
| 6103 | Поле справочника типа Дата | - equal<br>- notequal<br>- gt<br>- lt | Объект :<br>```<br> <br>"value": {<br>    "dateType": string,<br>    "dateValue": string,<br>    "dateFrom": string,<br>    "dateTo": string<br>}<br>```<br>dateType принимает следующие значения:<br>- today - сегодня<br>- yesterday - вчера<br>- tomorrow - завтра<br>- thisWeek - текущая неделя<br>- lastWeek<br>- nextWeek<br>- thisMonth<br>- lastMonth<br>- nextMonth<br>- last - последние n дней, n передается в dateValue<br>- next - следующие n дней, n передается в dateValue<br>- in - через n дней, n передается в dateValue<br>- otherDate - точная дата, дата передается в формате дд-мм-гггг в dateFrom<br>- otherRange - точный период, даты передаются в формате дд-мм-гггг в dateFrom и dateTo<br>- otherDate\_withTime - точная дата-время, дата передается в формате "дд-мм-гггг чч:мм" в dateFrom<br>- otherRange\_withTime - точный период с заданным временем, даты передаются в формате "дд-мм-гггг чч:мм" в dateFrom и dateTo<br>Даты считаются переданными в часовом поясе сотрудника, от имени которого сделан запрос.<br>Примеры:<br>```<br>"value": {<br>    "dateType": "thisWeek"<br>}<br>```<br>```<br>"value": {<br>    "dateType": "otherRange",<br>    "dateFrom": "01-12-2022",<br>    "dateTo": "06-12-2022"<br>}<br>```<br>```<br>"value": {<br>    "dateType": "otherDate_withTime",<br>    "dateFrom": "30-12-2022 12:00",<br>}<br>``` |
| 6108 | Поле справочника типа Контакт | - equal<br>- notequal | string - номер сотрудника/контакта/группы с префиксом.<br>Например: “user:1”, “contact:5”, “group:3” |
| 6109 | Поле справочника типа Сотрудник |
| 6110 | Поле справочника типа Контрагент |
| 6112 | Поле справочника типа Группа, сотрудник, контакт |
| 6113 | Поле справочника типа Список сотрудников | - equal<br>- notequal | string - осуществляется фильтр содержит / не содержит |
| 6101 | Поле справочника типа Строка | - equal<br>- notequal<br>- have<br>- nothave | string - осуществляется фильтр равно / не равно / содержит / не содержит |
| 6102 | Поле справочника типа Число | - equal<br>- notequal<br>- gt<br>- lt | int |
| 6105 | Поле справочника типа Чекбокс | - equal<br>- notequal | int - 1/0 <br> boolean |
| 6005 | Является ли запись архивной |
| 6106 | Поле справочника типа Список | - equal<br>- notequal | string |
| 6107 | Поле справочника типа Запись справочника | - equal<br>- notequal | int - идентификатор записи |
| 6114 | Поле справочника типа Набор записей справочника | - equal<br>- notequal | int — идентификатор записи, для условия по нескольким записям — идентификаторы через ; (точку с запятой) |
| 6115 | Поле справочника типа Задача | - equal<br>- notequal | int — номер задачи |
| 6117 | Поле справочника типа Проект | - equal<br>- notequal | int — номер проекта |
| 6001 | Содержит значение в пользовательском поле | - equal | int - идентификатор поля |
| 6002 | Не содержит значение в пользовательском поле | - equal | int - идентификатор поля |
| 6003 | Запись входит в группу справочника | - equal<br>- notequal | int - идентификатор группы справочника |
| 6006 | Запись справочника | - equal<br>- notequal | int - идентификатор записи или массив идентификаторов для условия по нескольким идентификаторам (ИЛИ) |

- REST API

---

## Сложные фильтры аналитик (REST API)

Сложные фильтры в REST API ПланФикса применяются в методе «/datatag/{id}/entry/list» при получении записей аналитики. Фильтры аналитик задаются следующим набором параметров:

- **type** — числовой идентификатор фильтра.
- **operator** — оператор фильтра, одно из значений списка (equal, notequal, gt, lt). У разных фильтров могут быть разные допустимые операторы.
- **value** — значение фильтра, в зависимости от типа фильтра может быть строкой, числом или сложным объектом.
- **field** — идентификатор пользовательского поля аналитики, по которому выполняется фильтр.

Пример запроса получения списка записей аналитик с передачей нескольких фильтров по пользовательским полям типа «Дата» и типа «Список пользователей» (используется логика И):

```
{
    "offset": 0,
    "pageSize": 100,
    "fields": "dataTag,key,3,2273",
    "filters": [\
        {\
            "type": 3101,\
            "field": 3,\
            "operator": "equal",\
            "value": {\
                "dateType": "otherRange",\
                "dateFrom": "15-11-2022",\
                "dateTo": "17-11-2022"\
            }\
        },\
        {\
            "type": 3103,\
            "field": 2273,\
            "operator": "equal",\
            "value": "user:46"\
        }\
    ]
}
```

| Тип | Название | Операторы | Формат value |
| --- | --- | --- | --- |
| 3101 | Пользовательское поле типа «Дата» | - equal<br>- notequal<br>- gt<br>- lt | Объект :<br>```<br> <br>"value": {<br>    "dateType": string,<br>    "dateValue": string,<br>    "dateFrom": string,<br>    "dateTo": string<br>}<br>```<br>dateType принимает следующие значения:<br>- today - сегодня<br>- yesterday - вчера<br>- tomorrow - завтра<br>- thisWeek - текущая неделя<br>- lastWeek<br>- nextWeek<br>- thisMonth<br>- lastMonth<br>- nextMonth<br>- last - последние n дней, n передается в dateValue<br>- next - следующие n дней, n передается в dateValue<br>- in - через n дней, n передается в dateValue<br>- otherDate - точная дата, дата передается в формате дд-мм-гггг в dateFrom<br>- otherRange - точный период, даты передаются в формате дд-мм-гггг в dateFrom и dateTo<br>- otherDate\_withTime - точная дата-время, дата передается в формате "дд-мм-гггг чч:мм" в dateFrom<br>- otherRange\_withTime - точный период с заданным временем, даты передаются в формате "дд-мм-гггг чч:мм" в dateFrom и dateTo<br>Даты считаются переданными в часовом поясе сотрудника, от имени которого сделан запрос.<br>Примеры:<br>```<br>"value": {<br>    "dateType": "thisWeek"<br>}<br>```<br>```<br>"value": {<br>    "dateType": "otherRange",<br>    "dateFrom": "02-07-2022",<br>    "dateTo": "06-07-2022"<br>}<br>```<br>```<br>"value": {<br>    "dateType": "otherDate_withTime",<br>    "dateFrom": "30-06-2022 12:00",<br>}<br>``` |
| 3106 | Пользовательское поле типа «Контрагент» | - equal<br>- notequal | string - номер сотрудника/контакта/группы с префиксом. <br>Например: “user:1”, “contact:5”, “group:3” |
| 3116 | Пользовательское поле типа «Контакт» |
| 3104 | Пользовательское поле типа «Сотрудник» |
| 3111 | Пользовательское поле типа «Группа, сотрудник, контакт» |
| 3103 | Пользовательское поле типа «Список сотрудников» |
| 3108 | Пользовательское поле типа «Строка» | - equal<br>- notequal<br>- have<br>- nothave | string - осуществляется фильтр равно / не равно / содержит / не содержит |
| 3109 | Пользовательское поле типа «Число» | - equal<br>- notequal<br>- gt<br>- lt | int |
| 3115 | Пользовательское поле типа «Чек-бокс» | - equal<br>- notequal | int - 1 / 0<br>boolean |
| 3105 | Пользовательское поле типа «Справочник» | - equal<br>- notequal | int - идентификатор записи |
| 3117 | Пользовательское поле типа «Задача» | - equal<br>- notequal | int - номер задачи |
| 3123 | Пользовательское поле типа «Набор задач» | - equal<br>- notequal | int - номер задачи или массив номеров |

- REST API

---

## Типы данных полей аналитики

Список допустимых типов данных полей аналитики:

| Значение | Описание | Примечание |
| --- | --- | --- |
| **STRING** | строка | не более 255 символов |
| **NUMBER** | число |  |
| **TEXT** | текст |  |
| **DATE** | дата |  |
| **DATETIME** | дата \+ время |  |
| **TIME** | время |  |
| **TIMEPERIOD** | период времени |  |
| **CHECKBOX** | чекбокс |  |
| **LIST** | список |  |
| **TASK** | задача |  |
| **TASKSET** | список задач |  |
| **PROJECT** | проект |  |
| **HANDBOOK** | справочник |  |
| **HANDBOOKSET** | набор значений справочника |  |
| **USER** | сотрудник |  |
| **CLIENT** | контрагент |  |
| **CONTACT** | контакт |  |
| **LOGINLIST** | список сотрудников или контактов с правом доступа в ПланФикс |  |
| **GROUPUSERCONTACT** | сотрудник, контанкт или группа сотрудников в ПланФикс |  |
| **SETSTRING** | набор значений |  |

---

## Типы пользовательских полей (REST API)

В REST API можно получить список типов пользовательских полей c помощью метода **/customfield/type**.

| ID | Name | Название |
| --- | --- | --- |
| 0 | Short text | Строка |
| 1 | Number | Число |
| 2 | Multi-line text | Текст |
| 3 | Date | Дата |
| 4 | Time | Время |
| 5 | Date and time | Дата и время |
| 6 | Period of time | Период времени |
| 7 | Checkbox | Чекбокс |
| 8 | List | Список |
| 9 | Directory entry | Запись справочника |
| 10 | Contact | Контакт |
| 11 | Employee | Сотрудник |
| 12 | Counterparty | Контрагент |
| 13 | Group, employee, or contact | Группа, сотрудник или контакт |
| 14 | List of users | Список пользователей |
| 15 | Set of directory values | Набор значений справочника |
| 16 | Task | Задача |
| 17 | Task set | Набор задач |
| 20 | Set of values | Набор значений |
| 21 | Files | Файлы |
| 22 | Project | Проект |
| 23 | Data tag summaries | Итоги аналитик |
| 24 | Calculated field | Вычисляемое поле |
| 25 | Location | Местоположение |
| 26 | Subtask total | Сумма подзадач |
| 27 | AI results field | Результат обучения |
| 28 | Date with time frame | Дата с периодом времени |
| 29 | Totals field | Суммарное поле |

## Пример

Запрос

```
curl -X 'GET' \
  'https://test.planfix.ru/rest/customfield/type' \
  -H 'accept: application/json' \
  -H 'Authorization: Bearer o4l3kwh6lkj43h6'
```

Ответ

```
{
  "result": "success",
  "customFieldTypes": [\
    {\
      "id": 0,\
      "name": "Line"\
    },\
    {\
      "id": 1,\
      "name": "Number"\
    },\
    {\
      "id": 2,\
      "name": "Text"\
    },\
    {\
      "id": 3,\
      "name": "Date"\
    },\
    {\
      "id": 4,\
      "name": "Time"\
    },\
    {\
      "id": 5,\
      "name": "Date and time"\
    },\
    {\
      "id": 6,\
      "name": "Period of time"\
    },\
    {\
      "id": 7,\
      "name": "Checkbox"\
    },\
    {\
      "id": 8,\
      "name": "List"\
    },\
    {\
      "id": 9,\
      "name": "Directory entry"\
    },\
    {\
      "id": 10,\
      "name": "Contact"\
    },\
    {\
      "id": 11,\
      "name": "Employee"\
    },\
    {\
      "id": 12,\
      "name": "Counterparty"\
    },\
    {\
      "id": 13,\
      "name": "Group, employee, or contact"\
    },\
    {\
      "id": 14,\
      "name": "List of users"\
    },\
    {\
      "id": 15,\
      "name": "Set of directory values"\
    },\
    {\
      "id": 16,\
      "name": "Task"\
    },\
    {\
      "id": 17,\
      "name": "Task set"\
    },\
    {\
      "id": 20,\
      "name": "Set of values"\
    },\
    {\
      "id": 21,\
      "name": "Files"\
    },\
    {\
      "id": 22,\
      "name": "Project"\
    },\
    {\
      "id": 23,\
      "name": "Data tag summaries"\
    },\
    {\
      "id": 24,\
      "name": "Calculated field"\
    },\
    {\
      "id": 25,\
      "name": "Location"\
    },\
    {\
      "id": 26,\
      "name": "Subtask total"\
    },\
    {\
      "id": 27,\
      "name": "AI results field"\
    },\
    {\
      "id": 28,\
      "name": "Date with time frame"\
    },\
    {\
      "id": 29,\
      "name": "Totals field"\
    }\
  ]
}
```

- REST API

---

## Связанные темы

- [[REST API — Авторизация]]
- [[REST API — Задачи (XML)]]
- [[PlanFix API — Index]]
