# REST API — Файлы (XML API)

← [[PlanFix API — Index]]

Функции XML API для работы с файлами.
Auth: см. [[REST API — Авторизация]]

---

## Список функций

## Перейти

# ПланФикс API: Работа с файлами

Функции для работы с файлами:

1. file.upload / Добавить файл
2. file.download / Скачать файл
3. file.get / Получение информации о файле
4. file.getListForProject / Список файлов в проекте
5. file.getListForTask / Список файлов в задаче
6. file.getListForUser / Список файлов загруженных пользователем
7. file.getListForClient /Список файлов контрагента
8. file.getHistory / Получение истории по файлу
9. file.delete / Удаление файлов

## Перейти

Список функций

---

## file.upload — Загрузить файл

## Перейти

# ПланФикс API file.upload

Функция позволяет прикрепить файл к проекту или задаче. Формат вызова:

```
<?xml version="1.0" encoding="UTF-8"?>
<request method="file.upload">
  <account></account>
  <sid></sid>
  <task>
    <id></id>
  </task>
  <project>
    <id></id>
  </project>
  <owner>
    <id></id>
  </owner>
  <target></target>
  <action>
    <id></id>
  </action>
  <files>
    <file>
      <name></name>
      <sourceType></sourceType>
      <otherFile>
        <id></id>
        <url></url>
      </otherFile>
      <body></body>
      <description></description>
      <newversion></newversion>
    </file>
  </files>
  <notifiedList>
    <users>
      <id></id>
      <id></id>
      <!-- ... -->
    </users>
  </notifiedList>
  <signature></signature>
</request>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| silent | bool | при значении 1 - об изменении не рассылаются уведомления | обязательное значение 1 при массовых периодических обновлениях задач |
| task |  | в рамках какой задачи сохраняется | исключает наличие **project** |
| task.id | int | идентификатор задачи |  |
| project |  | в рамках какого проекта сохраняется | используется, если не задан **task** |
| project.id | int | идентификатор проекта |  |
| owner |  | автор загружаемых файлов | необязательное поле. Если не указано \- берется пользователь от имени которого выполняется функция |
| owner.id | int | идентификатор автора файлов |  |
| target | string | куда прикрепить файл, может принимать одно из следующих значений:<br>- action - к существующему действию (указанному в action.id)<br>- task - к самой задаче (нужны права на ее изменение у автора запроса) | необязательный параметр, по умолчанию файл крепится новым действием к задаче |
| action.id | int | идентификатор действия, к которому крепится файл |  |
| files |  | корневой элемент содержащий список сохраняемых файлов |  |
| file |  | сохраняемый файл |  |
| file.name | string | имя сохраняемого файла | игнорируется, если **sourceType** =PROJECT |
| file.sourceType | enum | тип источника | список допустимых значений смотри в типы источников файла |
| file.otherFile |  | Использовать уже существующий файл | используется при sourceType: INTERNET, PROJECT |
| file.otherFile.id | int | идентификатор файла, используется при ссылке на файл из проекта | sourceType=PROJECT |
| file.otherFile.url | string | URL файла в Интернет | используется только при sourceType=INTERNET |
| file.body | string | тело файла закодированное base64 | используется при sourceType=FILESYSTEM |
| file.description | string | краткое описание содержимого файла | не обязательный параметр, игнорируется, если **sourceType** =PROJECT |
| file.newversion | boolean | если значение равно "1", при совпадении имени файла с ранее загруженным, не выдаётся ошибка, а файл загружается, как новая версия существующего | не обязательный параметр |
| notifiedList |  | список пользователей которых должны уведомить |  |
| notifiedList.users |  | корневой элемент списка пользователей |  |
| notifiedList.users.id | int | идентификатор пользователя |  |

Результат удачного выполнения запроса:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="ok">
  <files>
    <file>
      <uniqueId></uniqueId>
      <name></name>
    </file>
    <file>
      <uniqueId></uniqueId>
      <name></name>
    </file>
    <!-- ... -->
  </files>
</response>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| action.id | int | идентификатор добавленного действия |  |

Ответ является списком файлов из имён файлов и присвоенных им идентификаторов.

В противном случае будет возвращен ответ с ошибкой:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="error">
  <code></code>
  <id></id>
  <name></name>
</response>
```

