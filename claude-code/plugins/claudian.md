# Claudian

→ [[claude-code/README|← Назад к обзору]]

## Описание

Obsidian-плагин, который встраивает Claude Code прямо в vault. Vault становится рабочей директорией Claude — полные агентные возможности: чтение/запись файлов, поиск, bash-команды, многошаговые воркфлоу.

## Установка (из GitHub Release)

```bash
# Создать папку плагина
mkdir -p /path/to/vault/.obsidian/plugins/claudian

# Скачать 3 файла из latest release
curl -L https://github.com/YishenTu/claudian/releases/download/1.3.70/main.js -o .obsidian/plugins/claudian/main.js
curl -L https://github.com/YishenTu/claudian/releases/download/1.3.70/manifest.json -o .obsidian/plugins/claudian/manifest.json
curl -L https://github.com/YishenTu/claudian/releases/download/1.3.70/styles.css -o .obsidian/plugins/claudian/styles.css
```

Затем в Obsidian: **Settings → Community plugins → Enable "Claudian"**

## Требования

- Claude Code CLI установлен
- Obsidian v1.8.9+
- Claude подписка или API ключ

## Ключевые возможности

| Функция | Описание |
|---------|----------|
| **Inline Edit** | Выделить текст + хоткей → редактировать прямо в заметке с diff-preview |
| **Plan Mode** | `Shift+Tab` — Claude изучает и предлагает план перед выполнением |
| **Skills** | Поддержка Claude Code skills из `~/.claude/skills/` |
| **Plugins** | Все установленные Claude Code плагины доступны автоматически |
| **@-упоминания** | `@файл`, `@агент`, `@mcp-сервер` прямо в чате |
| **Vision** | Drag-drop/paste изображений в чат |
| **MCP** | Подключение внешних MCP-серверов |
| **YOLO mode** | Авто-акцепт всех действий без подтверждений |

## Интеграция с Claude Code плагинами

Claudian автоматически подхватывает все плагины из `~/.claude/plugins`. Включить в настройках: **Settings → Claude Code Plugins**.

Значит GSD, Firecrawl, Telegram — всё работает и внутри Obsidian.

## Troubleshooting — Windows

Если `spawn claude ENOENT`:

```
Settings → Advanced → Claude CLI path
```

Найти путь:
```powershell
where.exe claude
# Пример: C:\Users\tm\AppData\Local\Claude\claude.exe
```

Использовать `.exe`, не `.cmd`.

## Статус

- **Версия:** 1.3.70
- **Тип:** Obsidian Community Plugin
- **Install path:** `H:\backup\Gedevan\obsidian\.obsidian\plugins\claudian`
- **Установлен:** 2026-03-20

## Ссылки

- [GitHub](https://github.com/YishenTu/claudian)
- [Releases](https://github.com/YishenTu/claudian/releases/latest)
