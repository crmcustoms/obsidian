# Planfix REST API Reference (v1.5.3)

Полная документация REST API Планфикса для использования в сценариях и автоматизациях.

**Base URL:** `https://your-account.planfix.com/rest`  
**Auth:** `Bearer <token>`

## Ключевые разделы
- [Задачи (Task)](#task) — создание, обновление, получение списков.
- [Контакты (Contact)](#contact) — работа с клиентами и компаниями.
- [Справочники (Directory)](#directory) — управление записями в справочниках.
- [Комментарии (Comments)](#comments) — уведомления и обсуждения.
- [Аналитики (Data tags)](#data-tags) — запись и чтение данных аналитик.

---

<a name="task"></a>
## Задачи (Task)

### GET `/task/{id}`
Получить данные задачи. Можно запрашивать системные и кастомные поля.
- **Fields:** `id, name, description, priority, status, project, counterparty, assignees, participants, files, dataTags, customFieldData...`

### POST `/task/list`
Поиск задач по фильтрам.
- **Body:** `{"offset": 0, "pageSize": 100, "filterId": "...", "fields": "..."}`

### POST `/task/`
Создание новой задачи.

---

<a name="contact"></a>
## Контакты (Contact)

### POST `/contact/list`
Получение списка контактов/компаний.
- **Body:** `{"isCompany": true/false, "fields": "id, name, email, phones..."}`

### POST `/contact/{id}`
Обновление данных контакта. Используется для синхронизации с внешними CRM или сайтом.

---

<a name="directory"></a>
## Справочники (Directory)

### POST `/directory/{id}/entry/list`
Получить записи справочника.
### POST `/directory/{id}/entry/`
Добавить новую запись. Полезно для автоматизации прайс-листов или списков услуг.

---

<a name="data-tags"></a>
## Аналитики (Data tags)

### POST `/task/{id}/datatags/`
Добавить строку аналитики в задачу. Это основной способ фиксации метрик (SLA, расходы, доходы).

---

## Важные структуры данных (Schemas)

### UserResponse
- `id`: integer
- `role`: Admin, User, Robot...
- `status`: Active, Inactive...

### TaskResponse
- `status`: объект с `id`, `name`, `color`
- `customFieldData`: массив кастомных полей.

---
_Документация импортирована из REST API v1.5.3. Ку!_
