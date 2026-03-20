# Finansist API

**Base URL:** `https://rest.api.report.finance`
**Auth:** Header `apiKey: <key>`
**Pagination:** `?offset=N` (ліміт 100 записів на сторінку)

---

## Аутентифікація

```http
GET /api/Payments
apiKey: XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
```

> ⚠️ Доступ до API обмежений по IP — тільки з білого списку серверів.

---

## Основні ендпоінти

### ПЛАН — PaymentRequests (заявки на платіж)

```
GET /api/PaymentRequests
```

**Обов'язковий параметр:** `statusId` (без нього — 400 Bad Request)

| Параметр | Значення |
|----------|----------|
| `statusId` | 710, 711, 712, 713 (передавати всі) |
| `offset` | 0, 100, 200... |

**Ключові поля відповіді:**

| Поле | Тип | Опис |
|------|-----|------|
| `id` | int | ID заявки |
| `externalId` | string | Зовнішній ID (з Мегаплану) |
| `paymentDate` | string | Дата платежу (YYYY-MM-DD) |
| `sourcePaymentSum` | float | Сума |
| `sourceCurrencyId` | string | Валюта (RUB/USD/EUR) |
| `status` | int | Статус (710/711/712/713) |
| `userName` | string | ФІО відповідального |
| `userId` | int | ID відповідального |
| `contragentId` | int | ID контрагента |
| `projectId` | int | ID проєкту |
| `organisationId` | int | ID організації |
| `factStreamId` | int | ID статті ДДС |
| `accountId` | int | ID рахунку |
| `paymentFor` | string | Оплата за |
| `paymentPurpose` | string | Призначення платежу |
| `comment` | string | Коментар |
| `isUrgent` | bool | Термінова |
| `tags` | array | Теги |
| `invoiceId` | int | ID рахунку-фактури |

**Статуси:**
| Код | Назва |
|-----|-------|
| 710 | Створена/На підписання |
| 711 | На підписанні |
| 712 | Підписана |
| 713 | Відкликана |

---

### ФАКТ — Payments (фактичні платежі)

```
GET /api/Payments
```

**Ключові поля відповіді:**

| Поле | Тип | Опис |
|------|-----|------|
| `id` | int | ID платежу |
| `externalId` | string | Зовнішній ID |
| `paymentDate` | string | Дата (YYYY-MM-DD) |
| `paymentSum` | float | Сума |
| `sourcePaymentSum` | float | Вихідна сума |
| `sourceCurrencyId` | string | Валюта |
| `paymentStatusId` | int | ID статусу |
| `responsible` | string | Відповідальний (рядок) |
| `contragentId` | int | ID контрагента |
| `projectId` | int | ID проєкту |
| `organisationId` | int | ID організації |
| `factStreamId` | int | ID статті ДДС |
| `accountId` | int | ID рахунку |
| `comment` | string | Коментар |
| `tags` | array | Теги |

---

## Довідники

| Ендпоінт | Ключ списку | Опис |
|----------|-------------|------|
| `GET /api/Projects` | `listProject` | Проєкти |
| `GET /api/ProjectGroups` | `listProjectGroup` | Групи проєктів |
| `GET /api/Contragents` | — | Контрагенти |
| `GET /api/Accounts` | — | Банківські рахунки |
| `GET /api/FactStreams` | — | Статті ДДС (факт) |
| `GET /api/Streams` | — | Статті ДДС (план) |
| `GET /api/Organisations` | — | Організації |
| `GET /api/Users` | — | Користувачі (`name`) |
| `GET /api/Manager` | — | Менеджери (`fullName`) |

---

## Пагінація

```python
offset = 0
while True:
    data = get(endpoint, params={"offset": offset, ...})
    items = data.get("listPayment") or data.get("data") or []
    if not items:
        break
    yield from items
    offset += 100
```

---

## Відомі особливості

- `PaymentRequests` вимагає `statusId` — без нього повертає 400
- Поле дати може бути `paymentDate`, `date`, або `operationDate` залежно від типу
- API доступний тільки з IP що в білому списку
- Сервер іноді повертає 500 — потрібен retry (tenacity)
- Поле `status` в PaymentRequests (не `statusId`!)
- Поле `paymentStatusId` в Payments (не `statusId`!)
