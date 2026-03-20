# Excalidraw Diagram Skill

→ [[claude-code/README|← Назад к обзору]]

## Описание

Skill от Cole Medin для генерации Excalidraw-диаграмм по текстовому описанию. Диаграммы не просто показывают — они **визуально аргументируют**: fan-out для one-to-many, timeline для последовательностей, convergence для агрегации. Включает визуальную валидацию через Playwright.

## Установка

```bash
git clone https://github.com/coleam00/excalidraw-diagram-skill.git
cp -r excalidraw-diagram-skill ~/.claude/skills/excalidraw-diagram
```

### Настройка рендерера (для визуальной валидации)

Требует **uv** и **playwright**:

```bash
# Установить uv (Windows)
powershell -Command "irm https://astral.sh/uv/install.ps1 | iex"

# Установить зависимости
cd ~/.claude/skills/excalidraw-diagram/references
uv sync
uv run playwright install chromium
```

## Использование

Просто описать диаграмму:

> "Создай диаграмму синхронизации данных из Finansist в Google Sheets"

Skill сам: маппит концепции → генерирует JSON → рендерит → валидирует → исправляет.

## Кастомизация цветов

Редактировать `references/color-palette.md` — все диаграммы будут следовать палитре.

## Онлайн редактор

[excalidraw.com](https://excalidraw.com) — открыть `.excalidraw` файл для ручного редактирования.

## Структура

```
~/.claude/skills/excalidraw-diagram/
├── SKILL.md                  # Методология + воркфлоу
└── references/
    ├── color-palette.md      # Цвета бренда (редактировать здесь)
    ├── element-templates.md  # JSON шаблоны элементов
    ├── json-schema.md        # Формат Excalidraw JSON
    ├── render_excalidraw.py  # Рендер в PNG
    ├── render_template.html  # Браузерный шаблон
    └── pyproject.toml        # Python зависимости
```

## Статус

- **Тип:** Claude Code Skill (глобальный)
- **Install path:** `~/.claude/skills/excalidraw-diagram`

## Ссылки

- [GitHub](https://github.com/coleam00/excalidraw-diagram-skill)
- [Excalidraw онлайн](https://excalidraw.com)
