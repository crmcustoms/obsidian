# ITCOMMS — Мегаплан API

> ⚠️ Мегаплан доступний ТІЛЬКИ з сервера Hetzner (65.109.8.78), не локально без VPN!

## Підключення

```python
MEGAPLAN_HOST = "https://likhtman.megaplan.ru"
MEGAPLAN_TOKEN = "NzZkODNiOGUwMWNlMGIyMTY5NzlkMDkzOGEzOWFlOGI1MGYyNTk0YThmOWJkYWE5ZDFlMGMyNGU2YWQ2ZWI1ZA"

headers = {
    "Authorization": f"Bearer {MEGAPLAN_TOKEN}",
    "Content-Type": "application/json"
}
```

---

## Рахунки (Invoice)

### Отримати один рахунок
```
GET /api/v3/invoice/{id}
```
Ключові поля: `number`, `actualPaymentDate`, `sum.value`, `taxTotal`, `rows`

### Список рахунків
```python
GET /api/v3/invoice
params = {"limit": 100, "fields": json.dumps({"Invoice": ["id","number","actualPaymentDate","sum","taxTotal"]})}
```
⚠️ Пагінація НЕ підтримується (limit макс ~100). Перші 100 покривають потрібний період (серп 2025 – лют 2026).

---

## Угоди (Deal)

### Отримати угоду
```
GET /api/v3/deal/{id}
```

### Всі файли угоди (поля + коментарі)
```
GET /api/v3/deal/{id}/allFiles
Params: modelId={id}, limit=100, pageAfter={cursor}
```

### Тільки файли з полів угоди
```
GET /api/v3/deal/{id}/attaches
```

### Коментарі угоди
```
GET /api/v3/deal/{id}/comments
```

### Файли конкретного коментаря
```
GET /api/v3/comment/{commentId}/attaches
```

---

## Файли (Attache)

### Сутність файлу
```json
{
  "id": 123,
  "name": "document.pdf",
  "extension": "pdf",
  "size": 45678,
  "path": "/file/download/abc123",
  "mimeType": "application/pdf",
  "timeCreated": "2025-09-15T10:00:00"
}
```

### Завантажити файл
```python
url = f"{MEGAPLAN_HOST}{file['path']}"
response = requests.get(url, headers={"Authorization": f"Bearer {MEGAPLAN_TOKEN}"})
```

### ZIP архів кількох файлів
```
GET /api/v3/attache/archive?files[]=id1&files[]=id2
```

### Share URL файлу
```
GET /api/v3/attache/shareUrl/{id}
```

---

## Контрагент

```
GET /api/v3/contractor/{id}
```

---

## Структура угоди Deal (реальний приклад — Program 14 "Текущие клиенты")

```json
{
  "data": {
    "contentType": "Deal",
    "id": "28324",
    "number": "ТК-28324-01082025",
    "name": "Маркетинг ITCOMMS - Q3 2025",
    "contractor": {"contentType": "ContractorCompany", "id": "1039805", "name": "ITCOMMS KZ"},
    "payer": {"contentType": "Payer", "id": "1010702", "name": "ITCOMMS LLC"},
    "manager": {"contentType": "Employee", "id": "1000215"},
    "state": {"id": "127", "name": "в работе", "type": "active"},
    "program": {"contentType": "Program", "id": "14", "name": "Текущие клиенты"},
    "timeCreated": {"contentType": "DateTime", "value": "2025-08-01T12:01:55+00:00"},
    "attaches": [],
    "attachesCount": 0,
    "allFilesCount": 0,
    "commentsCount": 0,
    "attachesCountInComments": 0,

    "Category1000061CustomFieldBrif": [],
    "Category1000061CustomFieldSkanDogovora": [],
    "Category1000061CustomFieldSkanPrilozheniyaIliDs": [],
    "Category1000061CustomFieldAktVipolnennihRabot": [],

    "Category1000061CustomFieldSsilkaNaDisk": "https://drive.google.com/...",
    "Category1000061CustomFieldNachaloRabot": {"contentType": "DateOnly", "year": 2025, "month": 6, "day": 1},
    "Category1000061CustomFieldOzhidaemiyKonetsProekta": {"contentType": "DateOnly", "year": 2025, "month": 8, "day": 30},
    "Category1000061CustomFieldRashodiSummaItogo": {"contentType": "Money", "currency": "RUB", "value": 109096.14},
    "Category1000061CustomFieldRashodiPoProektu": "https://directus.2l-pr.com/?dealId=28324"
  }
}
```

### Маппінг Program → Planfix шаблон

| Megaplan program.id | program.name | Planfix шаблон ID | Назва | Умова |
|---|---|---|---|---|
| 14 | Текущие клиенты | 11 | Текущие клиенти | — |
| 35 | Прочие поставщики | 15 | Прочие поставщики Конфеты | `TipPlatezha == "Конфеты"` |
| 35 | Прочие поставщики | 7691 | Прочие поставщики безнал | `TipPlatezha != "Конфеты"` |

> ⚠️ Program 35 — ОДНА програма в Мегаплані → ДВА шаблони в Планфікс. Розділяємо по `Category1000083CustomFieldTipPlatezha`.

### Файлові поля угоди (Program 14 → шаблон 11)
```python
FILE_FIELDS_PROGRAM14 = [
    'Category1000061CustomFieldBrif',                   # Бриф
    'Category1000061CustomFieldSkanDogovora',            # Скан договора
    'Category1000061CustomFieldSkanPrilozheniyaIliDs',  # Скан приложения/ДС
    'Category1000061CustomFieldAktVipolnennihRabot',    # Акт выполненных работ
]
```

### Файлові поля угоди (Program 35 → шаблони 15 і 7691)
```python
FILE_FIELDS_PROGRAM35 = [
    'Category1000083CustomFieldSkanOriginalaDogovora',  # Скан оригинала договора
    'Category1000083CustomFieldSkanDogovora',           # Скан договора
    'Category1000083CustomFieldAktSkan',                # Акт (скан)
]
```

### Загальне (обидва)
```python
# + deal['attaches']  (прямі вкладення угоди)
# + файли в коментарях (якщо attachesCountInComments > 0):
#   GET /api/v3/deal/{id}/comments → для кожного comment:
#   GET /api/v3/comment/{id}/attaches
```

## Структура рахунку (приклад відповіді)

```json
{
  "data": {
    "Invoice": {
      "id": "1001",
      "number": "МП-2025-0001",
      "actualPaymentDate": "2025-10-15",
      "sum": {"value": 500000},
      "taxTotal": 71428.57,
      "rows": [...]
    }
  }
}
```

### Розрахунок ставки ПДВ
```python
tax_total = invoice.get('taxTotal', 0)
total = invoice.get('sum', {}).get('value', 0)

if tax_total == 0 or total == 0:
    tax_key = 3  # 0%
else:
    rate = round((tax_total / (total - tax_total)) * 100)
    if rate >= 15:
        tax_key = 1  # 16%
    elif rate >= 10:
        tax_key = 2  # 12%
    else:
        tax_key = 3  # 0%
```

---

## Програми Мегаплану → Шаблони Планфікс

| Мегаплан тип | Планфікс шаблон | ID |
|---|---|---|
| Розхідна угода (нал) | Прочие поставщики Конфеты | 15 |
| Розхідна угода (безнал) | Прочие поставщики безнал | 7691 |
| CRM угода | Текущие клиенти | 11 |
| Рахунок | Invoice | 21 |

Зв'язок: поле `132121` (ID сделки) в Планфікс ↔ `deal.id` в Мегаплані
