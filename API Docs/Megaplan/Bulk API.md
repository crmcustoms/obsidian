# Bulk API — Megaplan API v3

← [[Megaplan API — Index]]

---

## Зачем

Пакетный вызов нескольких запросов за **один HTTP-запрос**. Экономит время на round-trip.

---

## Пакетный вызов

```
POST /api/v3/api/v3/bulk
```

```json
{
  "contentType": "BulkApiCall",
  "calls": [
    {
      "contentType": "ApiCall",
      "method": "GET",
      "url": "/api/v3/deal/123"
    },
    {
      "contentType": "ApiCall",
      "method": "GET",
      "url": "/api/v3/invoice/456"
    },
    {
      "contentType": "ApiCall",
      "method": "POST",
      "url": "/api/v3/contractorCompany/1",
      "body": "{\"contentType\":\"ContractorCompany\",\"name\":\"Новое имя\"}"
    }
  ]
}
```

Ответ — массив результатов в том же порядке.

---

## Получить полные сущности по ссылкам

```
POST /api/v3/api/v3/bulk/getEntitiesByLinks
```

```json
[
  {"contentType": "Deal", "id": "1"},
  {"contentType": "Invoice", "id": "2"},
  {"contentType": "ContractorCompany", "id": "5"}
]
```

Сущности которые не существуют или недоступны — не возвращаются. Порядок не гарантирован.

---

## Типы ApiCall

```json
{
  "contentType": "ApiCall",
  "method": "GET|POST|PUT|PATCH|DELETE",
  "url": "/api/v3/...",
  "body": "JSON строка (для POST/PATCH)"
}
```

---

## Связанные темы

- [[Пагинация и поля]]
- [[Фильтрация]]
- [[Megaplan API — Index]]
