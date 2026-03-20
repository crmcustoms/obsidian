# ITCOMMS — Статус міграції

Останнє оновлення: 2026-03-20

## Загальний прогрес

| Етап | Статус | Дата | Деталі |
|------|--------|------|--------|
| fill_supplier | ✅ Готово | 2026-02-26 | 2042 заповнено, 701 not_found |
| fill_payment_date | ✅ Готово | — | Поле 138909 в шаблонах 15 + 7691 |
| fill_invoice_payment | ✅ Готово | — | Поле 128157 + TAX datatag в шаблоні 21 |
| migrate_files | 🔲 В плані | — | Файли з угод Мегаплану → задачі Планфікс |
| field-mapping | 🔲 В плані | — | Повний маппінг полів між системами |

---

## Деталі по етапах

### ✅ fill_supplier
- Скрипт: `fill_supplier.py` + `fill_supplier_from_megaplan.py`
- Заповнює поля 136609 (Поставщик конф.) і 136611 (Поставщик безн.)
- Шаблони: 15 і 7691
- 701 not_found — шукалися через `fill_supplier_from_megaplan.py` по Мегаплану

### ✅ fill_payment_date
- Скрипт: `fill_payment_date.py`
- Поле 138909 (Дата оплаты) з Мегаплану → Планфікс
- Шаблони: 15 і 7691

### ✅ fill_invoice_payment
- Скрипт: `fill_invoice_payment.py`
- Поле 128157 (Invoice Payment Date) — фактична дата з Мегаплану
- TAX datatag — datatag 9611 з полем 58393 (ставка ПДВ)
- Джерело: n8n webhook (список рахунків) + Мегаплан API (actualPaymentDate)
- Фільтр: рахунки серп 2025 – лют 2026
- Результат: 60 вже заповнені (already_set), 9 pf_not_found (номери з # та -)

### 🔲 migrate_files
- Шаблони: 11, 15, 7691
- Поле 132121 = Megaplan deal ID → беремо файли
- Файли з полів угоди: `GET /api/v3/deal/{id}/attaches`
- Файли з коментарів: `GET /api/v3/deal/{id}/comments` + `GET /api/v3/comment/{id}/attaches`
- Завантаження в Планфікс: `POST /file/from-url/` → `POST /task/{id}` з `files: [{id}]`
- ⚠️ Файли ЗАМІНЮЮТЬ існуючі — спочатку GET поточних файлів задачі

---

## Відомі проблеми / нюанси

1. **TAX has_tax detection** — поле 128159 (агрегат) ненадійне (0 для 0% ставки).
   Рішення: preload всіх записів datatag через `POST /datatag/9611/entry/list` з `fields="key,task"`.

2. **Мегаплан пагінація** — не підтримується для `/api/v3/invoice`. Перші 100 записів покривають потрібний період.

3. **Планфікс файли через API** — йдуть тільки в опис задачі, не як окремі вкладення.
   При оновленні `files` — замінюють існуючі. Потрібно спочатку GET + merge.

4. **Планфікс silent mode** — додавати `?silent=true` для міграційних запитів щоб не спамити сповіщення.

5. **Datatag endpoint** — правильний: `POST /task/{id}/datatags/` (НЕ `/datatag/{id}/entry/`!)
