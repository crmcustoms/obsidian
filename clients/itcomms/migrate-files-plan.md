# ITCOMMS — План міграції файлів

## Задача
Перенести всі файли з угод Мегаплану в задачі Планфікс у відповідні поля.

**Шаблони:** 11 (Текущие клиенти), 15 (Конфеты нал), 7691 (Безнал)
**Зв'язок:** поле `132121` (ID сделки) в Планфікс → `deal.id` в Мегаплані

---

## Ключові факти

1. **Один Program 35 → два шаблони Планфікс** (15 і 7691), розділяємо по `TipPlatezha == "Конфеты"`
2. **Файли йдуть у конкретні поля** (type 21), а не просто в опис задачі
3. **Файли ЗАМІНЮЮТЬ** існуючі при оновленні поля — треба спочатку GET
4. Файли є і в полях угоди, і в коментарях (перевіряємо `attachesCountInComments > 0`)
5. URL Мегаплану потребує Bearer — скачуємо через сервер і завантажуємо мультипартом

---

## Маппінг файлових полів

```python
FILE_FIELD_MAP = {
    # Program 14 → шаблон 11
    14: {
        'Category1000061CustomFieldBrif':                  120571,  # Бриф
        'Category1000061CustomFieldSkanDogovora':          120579,  # Скан договора
        'Category1000061CustomFieldSkanPrilozheniyaIliDs': 120581,  # Скан Приложения (или ДС)
        'Category1000061CustomFieldAktVipolnennihRabot':   120585,  # Акт выполненных работ
    },
    # Program 35 → шаблони 15 і 7691
    35: {
        'Category1000083CustomFieldSkanOriginalaDogovora': 121019,  # Скан оригинала договора
        'Category1000083CustomFieldSkanDogovora':          120579,  # Скан договора
        'Category1000083CustomFieldAktSkan':               121023,  # Акт (скан)
    }
}
# Файли з коментарів → до опису задачі (task description files)
# Прямі attaches угоди → теж до опису задачі
```

---

## Алгоритм (псевдокод)

```
для кожного шаблону (11, 15, 7691):
  для кожної задачі Планфікс:
    1. читаємо поле 132121 → megaplan_deal_id
       якщо порожнє → skip

    2. GET /api/v3/deal/{deal_id} → вся угода
       program_id = int(deal['program']['id'])  # 14 або 35
       field_map = FILE_FIELD_MAP[program_id]

    3. Для кожного файлового поля:
       for mp_field, pf_field_id in field_map.items():
           mp_files = deal.get(mp_field, [])
           якщо mp_files порожнє → skip

           # Отримати існуючі файли в PF полі
           existing = get_task_field_files(pf_task_id, pf_field_id)

           # Завантажити нові файли
           new_file_ids = []
           for file in mp_files:
               pf_id = upload_from_megaplan(file)
               new_file_ids.append(pf_id)

           # Оновити поле (merge!)
           all_ids = existing + new_file_ids
           update_task_field_files(pf_task_id, pf_field_id, all_ids)

    4. Якщо deal['attachesCountInComments'] > 0:
       коментарі = GET /api/v3/deal/{deal_id}/comments
       comment_files = []
       for comment in коментарі:
           if comment.get('attaches'):
               comment_files += comment['attaches']
           # або GET /api/v3/comment/{id}/attaches

       # Файли з коментарів → в опис задачі
       existing_desc = GET /rest/task/{pf_task_id}/files
       new_ids = [upload_from_megaplan(f) for f in comment_files]
       POST /rest/task/{pf_task_id} {"files": existing_desc + new_ids}

    5. Якщо deal['attaches']:
       # Прямі вкладення угоди → також в опис задачі
       (аналогічно п.4)

    6. Лог: task_id, deal_id, program_id, файли по полях, файли з коментарів
```

---

## Код скрипту (основа)