Параметры id и name присутствуют при code=9006 (имя файла совпадает с именем уже существующего в данном проекте), и в них содержится имя и идентификатор существующего файла.

## Перейти

---

## file.uploadNewVersion — Загрузить новую версию

## Перейти

# ПланФикс API file.uploadNewVersion

Функция позволяет загрузить новую версию файла. Формат запроса:

```
<?xml version="1.0" encoding="UTF-8"?>
<request method="file.uploadNewVersion">
  <account></account>
  <sid></sid>
  <file>
    <id></id>
    <description></description>
    <name></name>
    <body></body>
    <otherFile>
      <url></url>
    </otherFile>
  </file>
  <signature></signature>
</request>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| id | int | идентификатор обновляемого файла |  |
| description | string | новое описание файла |  |
| name | string | имя файла |  |
| body | string | base64 закодированное содержимое файла | используется при загрузке новой версии файла с типом **sourceType** = _FILESYSTEM_ |
| otherFile.url | string | ссылка на url | используется при загрузке новой версии файла с типом **sourceType** = _INTERNET_ |
| signature | string(32) | подпись |  |

Результат успешного выполнения функции:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="ok">
  <file>
    <id></id>
  </file>
</response>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| file.id | int | идентификатор обновляемого файла |  |

В противном случае будет возвращен ответ с ошибкой:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="error">
  <code></code>
</response>
```

## Перейти

---

## file.get — Информация о файле

## Перейти

# ПланФикс API file.get

Функция получения информации о файле. Формат запроса:

```
<?xml version="1.0" encoding="UTF-8"?>
<request method="file.get">
  <account></account>
  <sid></sid>
  <file>
    <id></id>
    <uniqueId></uniqueId>
  </file>
  <returnDownloadLinks></returnDownloadLinks>
  <signature></signature>
</request>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| file.id | int | идентификатор файла |  |
| file.uniqueId | int | уникальный идентификатор файла | игнорируется, если задан параметр **id** |
| returnDownloadLinks | bool | возвращать ли в ответе постоянные ссылки для скачивания файла | значение по-умолчанию 0 |
| signature | string(32) | подпись |  |

Результат выполнения функции:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="ok">
  <file>
    <id></id>
    <uniqueId></uniqueId>
    <name></name>
    <version></version>
    <description></description>
    <date></date>
    <sourceType></sourceType>
    <size></size>
    <task>
      <id></id>
      <title></title>
    </task>
    <project>
      <id></id>
      <title></title>
    </project>
    <user>
      <id></id>
      <name></name>
    </user>
    <downloadLink></downloadLink>
  </file>
</response>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| id | int | идентификатор файла |  |
| uniqueId | int | уникальный идентификатор файла |  |
| name | string | имя файла |  |
| version | int | версия |  |
| description | string | описание |  |
| date | DateTime | дата загрузки файла |  |
| sourceType | enum | типы файлов | список допустимых значений смотри в типы файлов |
| size | int | размер в килобайтах |  |
| task |  | в рамках какой задачи был залит |  |
| task.id | int | идентификатор задачи |  |
| task.title | string | название задачи |  |
| project |  | в рамках какого проекта был загружен файл |  |
| project.id | int | идентификатор проекта |  |
| project.title | string | название проекта |  |
| user |  | пользователь, который загрузил файл |  |
| user.id | int | идентификатор пользователя |  |
| user.name | string | имя пользователя |  |
| downloadLink | string | ссылка для скачивания файла |  |

В противном случае будет возвращен ответ с ошибкой:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="error">
  <code></code>
</response>
```

## Перейти

---

## file.download — Скачать файл

## Перейти

# ПланФикс API file.download

