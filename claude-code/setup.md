# Setup Guide — Новый компьютер

Полная инструкция по восстановлению окружения Claude Code на новом компьютере.

→ [[claude-code/README|← Назад к обзору]]

---

## 1. Установка Claude Code

```bash
npm install -g @anthropic-ai/claude-code
```

---

## 2. Установка Bun

```bash
curl -fsSL https://bun.sh/install | bash
```

Добавить в `~/.bashrc`:
```bash
export BUN_INSTALL="$HOME/.bun"
export PATH="$BUN_INSTALL/bin:$PATH"
```

---

## 3. Установка плагинов

### Официальные (через Claude Code)

Запустить `claude`, затем:
```
/plugin install firecrawl@claude-plugins-official
/plugin install frontend-design@claude-plugins-official
/plugin install telegram@claude-plugins-official
```

### GSD

```bash
npx get-shit-done-cc --claude --global
```

### Локальные плагины (megaplan, mytets)

Скопировать папки из бэкапа в:
```
~/.claude/plugins/marketplaces/local-desktop-app-uploads/
```

---

## 4. Настройки settings.json

Создать `~/.claude/settings.json`:
```json
{
  "enabledPlugins": {
    "frontend-design@claude-plugins-official": true,
    "mytets@local-desktop-app-uploads": true,
    "firecrawl@claude-plugins-official": true,
    "telegram@claude-plugins-official": true
  }
}
```

---

## 5. Настройка Telegram

1. Токен бота хранится отдельно — см. [[claude-code/plugins/telegram|Telegram plugin]]
2. Создать файл:
```bash
mkdir -p ~/.claude/channels/telegram
echo "TELEGRAM_BOT_TOKEN=<токен>" > ~/.claude/channels/telegram/.env
```
3. Создать `~/.claude/telegram-mcp.json`:
```json
{
  "mcpServers": {
    "telegram": {
      "command": "bun",
      "args": ["run", "--cwd", "<путь_к_плагину>", "--shell=bun", "--silent", "start"]
    }
  }
}
```
4. Запустить: `claude --mcp-config ~/.claude/telegram-mcp.json`
5. Написать боту → получить код → `/telegram:access pair <code>`
6. Заблокировать: `/telegram:access policy allowlist`

---

## Связанные файлы

- [[claude-code/plugins/firecrawl]]
- [[claude-code/plugins/gsd]]
- [[claude-code/plugins/telegram]]
- [[claude-code/plugins/frontend-design]]
- [[claude-code/plugins/megaplan]]
- [[claude-code/plugins/mytets]]
