# ITCOMMS — План міграції файлів

## Задача
Перенести всі файли з угод Мегаплану в задачі Планфікс.

**Шаблони:** 11 (Текущие клиенти), 15 (Конфеты нал), 7691 (Безнал)
**Зв'язок:** поле `132121` (ID сделки) в Планфікс → `deal.id` в Мегаплані
**Файли:** з полів угоди + з коментарів угоди

---

## Алгоритм

```
для кожного шаблону (11, 15, 7691):
  для кожної задачі Планфікс:
    1. читаємо поле 132121 → megaplan_deal_id
    2. якщо megaplan_deal_id порожній → пропуск
    3. GET /api/v3/deal/{deal_id}/attaches → файли з полів
    4. GET /api/v3/deal/{deal_id}/comments → список коментарів
       для кожного коментаря:
         GET /api/v3/comment/{comment_id}/attaches → файли
    5. об'єднуємо всі файли (унікальні по id)
    6. якщо файлів немає → пропуск
    7. GET /task/{pf_task_id}/files → існуючі файли задачі
    8. для кожного файлу Мегаплану:
       a. POST /file/from-url/ {"url": megaplan_file_url, "name": file.name}
          → pf_file_id
    9. POST /task/{pf_task_id}?silent=true
       {"files": [*existing_pf_file_ids, *new_pf_file_ids]}
```

---

## Мегаплан — отримати файли

```python
import requests

MEGAPLAN_HOST = "https://likhtman.megaplan.ru"
MEGAPLAN_TOKEN = "NzZkODN..."
mp_headers = {"Authorization": f"Bearer {MEGAPLAN_TOKEN}"}

# Файли з полів угоди
def get_deal_attaches(deal_id):
    r = requests.get(f"{MEGAPLAN_HOST}/api/v3/deal/{deal_id}/attaches", headers=mp_headers)
    return r.json().get('data', {}).get('attaches', [])

# Файли з коментарів
def get_deal_comment_files(deal_id):
    files = []
    # Отримати всі коментарі
    r = requests.get(f"{MEGAPLAN_HOST}/api/v3/deal/{deal_id}/comments", headers=mp_headers)
    comments = r.json().get('data', {}).get('comments', [])
    for comment in comments:
        cid = comment['id']
        r2 = requests.get(f"{MEGAPLAN_HOST}/api/v3/comment/{cid}/attaches", headers=mp_headers)
        attaches = r2.json().get('data', {}).get('attaches', [])
        files.extend(attaches)
    return files

# Всі файли угоди (альтернативно - один endpoint)
def get_all_deal_files(deal_id):
    r = requests.get(
        f"{MEGAPLAN_HOST}/api/v3/deal/{deal_id}/allFiles",
        params={"modelId": deal_id, "limit": 100},
        headers=mp_headers
    )
    return r.json().get('data', {}).get('files', [])
```

---

## Планфікс — завантажити та прикріпити

```python
PLANFIX_HOST = "https://itcomms.planfix.com"
PLANFIX_TOKEN = "6ca06006655c6e695c495a4705609c85"
pf_headers = {"Authorization": f"Bearer {PLANFIX_TOKEN}", "Content-Type": "application/json"}

# Отримати поточні файли задачі
def get_task_files(task_id):
    r = requests.get(f"{PLANFIX_HOST}/rest/task/{task_id}/files", headers=pf_headers)
    return r.json().get('files', [])

# Завантажити файл з Мегаплану в Планфікс
def upload_file_from_megaplan(mp_file):
    file_url = f"{MEGAPLAN_HOST}{mp_file['path']}"
    # Планфікс завантажить файл сам по URL
    r = requests.post(
        f"{PLANFIX_HOST}/rest/file/from-url/",
        headers=pf_headers,
        json={"url": file_url, "name": mp_file['name']}
    )
    # ⚠️ Але URL Мегаплану потребує авторизації — можливо треба спочатку скачати локально
    return r.json().get('id')

# Оновити файли задачі (merge з існуючими!)
def attach_files_to_task(task_id, new_file_ids, dry_run=True):
    existing = get_task_files(task_id)
    existing_ids = [{"id": f["id"]} for f in existing]
    new_ids = [{"id": fid} for fid in new_file_ids]
    all_files = existing_ids + new_ids

    if dry_run:
        print(f"DRY: task {task_id}: додати {len(new_file_ids)} файлів (є {len(existing_ids)})")
        return

    r = requests.post(
        f"{PLANFIX_HOST}/rest/task/{task_id}?silent=true",
        headers=pf_headers,
        json={"files": all_files}
    )
    return r.json()
```

---

## Нюанс: авторизований URL Мегаплану

Планфікс при `POST /file/from-url/` завантажує файл сам по URL. Але URL Мегаплану
(`https://likhtman.megaplan.ru/file/download/...`) потребує Bearer token.

**Варіант А:** Спочатку скачати файл на сервер → завантажити в Планфікс через `POST /file/`
```python
# Скачати з Мегаплану
mp_response = requests.get(f"{MEGAPLAN_HOST}{file['path']}", headers=mp_headers)
# Завантажити в Планфікс
pf_response = requests.post(
    f"{PLANFIX_HOST}/rest/file/",
    headers={"Authorization": f"Bearer {PLANFIX_TOKEN}"},
    files={"file": (file['name'], mp_response.content, file['mimeType'])}
)
```

**Варіант Б:** Отримати публічний URL через Мегаплан shareUrl → передати в Планфікс
```python
r = requests.get(f"{MEGAPLAN_HOST}/api/v3/attache/shareUrl/{file['id']}", headers=mp_headers)
public_url = r.json().get('data', {}).get('url')
# Потім POST /file/from-url/ з public_url
```

**Рекомендація:** Варіант А (надійніший). Файли проходять через сервер.

---

## Потенційні проблеми

1. **Розмір файлів** — великі файли можуть уповільнити міграцію. Додати timeout.
2. **Дублікати** — перевіряти чи файл вже є (по імені) перед завантаженням.
3. **Rate limiting** — додати `time.sleep(0.2)` між запитами.
4. **Угоди без файлів** — пропускати (not_found логувати окремо).
5. **Планфікс "files replace"** — обов'язково робити GET існуючих файлів перед update!

---

## Статистика (очікувана)

- Шаблон 11: ~500+ задач
- Шаблон 15: ~1500+ задач
- Шаблон 7691: ~800+ задач
- Середнє файлів на угоду: ~2-5
- Очікуваний час: 2-4 год (з rate limiting)
