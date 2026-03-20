# ITCOMMS — Скрипти міграції

Всі скрипти в репозиторії `github.com/crmcustoms/itcomms_migrations`
Гілка: `claude/review-migration-S7AZD`
Запуск: `docker exec itcomms-scripts python <script>.py`

---

## Таблиця скриптів

| Файл | Призначення | Статус |
|------|-------------|--------|
| fill_supplier.py | Заповнює Постачальника в шаблонах 15 і 7691 | ✅ |
| fill_supplier_from_megaplan.py | Те ж, через Megaplan API для not_found кейсів | ✅ |
| fill_supplier_standalone.py | Standalone версія fill_supplier | ✅ |
| fill_payment_date.py | Дата оплати (поле 138909) з Мегаплану в 15 і 7691 | ✅ |
| fill_invoice_payment.py | Дата оплати (128157) + TAX в Invoice (21) | ✅ |
| export_megaplan_invoices.py | Вигрузка рахунків Мегаплан за період + матч Планфікс | ✅ |
| server.py | Control API на порту 8002 | ✅ Live |

---

## Детальний опис

### fill_invoice_payment.py
Джерело даних: n8n webhook → список рахунків → фільтр по `property_invoicedate` (серп 2025 – лют 2026)
Для кожного рахунку:
1. Пошук задачі в Планфікс по `Invoice Number` (поле 128167)
2. Отримання `actualPaymentDate` з Мегаплану через `GET /api/v3/invoice/{id}`
3. Запис в поле 128157 (Invoice Payment Date)
4. Визначення ставки ПДВ (16%/12%/0%) з `taxTotal/sum`
5. Перевірка чи TAX вже є (preload з `/datatag/9611/entry/list`)
6. Запис TAX datatag через `POST /task/{id}/datatags/`

### server.py
FastAPI сервер, завжди активний в контейнері.
```bash
# Перевірити статус
curl http://65.109.8.78:8002/health

# Запустити скрипт (dry run)
curl -X POST http://65.109.8.78:8002/run/fill_invoice_payment \
  -H "Authorization: Bearer b082217377346689381200f53d6cab7c"

# Запустити live
curl -X POST "http://65.109.8.78:8002/run/fill_invoice_payment?dry_run=false" \
  -H "Authorization: Bearer b082217377346689381200f53d6cab7c"

# Записати дату/TAX напряму в задачу
curl -X POST http://65.109.8.78:8002/invoice/fill \
  -H "Authorization: Bearer b082217377346689381200f53d6cab7c" \
  -H "Content-Type: application/json" \
  -d '{"pf_task_id": 4015, "payment_date": "2025-10-15", "tax_rate": 12}'
```

---

## Паттерн скрипту міграції

```python
import requests
import logging

PLANFIX_HOST = "https://itcomms.planfix.com"
PLANFIX_TOKEN = "6ca06006655c6e695c495a4705609c85"
MEGAPLAN_HOST = "https://likhtman.megaplan.ru"
MEGAPLAN_TOKEN = "NzZkODN..."

pf_headers = {"Authorization": f"Bearer {PLANFIX_TOKEN}", "Content-Type": "application/json"}
mp_headers = {"Authorization": f"Bearer {MEGAPLAN_TOKEN}"}

DRY_RUN = True  # False для реального запису

# Список задач по шаблону
def get_tasks(template_id, offset=0):
    r = requests.post(f"{PLANFIX_HOST}/rest/task/list", headers=pf_headers, json={
        "offset": offset,
        "pageSize": 100,
        "filters": [{"type": 325, "operator": "equal", "value": template_id}],
        "fields": "customField132121,customField130207"
    })
    return r.json()

# Оновити поле задачі
def update_task_field(task_id, field_id, value):
    if DRY_RUN:
        logging.info(f"DRY_RUN: task {task_id}, field {field_id} = {value}")
        return
    r = requests.post(
        f"{PLANFIX_HOST}/rest/task/{task_id}?silent=true",
        headers=pf_headers,
        json={"customFields": [{"field": {"id": field_id}, "value": value}]}
    )
    return r.json()
```