Функция скачивания файла. Формат запроса:

```
<?xml version="1.0" encoding="UTF-8"?>
<request method="file.download">
  <account></account>
  <sid></sid>
  <file>
    <id></id>
    <uniqueId></uniqueId>
  </file>
  <signature></signature>
</request>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| file.id | int | идентификатор файла |  |
| file.uniqueId | int | уникальный идентификатор файла |  |
| signature | string(32) | подпись |  |

При вызове функции можно использовать один из параметров **id** или **uniqueId**. Наличие параметра **id** исключает использование параметра **uniqueId**

Результат выполнения функции расшифровывается по коду HTTP ответа

| Значение | Описание | Примечание |
| --- | --- | --- |
| 200 | тело ответа \- файл |  |
| 302 | прямая ссылка на файл находится в заголовке Location |  |
| 403 | доступ запрещен | тело ответа \- стандартный XML-ответ с расшифровкой ошибки |
| 404 | такого файла не существует | тело ответа \- стандартный XML-ответ с расшифровкой ошибки |

Тело ответа с ошибкой:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="error">
  <code></code>
</response>
```

## Перейти

---

## file.delete — Удалить файл

## Перейти

# ПланФикс API file.delete

Функция удаления списка файлов. Формат запроса:

```
<?xml version="1.0" encoding="UTF-8"?>
<request method="file.delete">
  <account></account>
  <sid></sid>
  <files>
    <id></id>
    <id></id>
    <!-- ... -->
  </files>
  <signature></signature>
</request>
```

или

```
<?xml version="1.0" encoding="UTF-8"?>
<request method="file.delete">
  <account></account>
  <sid></sid>
  <files>
    <uniqueId></uniqueId>
    <uniqueId></uniqueId>
    <!-- ... -->
  </files>
  <signature></signature>
</request>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| files |  | список идентификаторов файлов |  |
| files.id | int | идентификатор файла |  |
| files.uniqueId | int | уникальный идентификатор файла |  |
| signature | string(32) | подпись |  |

Результат выполнения функции:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="ok">
  <count></count>
</response>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| count | int | количество удаленных файлов |  |

В противном случае будет возвращен ответ с ошибкой:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="error">
  <code></code>
</response>
```

## Перейти

---

## file.getListForTask — Файлы задачи

## Перейти

# ПланФикс API file.getListForTask

Функция получения списка файлов для задач. Формат запроса:

```
<?xml version="1.0" encoding="UTF-8"?>
<request method="file.getListForTask">
  <account></account>
  <sid></sid>
  <task>
    <id></id>
    <general></general>
  </task>
  <pageCurrent></pageCurrent>
  <pageSize></pageSize>
  <returnDownloadLinks></returnDownloadLinks>
  <sort></sort>
  <signature></signature>
</request>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| task.id | int | идентификатор задачи |  |
| task.general | int | номер задачи |  |
| pageCurrent | int | текущая страница | нумерация с 1\. 0 - используется для получения количества |
| pageSize | int | размер запрашиваемой страницы |  |
| sort | enum | тип сортировки | список допустимых значений смотри в разделе типы сортировок файлов |
| returnDownloadLinks | bool | возвращать ли в ответе постоянные ссылки для скачивания файла | значение по-умолчанию 0 |
| signature | string(32) | подпись |  |

Результат выполнения функции:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="ok">
  <files count="count" totalCount="totalCount">
    <file>
    <id></id>
    <uniqueId></uniqueId>
      <name></name>
      <version></version>
      <description></description>
      <date></date>
      <sourceType></sourceType>
      <size></size>
      <task>
        <id></id>
        <title></title>
      </task>
      <project>
        <id></id>
        <title></title>
      </project>
      <user>
        <id></id>
        <name></name>
      </user>
      <downloadLink></downloadLink>
    </file>
    <file>
      <!-- ... -->
    </file>
    <!-- ... -->
  </files>
</response>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| **files** |  |  |  |
| **files** count | int | количество возвращенных запросом файлов |  |
| **files** totalCount | int | количество файлов удовлетворяющих запросу |  |
| file |  | узел описывающий файл |  |
| id | int | идентификатор файла | общий для всех версий одного файла |
| uniqueId | int | уникальный идентификатор файла |  |
| name | string | имя файла |  |
| version | int | версия |  |
| description | string | описание |  |
| date | DateTime | дата загрузки файла |  |
| sourceType | enum | типы файлов | список допустимых значений смотри в типы файлов |
| size | int | размер в килобайтах |  |
| task |  | в рамках какой задачи был залит |  |
| task.id | int | идентификатор задачи |  |
| task.title | string | название задачи |  |
| project |  | в рамках какого проекта был загружен файл |  |
| project.id | int | идентификатор проекта |  |
| project.title | string | название проекта |  |
| user |  | пользователь, который загрузил файл |  |
| user.id | int | идентификатор пользователя |  |
| user.name | string | имя пользователя |  |
| downloadLink | string | ссылка для скачивания файла |  |

В противном случае будет возвращен ответ с ошибкой:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="error">
  <code></code>
</response>
```

