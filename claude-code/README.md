# Claude Code — Configuration Hub

Центральный файл конфигурации Claude Code. Обновляется при каждом изменении окружения.

**Последнее обновление:** 2026-03-20

---

## Быстрая установка на новом компе

→ [[claude-code/setup|Setup Guide]]

---

## Установленные плагины

| Плагин | Тип | Статус | Описание |
|--------|-----|--------|----------|
| [[claude-code/plugins/firecrawl\|Firecrawl]] | MCP / Skill | ✅ Active | Веб-скрейпинг и поиск |
| [[claude-code/plugins/gsd\|GSD]] | Commands | ✅ Active | Spec-driven разработка |
| [[claude-code/plugins/telegram\|Telegram]] | Channel / MCP | ✅ Active | Управление через Telegram |
| [[claude-code/plugins/frontend-design\|Frontend Design]] | Skill | ✅ Active | UI/UX генерация |
| [[claude-code/plugins/megaplan\|Megaplan]] | Skill | ✅ Active | Интеграция с Megaplan |
| [[claude-code/plugins/mytets\|Mytets]] | Skill | ✅ Active | Театральный режим |

---

## Встроенные функции

- **Dispatch** — управление сессиями с телефона через приложение Claude (iOS/Android) → вкладка Dispatch. Не требует установки.

---

## Файлы настроек на компе

```
~/.claude/settings.json                                     # включённые плагины
~/.claude/plugins/installed_plugins.json                    # реестр плагинов
~/.claude/projects/APEX/.claude/channels/telegram/.env     # токен Telegram бота
~/.claude/telegram-mcp.json                                 # MCP конфиг для Telegram
```

---

## Проекты

| Проект | Путь | Описание |
|--------|------|----------|
| APEX | `C:\Users\tm\.claude\projects\APEX` | Синхронизация Finansist → Google Sheets |