```python
import requests, logging, time, os, tempfile

PLANFIX_HOST  = "https://itcomms.planfix.com"
PLANFIX_TOKEN = "6ca06006655c6e695c495a4705609c85"
MEGAPLAN_HOST = "https://likhtman.megaplan.ru"
MEGAPLAN_TOKEN = "NzZkODN..."

pf_headers = {"Authorization": f"Bearer {PLANFIX_TOKEN}", "Content-Type": "application/json"}
mp_headers = {"Authorization": f"Bearer {MEGAPLAN_TOKEN}"}

DRY_RUN = True  # False для live

FILE_FIELD_MAP = {
    14: {
        'Category1000061CustomFieldBrif':                  120571,
        'Category1000061CustomFieldSkanDogovora':          120579,
        'Category1000061CustomFieldSkanPrilozheniyaIliDs': 120581,
        'Category1000061CustomFieldAktVipolnennihRabot':   120585,
    },
    35: {
        'Category1000083CustomFieldSkanOriginalaDogovora': 121019,
        'Category1000083CustomFieldSkanDogovora':          120579,
        'Category1000083CustomFieldAktSkan':               121023,
    }
}

# Завантажити файл з Мегаплану → Планфікс
def upload_from_megaplan(mp_file):
    url = f"{MEGAPLAN_HOST}{mp_file['path']}"
    # Скачуємо з Мегаплану (потребує авторизації)
    r = requests.get(url, headers=mp_headers, timeout=60)
    r.raise_for_status()
    # Завантажуємо в Планфікс
    files = {'file': (mp_file['name'], r.content, mp_file.get('mimeType', 'application/octet-stream'))}
    pf_headers_upload = {"Authorization": f"Bearer {PLANFIX_TOKEN}"}
    r2 = requests.post(f"{PLANFIX_HOST}/rest/file/", headers=pf_headers_upload, files=files)
    return r2.json()['id']

# Оновити файлове поле задачі
def update_task_field_files(task_id, pf_field_id, file_ids):
    if DRY_RUN:
        logging.info(f"DRY: task {task_id} field {pf_field_id} ← {file_ids}")
        return
    r = requests.post(
        f"{PLANFIX_HOST}/rest/task/{task_id}?silent=true",
        headers=pf_headers,
        json={"customFields": [{"field": {"id": pf_field_id}, "value": [{"id": fid} for fid in file_ids]}]}
    )
    return r.json()

# Отримати існуючі файли в полі задачі
def get_task_field_files(task_id, pf_field_id):
    # GET /rest/task/{id} з конкретним полем
    r = requests.get(
        f"{PLANFIX_HOST}/rest/task/{task_id}",
        headers=pf_headers,
        params={"fields": f"customField{pf_field_id}"}
    )
    data = r.json()
    fields = data.get('customFieldData', [])
    for f in fields:
        if f.get('field', {}).get('id') == pf_field_id:
            return [v['id'] for v in f.get('value', [])]
    return []
```

---

## Нюанс: завантаження файлу

**Мегаплан URL потребує авторизації** — Планфікс не може сам завантажити по URL.
Варіант: скачати на сервер → завантажити мультипартом.

```python
# POST /rest/file/ (multipart)
files_payload = {'file': (filename, content_bytes, mime_type)}
headers_upload = {"Authorization": f"Bearer {PLANFIX_TOKEN}"}  # без Content-Type!
r = requests.post(f"{PLANFIX_HOST}/rest/file/", headers=headers_upload, files=files_payload)
pf_file_id = r.json()['id']
```

Альтернатива: `GET /api/v3/attache/shareUrl/{id}` → публічний URL → `POST /file/from-url/`

---

## Очікувана статистика

| Шаблон | Задач | Avg файлів | Час (~0.5s/файл) |
|--------|-------|-----------|------------------|
| 11 | ~500 | 2-4 | ~30 хв |
| 15 | ~1500 | 1-3 | ~60 хв |
| 7691 | ~800 | 1-3 | ~40 хв |

Всього: ~2-3 години з rate limiting (`time.sleep(0.3)` між запитами).

---

## Чеклист запуску

- [ ] Написати `migrate_files.py`
- [ ] Запустити dry_run на 10 задачах шаблону 15
- [ ] Перевірити що файли з'явились у правильних полях Планфікс
- [ ] Запустити dry_run на всіх шаблонах (лог кількості файлів)
- [ ] Запустити live на шаблоні 15
- [ ] Запустити live на шаблонах 7691 і 11