## Перейти

---

## file.getListForProject — Файлы проекта

## Перейти

# ПланФикс API file.getListForProject

Функция получения списка файлов для проектов. Формат запроса:

```
<?xml version="1.0" encoding="UTF-8"?>
<request method="file.getListForProject">
  <account></account>
  <sid></sid>
  <project>
    <id></id>
  </project>
  <pageCurrent></pageCurrent>
  <pageSize></pageSize>
  <returnDownloadLinks></returnDownloadLinks>
  <signature></signature>
</request>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| project.id | int | идентификатор проекта |  |
| pageCurrent | int | текущая страница | нумерация с 1\. 0 - используется для получения количества |
| pageSize | int | размер запрашиваемой страницы |  |
| returnDownloadLinks | bool | возвращать ли в ответе постоянные ссылки для скачивания файла | значение по-умолчанию 0 |
| signature | string(32) | подпись |  |

Результат выполнения функции:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="ok">
  <files count="count" totalCount="totalCount">
    <file>
      <id></id>
      <name></name>
      <version></version>
      <description></description>
      <date></date>
      <sourceType></sourceType>
      <size></size>
      <task>
        <id></id>
        <title></title>
      </task>
      <project>
        <id></id>
        <title></title>
      </project>
      <user>
        <id></id>
        <name></name>
      </user>
      <downloadLink></downloadLink>
    </file>
    <file>
      <!-- ... -->
    </file>
    <!-- ... -->
  </files>
</response>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| **files** |  |  |  |
| **files** count | int | количество возвращенных запросом файлов |  |
| **files** totalCount | int | количество файлов удовлетворяющих запросу |  |
| file |  | узел описывающий файл |  |
| id | int | идентификатор файла |  |
| name | string | имя файла |  |
| version | int | версия |  |
| description | string | описание |  |
| date | DateTime | дата загрузки файла |  |
| sourceType | enum | типы файлов | список допустимых значений смотри в типы файлов |
| size | int | размер в килобайтах |  |
| task |  | в рамках какой задачи был залит |  |
| task.id | int | идентификатор задачи |  |
| task.title | string | название задачи |  |
| project |  | в рамках какого проекта был загружен файл |  |
| project.id | int | идентификатор проекта |  |
| project.title | string | название проекта |  |
| user |  | пользователь, который загрузил файл |  |
| user.id | int | идентификатор пользователя |  |
| user.name | string | имя пользователя |  |
| downloadLink | string | ссылка для скачивания файла |  |

В противном случае будет возвращен ответ с ошибкой:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="error">
  <code></code>
</response>
```

## Перейти

---

## file.getListForClient — Файлы контакта

## Перейти

# ПланФикс API file.getListForClient

Функция получение списка файлов для контрагента/клиента. Формат запроса:

