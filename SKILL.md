---
name: obsidian-saver
description: >
  Saves notes from the current conversation to an Obsidian vault. The folder path
  is asked on first use and remembered in ~/.obsidian-saver.json.
  ALWAYS use this skill when the user says things like:
  "create a note in obsidian", "save note", "save to obsidian", "save note",
  "create obsidian note", "save conversation to obsidian", "write to obsidian", "save this to obsidian",
  "create md file", "write note". Even if the phrasing is imprecise — if the user clearly wants
  to save something from the conversation to a file or Obsidian, use this skill.
---

# Obsidian Saver

A skill for saving notes from the current conversation to Obsidian.

## Compatibility

- **Claude.ai**: requires an MCP tool with filesystem access (e.g. Desktop Commander)
- **Claude Code**: works out of the box — filesystem access is built in
- Uses Python for all file operations — works on macOS, Linux, and Windows

---

## Step 0 — Determine save path

**First**, check for an existing config using Python:

```python
import json, os

config_path = os.path.expanduser("~/.obsidian-saver.json")
if os.path.exists(config_path):
    with open(config_path) as f:
        config = json.load(f)
    print(config["vault_path"])
else:
    print("NOT FOUND")
```

**If config exists** — use the `vault_path` value. Proceed to Step 1.

**If no config** — ask the user for the folder path:

> "Please provide the path to the folder in your Obsidian vault where notes should be saved (e.g. /Users/name/Documents/MyVault/Notes or C:\Users\name\Documents\MyVault\Notes)"

Once the user responds — save the config and create the folder:

```python
import json, os

vault_path = "PATH_FROM_USER"
config_path = os.path.expanduser("~/.obsidian-saver.json")

os.makedirs(vault_path, exist_ok=True)

with open(config_path, 'w') as f:
    json.dump({"vault_path": vault_path}, f)

print("Saved")
```

Confirm: `Path saved. All notes will be saved to: <path>`

---

## Step 1 — Offer format options

Offer 3 options via `ask_user_input_v0`:

```
Question: "How would you like to save the note?"
Type: single_select
Options:
  1. "Last reply only" — save only the last Claude response
  2. "Full article" — detailed summary of the entire conversation with facts, links, and details
  3. "Compact note" — brief summary: key facts, links, and main points only
```

---

## Step 2 — Generate filename

Format: `<Main topic of conversation> - <DD.MM.YYYY HH:MM>.md`

- Determine the topic from the conversation context (briefly, 2–5 words)
- Get date and time via Python:

```python
from datetime import datetime
print(datetime.now().strftime("%d.%m.%Y %H:%M"))
```

- Example: `Python Data Analysis - 28.01.2026 14:30.md`
- The topic should be in the **same language as the conversation**

---

## Step 3 — Generate content

### Language rule

Write the entire note (headings, content, summaries) in the **same language the conversation was conducted in**. If the conversation was in Russian — write in Russian. If in English — write in English. If mixed — use the dominant language.

### Getting the conversation URL

Try to get the current conversation URL via JavaScript in the browser context — it may be available as `window.location.href`. If it's accessible, include it as a Markdown link on the second line of the note. If not available, omit that line silently.

### Common structure for all options:

```markdown
#Claude
[Full conversation](<conversation URL if available>)

# <Note title (topic)>

**Date:** <DD.MM.YYYY HH:MM>

---

<content depending on chosen option>
```

If the conversation URL is not available, omit the second line entirely.

---

### Option 1: Last reply only

Insert the last Claude response verbatim. Preserve formatting as-is.

---

### Option 2: Full article

This is **not** a Q&A format. A coherent, detailed article on the conversation topic.

```markdown
## Introduction
Brief context of the topic.

## <Logical sections by topic>
Detailed presentation of all important information from the conversation.
Include:
- all facts, figures, terms
- all links (in format [text](url))
- code examples (in ``` blocks)
- step-by-step instructions if any
- warnings and important notes

## Conclusion / Summary
Brief recap and takeaways.

## Links and Resources
List of all mentioned links.
```

Goal: the reader should fully understand the topic without referring to the original conversation.

---

### Option 3: Compact note

Only the essentials, no fluff:

```markdown
## Summary
1–3 sentences about what the conversation was about.

## Key Facts
- fact 1
- fact 2

## Important Details / Commands / Code
Only what's worth remembering.

## Links
- [title](url)
```

---

## Step 4 — Save the file

```python
import json, os

config_path = os.path.expanduser("~/.obsidian-saver.json")
with open(config_path) as f:
    config = json.load(f)

vault_path = config["vault_path"]
filename = "<topic> - <DD.MM.YYYY HH:MM>.md"
full_path = os.path.join(vault_path, filename)

content = """..."""

with open(full_path, 'w', encoding='utf-8') as f:
    f.write(content)

print(full_path)
```

---

## Step 5 — Confirm save

```
Note saved to Obsidian:
<filename>
<full path>
```

---

## Reset path command

If the user says "change obsidian folder", "reset obsidian path", or "change obsidian path" — delete the config and repeat Step 0:

```python
import os
config_path = os.path.expanduser("~/.obsidian-saver.json")
os.remove(config_path)
print("Config removed")
```

---

## Important rules

- Write the entire note in the **same language as the conversation**
- First line of the file is always `#Claude`
- Second line is `[Full conversation](url)` — only if URL is available
- Filename topic should match the conversation language
- Links: `[link text](https://...)`
- Code blocks wrapped in triple backticks with language specified