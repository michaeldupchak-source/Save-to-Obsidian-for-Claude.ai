# Obsidian Saver — Claude Skill

[English](#english) | [Русский](#русский)

---

## English

A skill for [Claude](https://claude.ai) that saves notes from your conversation directly to your Obsidian vault as `.md` files. On first use, Claude asks where to save — and remembers it for all future conversations.

Works in both **Claude.ai** (web/desktop) and **Claude Code** (CLI).

### Features

- Saves notes directly to any folder in your Obsidian vault
- Three note formats to choose from
- Automatically names files by topic and timestamp: `Python Data Analysis - 28.01.2026 14:30.md`
- Writes notes in the same language the conversation was held in
- Adds a `#Claude` tag and a link back to the original conversation
- Remembers your vault path — no need to specify it every time
- Easy path reset with a single phrase
- Works on macOS, Linux, and Windows

---

### Using with Claude.ai (web / desktop app)

#### Requirements

**1. Claude.ai with Skills support**
Skills are available on Pro, Team, and Enterprise plans.

**2. MCP tool with filesystem access**
Claude.ai cannot write files to your computer by default. You need an MCP (Model Context Protocol) tool that provides filesystem access. The recommended option is **Desktop Commander**:

- GitHub: [github.com/wonderwhy-er/DesktopCommanderMCP](https://github.com/wonderwhy-er/DesktopCommanderMCP)
- Install via npm: `npx @wonderwhy-er/desktop-commander setup`

Without Desktop Commander (or a similar MCP tool), Claude will not be able to create files and the skill will not work.

**3. Obsidian** with an existing vault on your machine.

#### Installation

1. Install Desktop Commander (see above)
2. Download `obsidian-saver.skill`
3. Open Claude.ai → Settings → Skills
4. Upload the `.skill` file

---

### Using with Claude Code (CLI)

#### Requirements

**1. Claude Code installed**

```bash
npm install -g @anthropic-ai/claude-code
```

**2. No additional MCP tools needed** — Claude Code has built-in filesystem access.

**3. Obsidian** with an existing vault on your machine.

#### Installation

Extract the skill folder and place it in one of these locations:

- **Personal (all projects):** `~/.claude/skills/obsidian-saver/`
- **Project-specific:** `.claude/skills/obsidian-saver/` inside your project folder

```bash
# Personal installation example
unzip obsidian-saver.skill -d ~/.claude/skills/obsidian-saver/
```

> **Note:** Claude.ai and Claude Code store their configurations separately and do not sync. If you use both, install the skill in each environment individually. The vault path config (`~/.obsidian-saver.json`) is shared between both — so you only need to set the path once.

---

### How to use

Just say something like:

> "Save note" / "Create an Obsidian note" / "Save this conversation to Obsidian" / "Write note"

On **first use**, Claude will ask:

> "Please provide the path to the folder in your Obsidian vault where notes should be saved"

Enter your path — Claude saves it and won't ask again.

On every use, Claude will offer three formats:

| Format | Description |
|---|---|
| **Last reply only** | Saves only the last Claude response verbatim |
| **Full article** | A coherent, detailed article synthesized from the whole conversation — not a Q&A transcript. Includes all facts, links, code, and key details |
| **Compact note** | A brief summary: the gist, key facts, important commands/code, and links |

### Note structure

```
#Claude
[Full conversation](https://claude.ai/...)   ← if URL is available

# Note Title

**Date:** DD.MM.YYYY HH:MM

---

... content ...
```

### Changing the vault path

Say: `"change obsidian folder"` or `"reset obsidian path"` — Claude will delete the saved config and ask for a new path.

### Config file

The vault path is stored in `~/.obsidian-saver.json` (shared across Claude.ai and Claude Code):

```json
{
  "vault_path": "/Users/name/Documents/MyVault/Notes"
}
```

On Windows: `C:\Users\name\Documents\MyVault\Notes`

---

## Русский

Навык для [Claude](https://claude.ai), который сохраняет заметки из диалога прямо в ваш Obsidian vault в формате `.md`. При первом использовании Claude спросит, куда сохранять — и запомнит это для всех последующих разговоров.

Работает в **Claude.ai** (веб/десктоп) и в **Claude Code** (CLI).

### Возможности

- Сохраняет заметки напрямую в любую папку Obsidian vault
- Три формата заметок на выбор
- Автоматически называет файлы по теме и времени: `Анализ данных Python - 28.01.2026 14:30.md`
- Пишет заметки на том языке, на котором шёл диалог
- Добавляет тег `#Claude` и ссылку на исходный диалог
- Запоминает путь к vault — не нужно указывать каждый раз
- Смена пути одной фразой
- Работает на macOS, Linux и Windows

---

### Использование в Claude.ai (веб / десктоп)

#### Требования

**1. Claude.ai с поддержкой Skills**
Skills доступны на планах Pro, Team и Enterprise.

**2. MCP-инструмент с доступом к файловой системе**
Claude.ai по умолчанию не может записывать файлы на компьютер пользователя. Нужен MCP-инструмент (Model Context Protocol). Рекомендуемый вариант — **Desktop Commander**:

- GitHub: [github.com/wonderwhy-er/DesktopCommanderMCP](https://github.com/wonderwhy-er/DesktopCommanderMCP)
- Установка через npm: `npx @wonderwhy-er/desktop-commander setup`

Без Desktop Commander (или аналогичного инструмента) навык не будет работать.

**3. Obsidian** с существующим vault на вашем компьютере.

#### Установка

1. Установите Desktop Commander (см. выше)
2. Скачайте файл `obsidian-saver.skill`
3. Откройте Claude.ai → Настройки → Skills
4. Загрузите `.skill` файл

---

### Использование в Claude Code (CLI)

#### Требования

**1. Установленный Claude Code**

```bash
npm install -g @anthropic-ai/claude-code
```

**2. Дополнительные MCP-инструменты не нужны** — Claude Code имеет встроенный доступ к файловой системе.

**3. Obsidian** с существующим vault на вашем компьютере.

#### Установка

Распакуйте папку навыка в одно из мест:

- **Личная установка (все проекты):** `~/.claude/skills/obsidian-saver/`
- **Для конкретного проекта:** `.claude/skills/obsidian-saver/` внутри папки проекта

```bash
# Пример личной установки
unzip obsidian-saver.skill -d ~/.claude/skills/obsidian-saver/
```

> **Важно:** Claude.ai и Claude Code хранят конфигурации отдельно и не синхронизируются. Если вы используете оба продукта — установите навык в каждом из них вручную. Конфиг с путём к vault (`~/.obsidian-saver.json`) при этом общий — путь достаточно указать один раз.

---

### Как использовать

Просто скажите что-то вроде:

> "Сохрани заметку" / "Создай заметку в Obsidian" / "Сохрани разговор в Obsidian" / "Save note"

При **первом использовании** Claude спросит:

> "Укажи путь к папке в Obsidian, куда сохранять заметки"

Введите путь — Claude сохранит его и больше не будет спрашивать.

При каждом использовании Claude предложит три формата:

| Формат | Описание |
|---|---|
| **Только последний ответ** | Дословно сохраняет последний ответ Claude |
| **Подробная статья** | Связная, развёрнутая статья по теме всего диалога — не формат вопрос-ответ. Включает все факты, ссылки, код и детали |
| **Компактная заметка** | Краткая выжимка: суть, ключевые факты, важные команды/код, ссылки |

### Структура заметки

```
#Claude
[Полный диалог](https://claude.ai/...)   ← если URL доступен

# Название заметки

**Дата:** DD.MM.YYYY HH:MM

---

... содержимое ...
```

### Смена пути к vault

Скажите: `"смени папку obsidian"` или `"сбрось путь obsidian"` — Claude удалит конфиг и спросит новый путь.

### Конфиг-файл

Путь хранится в `~/.obsidian-saver.json` (общий для Claude.ai и Claude Code):

```json
{
  "vault_path": "/Users/name/Documents/MyVault/Заметки"
}
```

На Windows: `C:\Users\name\Documents\MyVault\Заметки`

---

## License

MIT
