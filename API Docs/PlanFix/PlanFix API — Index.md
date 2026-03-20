# PlanFix API — Index

Base URL REST: `https://ваш_аккаунт.planfix.ru/rest/`
Base URL XML:  `https://ваш_аккаунт.planfix.ru/xml/`
Swagger spec:  `https://help.planfix.com/restapidocs/swagger.json`
Docs:          https://planfix.com/ru/help/REST_API
Auth REST:     `Authorization: Bearer <token>`
Auth XML:      подпись MD5 или токен в заголовке

Требует платного или премиум-аккаунта.

---

## Навигация

| Тема | Файл |
|------|------|
| Обзор REST API, история изменений | [[REST API — Обзор]] |
| Авторизация, токены, scope, Postman | [[REST API — Авторизация]] |
| Задачи (XML API) — CRUD, статусы, фильтры | [[REST API — Задачи (XML)]] |
| Контакты (XML API) — CRUD, фильтры | [[REST API — Контакты (XML)]] |
| Проекты (XML API) — CRUD, группы | [[REST API — Проекты (XML)]] |
| Комментарии / действия (XML API) | [[REST API — Комментарии (XML)]] |
| Сотрудники и группы пользователей (XML API) | [[REST API — Пользователи (XML)]] |
| Справочники и аналитики (XML API) | [[REST API — Справочники]] |
| Файлы — upload/download (XML API) | [[REST API — Файлы]] |
| XML API v1 — устаревший XML-интерфейс | [[XML API v1]] |
| Входящие вебхуки, HTTP-запросы | [[Вебхуки и HTTP]] |
| Парсинг ответов: JSONPath и XPath | [[JSONPath и XPath]] |
| Интеграции: SMS, email, телефония, чаты | [[API для интеграций]] |

---

## Ключевые правила REST API

- Авторизация: `Authorization: Bearer <token>` в каждом запросе
- Для GET-запросов токен передаётся параметром `access_token`
- Только HTTPS — незащищённые соединения игнорируются
- Токены создаются в: Управление аккаунтом → Доступ к API → REST API
- Можно ограничить токен scope'ами (task_readonly, contact_add, и т.д.)
- У каждого аккаунта свой адрес: `https://домен_аккаунта/rest`

## Ключевые правила XML API v1

- Base URL: `https://ваш_аккаунт.planfix.ru/xml/`
- Аутентификация через MD5-подпись или токен
- Все запросы — POST с XML-телом
- Пагинация: параметры `pageCurrent` и `pageSize`
- Форматы дат: DateTime (`DD-MM-YYYY HH:MM`), date (`DD-MM-YYYY`)

---

## Scope (уровни доступа) — быстрый справочник

| Ресурс | readonly | add | update | delete |
|--------|----------|-----|--------|--------|
| Задачи | task_readonly | task_add | task_update | — |
| Контакты | contact_readonly | contact_add | contact_update | — |
| Сотрудники | user_readonly | user_add | user_update | — |
| Комментарии | comment_readonly | comment_add | comment_update | comment_delete |
| Аналитики | datatag_readonly | datatag_add | datatag_update | datatag_delete |
| Файлы | file_readonly | — | — | file_delete |

Полный список: [[REST API — Авторизация]]

---

*Источник: спарсено с https://planfix.com/ru/help/, март 2026*