```
<?xml version="1.0" encoding="UTF-8"?>
<request method="file.getListForClient">
  <account></account>
  <sid></sid>
  <client>
    <id></id>
  </client>
  <pageCurrent></pageCurrent>
  <pageSize></pageSize>
  <signature></signature>
</request>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| client.id | int | идентификатор контрагента/клиента |  |
| pageCurrent | int | текущая страница | нумерация с 1\. 0 - используется для получения количества |
| pageSize | int | размер запрашиваемой страницы |  |
| signature | string(32) | подпись |  |

Результат выполнения функции:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="ok">
  <files count="count" totalCount="totalCount">
    <file>
      <id></id>
      <name></name>
      <version></version>
      <description></description>
      <date></date>
      <sourceType></sourceType>
      <size></size>
      <task>
        <id></id>
        <title></title>
      </task>
      <project>
        <id></id>
        <title></title>
      </project>
      <user>
        <id></id>
        <name></name>
      </user>
    </file>
    <file>
      <!-- ... -->
    </file>
    <!-- ... -->
  </files>
</response>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| **files** |  |  |  |
| **files** count | int | количество возвращенных запросом файлов |  |
| **files** totalCount | int | количество файлов удовлетворяющих запросу |  |
| file |  | узел описывающий файл |  |
| id | int | идентификатор файла |  |
| name | string | имя файла |  |
| version | int | версия |  |
| description | string | описание |  |
| date | DateTime | дата загрузки файла |  |
| sourceType | enum | типы файлов | список допустимых значений смотри в типы файлов |
| size | int | размер в байтах |  |
| task |  | в рамках какой задачи был залит |  |
| task.id | int | идентификатор задачи |  |
| task.title | string | название задачи |  |
| project |  | в рамках какого проекта был загружен файл |  |
| project.id | int | идентификатор проекта |  |
| project.title | string | название проекта |  |
| user |  | пользователь, который загрузил файл |  |
| user.id | int | идентификатор пользователя |  |
| user.name | string | имя пользователя |  |

В противном случае будет возвращен ответ с ошибкой:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="error">
  <code></code>
</response>
```

## Перейти

---

## file.getListForUser — Файлы пользователя

## Перейти

# ПланФикс API file.getListForUser

Функция получение списка файлов, которые загрузил пользователь. Данную функцию можно вызывать с правами администратора для любого пользователя, либо получать список собственных файлов.

```
<?xml version="1.0" encoding="UTF-8"?>
<request method="file.getListForUser">
  <account></account>
  <sid></sid>
  <user>
    <id></id>
  </user>
  <pageCurrent></pageCurrent>
  <pageSize></pageSize>
  <signature></signature>
</request>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| user.id | int | идентификатор пользователя |  |
| pageCurrent | int | текущая страница | нумерация с 1\. 0 - используется для получения количества |
| pageSize | int | размер запрашиваемой страницы |  |
| signature | string(32) | подпись |  |

Результат выполнения функции:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="ok">
  <files count="count" totalCount="totalCount">
    <file>
      <id></id>
      <name></name>
      <version></version>
      <description></description>
      <date></date>
      <sourceType></sourceType>
      <size></size>
      <task>
        <id></id>
        <title></title>
      </task>
      <project>
        <id></id>
        <title></title>
      </project>
      <user>
        <id></id>
        <name></name>
      </user>
    </file>
    <file>
      <!-- ... -->
    </file>
    <!-- ... -->
  </files>
</response>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| **files** |  |  |  |
| **files** count | int | количество возвращенных запросом файлов |  |
| **files** totalCount | int | количество файлов удовлетворяющих запросу |  |
| file |  | узел описывающий файл |  |
| id | int | идентификатор файла |  |
| name | string | имя файла |  |
| version | int | версия |  |
| description | string | описание |  |
| date | DateTime | дата загрузки файла |  |
| sourceType | enum | типы файлов | список допустимых значений смотри в типы файлов |
| size | int | размер в килобайтах |  |
| task |  | в рамках какой задачи был залит |  |
| task.id | int | идентификатор задачи |  |
| task.title | string | название задачи |  |
| project |  | в рамках какого проекта был загружен файл |  |
| project.id | int | идентификатор проекта |  |
| project.title | string | название проекта |  |
| user |  | пользователь, который загрузил файл |  |
| user.id | int | идентификатор пользователя |  |
| user.name | string | имя пользователя |  |

В противном случае будет возвращен ответ с ошибкой:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="error">
  <code></code>
</response>
```

