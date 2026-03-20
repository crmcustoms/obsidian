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
