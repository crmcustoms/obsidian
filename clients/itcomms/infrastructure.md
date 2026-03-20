# ITCOMMS — Інфраструктура

## Сервер Hetzner

| Параметр | Значення |
|----------|----------|
| IP | `65.109.8.78` |
| Контейнер | `itcomms-scripts` |
| Репозиторій | `github.com/crmcustoms/itcomms_migrations` |
| Гілка | `claude/review-migration-S7AZD` |

### Основні команди

```bash
# Запуск скрипту
docker exec itcomms-scripts python <script>.py

# Перегляд логів
docker exec itcomms-scripts tail -f <script>.log

# Деплой змін
git pull origin claude/review-migration-S7AZD
docker restart itcomms-scripts
```

### server.py — Control API
**URL:** `http://65.109.8.78:8002`
**Auth:** `Bearer b082217377346689381200f53d6cab7c`

| Метод | Ендпоінт | Опис |
|-------|----------|------|
| GET | `/health` | Статус сервера |
| POST | `/run/{script}` | Запуск скрипту (`?dry_run=false` для live) |
| GET | `/jobs/{id}` | Статус джобу |
| POST | `/invoice/fill` | Записати дату/TAX в Invoice задачу напряму |

#### POST /invoice/fill — тіло запиту
```json
{
  "pf_task_id": 4015,
  "payment_date": "2025-10-15",
  "tax_rate": 12
}
```

---

## Планфікс

| Параметр | Значення |
|----------|----------|
| Host | `https://itcomms.planfix.com` |
| Token | `6ca06006655c6e695c495a4705609c85` |
| Auth | `Bearer <token>` |
| Base path | `/rest/` |

### Важливі нюанси API
- Поля запитувати за ID в параметрі `fields` (не через "customFieldData" напряму)
- Filter type `325` = фільтр по шаблону задачі
- Filter type `4101` = фільтр контактів по текстовому полю
- `silent=true` в query — без сповіщень та логів (для міграції!)
- Список задач: `POST /rest/task/list`
- При паралельному оновленні задачі → відповідь 202 (в черзі), 200 (відразу)

### Planfix файли через API
```
# 1. Завантажити файл по URL
POST /file/from-url/
Body: {"url": "...", "name": "filename.ext"}
→ Відповідь: {"id": file_id}

# 2. Прикріпити до задачі (файли йдуть тільки в опис задачі!)
POST /task/{task_id}
Body: {"files": [{"id": file_id1}, {"id": file_id2}]}
⚠️  Файли ЗАМІНЮЮТЬСЯ, не додаються! Спочатку GET існуючих.

# Отримати поточні файли задачі
GET /task/{task_id}/files
```

---

## Мегаплан

| Параметр | Значення |
|----------|----------|
| Host | `https://likhtman.megaplan.ru` |
| Token | `NzZkODNiOGUwMWNlMGIyMTY5NzlkMDkzOGEzOWFlOGI1MGYyNTk0YThmOWJkYWE5ZDFlMGMyNGU2YWQ2ZWI1ZA` |
| API | `/api/v3` |
| Auth | `Bearer <token>` (рядок як є, НЕ декодувати!) |

> ⚠️ **Мегаплан доступний тільки з сервера Hetzner** (не з локальної машини без VPN)

### Корисні ендпоінти

```
GET  /api/v3/invoice/{id}          # Рахунок (fields: number, actualPaymentDate, sum, taxTotal, rows)
GET  /api/v3/invoice               # Список (params як JSON-рядок, limit макс ~100)
GET  /api/v3/contractor/{id}       # Контрагент
GET  /api/v3/deal/{id}             # Угода
GET  /api/v3/deal/{id}/allFiles    # ВСІ файли угоди (поля + коментарі)
GET  /api/v3/deal/{id}/attaches    # Тільки файли з полів угоди
GET  /api/v3/deal/{id}/comments    # Коментарі угоди
GET  /api/v3/comment/{id}/attaches # Файли конкретного коментаря
GET  /api/v3/attache/archive?files[]=id  # ZIP кількох файлів
```

### Файлова сутність Мегаплан
```json
{
  "id": 123,
  "name": "filename.pdf",
  "extension": "pdf",
  "size": 12345,
  "path": "/file/download/...",
  "mimeType": "application/pdf",
  "timeCreated": "2025-09-15T10:00:00"
}
```
Завантажити: `GET {MEGAPLAN_HOST}{file.path}` з Bearer token

---

## n8n webhook
- Invoice list: `GET https://n8n.crmcustoms.com/webhook/ebcec118-f1fc-4214-9586-a539fb92a0e4`
- Повертає ~1025 рахунків з полями: `property_invoice_name`, `property_invoiseidmp`, `property_dedlinepay`, `property_sum_fact`, `property_invoicedate`