## Перейти

---

## file.getGroupList — Список групп файлов

## Перейти

# ПланФикс API file.getGroupList

Функция позволяет получить список групп файлов. Формат запроса:

```
<?xml version="1.0" encoding="UTF-8"?>
<request method="file.getGroupList">
  <account></account>
  <sid></sid>
  <signature></signature>
</request>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| account | string | имя аккаунта |  |
| sid | string(32) | ключ сессии |  |
| signature | string(32) | подпись |  |

Результат успешного выполнения функции:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="ok">
  <fileGroups count="count" totalCount="totalCount">
    <fileGroup>
      <id></id>
      <name></name>
    </fileGroup>
    <fileGroup>
      <!-- ... -->
    </fileGroup>
    <!-- ... -->
  </fileGroups>
</response>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| **fileGroups** |  | корневой элемент списка групп файлов |  |
| **fileGroups** count |  | количество элементов в списке |  |
| **fileGroups** totalCount |  | также равно количеству элементов в списке |  |
| fileGroup |  | узел описывающий элемент группы файла |  |
| fileGroup.id |  | идентификатор записи |  |
| fileGroup.name |  | название группы |  |

В противном случае будет возвращен ответ с ошибкой:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="error">
  <code></code>
</response>
```

## Перейти

---

## file.getHistory — История версий

## Перейти

# ПланФикс API file.getHistory

Функция получения истории по изменениям файла. Формат запроса:

```
<?xml version="1.0" encoding="UTF-8"?>
<request method="file.getHistory">
  <account></account>
  <sid></sid>
  <file>
    <id></id>
  </file>
  <signature></signature>
</request>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| file.id | int | идентификатор файла |  |
| signature | string(32) | подпись |  |

Результат выполнения функции:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="ok">
  <files count="count" totalCount="totalCount">
    <file>
      <id></id>
      <name></name>
      <version></version>
      <description></description>
      <date></date>
      <sourceType></sourceType>
      <size></size>
      <task>
        <id></id>
        <title></title>
      </task>
      <project>
        <id></id>
        <title></title>
      </project>
      <user>
        <id></id>
        <name></name>
      </user>
    </file>
    <file>
      <!-- ... -->
    </file>
    <!-- ... -->
  </files>
</response>
```

| Название | Тип | Значение | Примечание |
| --- | --- | --- | --- |
| **files** |  |  |  |
| **files** count | int | количество возвращенных запросом файлов |  |
| **files** totalCount | int | количество файлов удовлетворяющих запросу |  |
| file |  | узел описывающий файл |  |
| id | int | идентификатор файла |  |
| name | string | имя файла |  |
| version | int | версия |  |
| description | string | описание |  |
| date | DateTime | дата загрузки файла |  |
| sourceType | enum | типы файлов | список допустимых значений смотри в типы файлов |
| size | int | размер в байтах |  |
| task |  | в рамках какой задачи был залит |  |
| task.id | int | идентификатор задачи |  |
| task.title | string | название задачи |  |
| project |  | в рамках какого проекта был загружен файл |  |
| project.id | int | идентификатор проекта |  |
| project.title | string | название проекта |  |
| user |  | пользователь, который загрузил файл |  |
| user.id | int | идентификатор пользователя |  |
| user.name | string | имя пользователя |  |

В противном случае будет возвращен ответ с ошибкой:

```
<?xml version="1.0" encoding="UTF-8"?>
<response status="error">
  <code></code>
</response>
```

## Перейти

---

## Типы источников файла

## Перейти

# ПланФикс API Типы источников файла

