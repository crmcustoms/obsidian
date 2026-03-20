# Firecrawl

→ [[claude-code/README|← Назад к обзору]]

## Описание

Веб-скрейпинг, поиск и исследование через API Firecrawl. Заменяет все встроенные веб-инструменты Claude. Возвращает чистый Markdown, обходит блокировки, рендерит JS.

## Установка

```
/plugin install firecrawl@claude-plugins-official
```

## Аутентификация

```bash
firecrawl login --browser
```

## Основные команды

| Команда | Описание |
|---------|----------|
| `firecrawl search "запрос"` | Поиск в интернете |
| `firecrawl scrape <url>` | Скрейпинг страницы |
| `firecrawl map <url>` | Карта всех URL сайта |
| `firecrawl crawl <url>` | Полный краул сайта |
| `firecrawl agent "задача"` | AI-агент для извлечения данных |

## Статус

- **Версия:** 1.0.3
- **Scope:** user (global)
- **Install path:** `~/.claude/plugins/cache/claude-plugins-official/firecrawl/1.0.3`

## Ссылки

- [GitHub](https://github.com/anthropics/claude-plugins-official/tree/main/plugins/firecrawl)
