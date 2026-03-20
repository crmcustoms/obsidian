# Telegram Channel

→ [[claude-code/README|← Назад к обзору]]

## Описание

MCP-сервер, подключающий Telegram-бота к Claude Code. Позволяет отправлять задачи и получать ответы прямо из Telegram.

## Бот

- Создан через [@BotFather](https://t.me/BotFather)
- Токен хранится в `~/.claude/projects/APEX/.claude/channels/telegram/.env`
- **Токен нигде не публиковать**

## Установка

```
/plugin install telegram@claude-plugins-official
```

Требует **Bun** — см. [[claude-code/setup|Setup Guide]].

## Запуск

```bash
claude --mcp-config ~/.claude/telegram-mcp.json
```

Файл `~/.claude/telegram-mcp.json`:
```json
{
  "mcpServers": {
    "telegram": {
      "command": "bun",
      "args": ["run", "--cwd", "~/.claude/plugins/cache/claude-plugins-official/telegram/<версия>", "--shell=bun", "--silent", "start"]
    }
  }
}
```

## Первоначальное спаривание

1. Написать боту в Telegram
2. Бот ответит 6-значным кодом
3. В Claude Code: `/telegram:access pair <code>`
4. Заблокировать доступ: `/telegram:access policy allowlist`

## Инструменты доступные Claude

| Инструмент | Описание |
|-----------|----------|
| `reply` | Отправить сообщение в чат |
| `react` | Добавить реакцию на сообщение |
| `edit_message` | Редактировать отправленное сообщение |

## Статус

- **Версия:** b664e152af57 (commit SHA)
- **Scope:** user (global)
- **Install path:** `~/.claude/plugins/cache/claude-plugins-official/telegram/b664e152af57`

## Ссылки

- [GitHub](https://github.com/anthropics/claude-plugins-official/tree/main/external_plugins/telegram)
- [ACCESS.md](https://github.com/anthropics/claude-plugins-official/blob/main/external_plugins/telegram/ACCESS.md)