Список допустимых значений для параметра типы источников файла ( **sourceType**)

| Значение | Описание | Примечание |
| --- | --- | --- |
| FILESYSTEM | загружаемый файл |  |
| INTERNET | файл \- ссылка на файл |  |
| PROJECT | файл проекта |  |

## Перейти

---

## Типы файлов

## Перейти

# ПланФикс API: Типы файлов

| Значение | Описание | Примечание |
| --- | --- | --- |
| FILESYSTEM | обычный файл |  |
| INTERNET | внешний файл, хранящийся на внешнем ресурсе |  |

## Перейти

---

## Типы сортировок файлов

## Перейти

# ПланФикс API: Типы сортировок файлов

Список допустимых значений для сортировки файлов:

| Значение | Описание | Примечание |
| --- | --- | --- |
| NUMBER\_ASC | сортировка по номеру (возрастание) |  |
| NUMBER\_DESC | сортировка по номеру (убывание) |  |
| NAME\_ASC | сортировка по имени (возрастание) |  |
| NAME\_DESC | сортировка по имени (убывание) |  |
| DATE\_ASC | сортировка по времени загрузки (возрастание) |  |
| DATE\_DESC | сортировка по времени загрузки (убывание) |  |
| EXT\_ASC | сортировка по типу (возрастание) |  |
| EXT\_DESC | сортировка по типу (убывание) |  |

## Перейти

---

## REST API: Работа с файлами

## Содержание

## Перейти

- REST API

# REST API: Работа с файлами

REST API ПланФикса поддерживает работу с файлами.

## Прикрепить и обновить

Через API можно прикреплять или обновлять ранее загруженные файлы в:

### **Задача**

- Файлы крепятся или обновляются только в описании задачи.
- Прикрепить файлы к описанию во время создания задачи:

- Обновить файлы в описании задачи:

**Важно:**

- Файлы заменяются теми, что переданы в параметре files.
- Если передан пустой массив, файлы будут откреплены от задачи и удалены, если нигде более не используются.
- Если поле files передано не будет, файлы обновляться не будут.

### **Контакт**

- Файлы крепятся или обновляются только в описании контакта.
- Прикрепить файлы к описанию во время создания контакта:

- Обновить файлы в описании контакта:

**Важно:**

- Файлы заменяются теми, что переданы в параметре files.
- Если передан пустой массив, файлы будут откреплены от контакта и удалены, если нигде более не используются.
- Если поле files передано не будет, файлы обновляться не будут.

### **Проект**

- Файлы крепятся или обновляются только в документах проекта.
- Прикрепить файлы во время создания проекта:

- Обновить файлы в проекте:

**Важно:**

- Файлы заменяются теми, что переданы в параметре files.
- Если передан пустой массив, файлы будут откреплены от проекта и удалены, если нигде более не используются.
- Если поле files передано не будет, файлы обновляться не будут.

### **Справочник**

Добавление файла в запись справочника выполняется в два этапа:

1\. Загрузите файл в систему, используя один из методов REST API:

- /file/ — загрузка файла из локального источника.
- /file/from-url/ — загрузка файла по ссылке.

Необходимо передать в параметре **targetType** значение **directory**.

В ответе вы получите идентификатор загруженного файла.

2\. Создаем новую запись справочника и добавляем файл в поле типа файлы

Или обновляем существующую запись справочника

**Важно:**

- При обновлении записи файлы заменяются теми, что переданы в параметре **value**.

## Получить список файлов

Через API можно получить список файлов из:

### Задачи

У метода есть параметр onlyFromDescription, который позволяет получать файлы только из описания задачи:

### Контакта

У метода есть параметр onlyFromDescription, который позволяет получать файлы только из описания контакта:

### Проекта

У метода есть параметры pageSize и offset, для постраничной навигации по списку:

## Перейти

- REST API

---

## Связанные темы

- [[REST API — Авторизация]]
- [[REST API — Задачи (XML)]]
- [[PlanFix API — Index]]
