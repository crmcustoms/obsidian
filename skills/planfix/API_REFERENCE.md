# Planfix API — Полная документация для Гедевана

## Доступ

**Аккаунт:** https://crmcustomsua.planfix.ua/  
**Токен:** `7c1410e144df333871be91847fc77885`  
**Base URL:** `https://crmcustomsua.planfix.ua/rest/`

## REST API v2

### Аутентификация
```
Authorization: Bearer 7c1410e144df333871be91847fc77885
Content-Type: application/json
```

### Основные endpoint'ы

#### Получить список задач
```bash
POST /rest/task/list
```

**Параметры:**
- `pageSize` — количество задач (по умолчанию 50)
- `fields` — список полей через запятую

**Пример:**
```bash
curl -X POST "https://crmcustomsua.planfix.ua/rest/task/list" \
  -H "Authorization: Bearer ТОКЕН" \
  -H "Content-Type: application/json" \
  -d '{
    "pageSize": 10,
    "fields": "id,name,status,description,assignee,dueDate"
  }'
```

**Ответ:**
```json
{
  "result": "success",
  "tasks": [
    {
      "id": 3,
      "name": "Загальний чат",
      "description": "",
      "status": {
        "id": 2,
        "name": "В роботі",
        "color": "#3377C3",
        "isActive": true
      }
    }
  ]
}
```

#### Получить задачу по ID
```bash
GET /rest/task/{id}
```

**Пример:**
```bash
curl -X GET "https://crmcustomsua.planfix.ua/rest/task/3" \
  -H "Authorization: Bearer ТОКЕН"
```

### Статусы задач (из твоего аккаунта)

| ID | Название | Цвет | Активный |
|----|----------|------|----------|
| 1 | Нове | #00b4b4 | ✅ |
| 2 | В роботі | #3377C3 | ✅ |
| 3 | Завершене | #85878E | ❌ |
| 6 | Виконане | #30AB50 | ✅ |
| 104 | Успешно | #94b300 | ❌ |

## Что я могу делать

### 1. Проверять задачи с дедлайнами
```bash
POST /rest/task/list
-d '{"fields": "id,name,dueDate,status", "filter": {...}}'
```

### 2. Создавать задачи
```bash
POST /rest/task
-d '{
  "name": "Новая задача",
  "description": "Описание",
  "status": {"id": 1}
}'
```

### 3. Обновлять задачи
```bash
POST /rest/task/{id}
-d '{
  "status": {"id": 6},
  "description": "Обновленное описание"
}'
```

### 4. Получать проекты
```bash
POST /rest/project/list
```

### 5. Получать контакты
```bash
POST /rest/contact/list
```

## Структура твоих задач

На основе первого запроса, у тебя 95 задач:
- **Активные статусы:** Нове, В роботі, Виконане
- **Завершённые:** Завершене, Успешно

## Идеи для автоматизации

1. **Проверка дедлайнов** — cron каждые 2 часа, напоминание о просроченных
2. **Ежедневный отчёт** — список задач в работе на сегодня
3. **Автоматическое создание** — задачи из Telegram/Email
4. **Синхронизация** — с Google Calendar для встреч

## Полная документация
- Swagger: https://help.planfix.com/restapidocs/

---
_Ку! 🪐_
