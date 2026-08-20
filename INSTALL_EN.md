[![English](https://img.shields.io/badge/lang-English-blue.svg)](INSTALL_EN.md)
[![简体中文](https://img.shields.io/badge/lang-%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87-red.svg)](INSTALL.md)

# Installation Guide

## ⚠ AI Assistant Install Protocol (Required Reading)

> When a user requests "install embed-ai-tool skills", AI assistants (Claude Code / Cursor / Codex, etc.) **MUST** follow this flow. **Never** blindly copy all skills.

### Three Prohibitions

1. **Do NOT** run `install.py` for full install without explicit user confirmation — new default is analyze-only; requires `--skills X Y` or `--yes` to write files
2. **Do NOT** use `cp -r` / `xcopy` / `Copy-Item` / file explorer to bulk-copy the `skills/` directory into the target project — this pollutes `.claude/skills/`
3. **Do NOT** bypass `install.py` with other tools (PowerShell, FileExplorer, Robocopy, etc.) to achieve equivalent bulk copy

### Required Flow

1. **Analyze project** — In the target project, run `python3 scripts/install.py /path/to/project` (no install args). The script prints project-type detection + recommended set, **writes nothing**
2. **Present to user** — Show analysis result + 4 options:
   - **A. Recommended set** (based on project analysis, typically 5-7 skills)
   - **B. Install all** (24 skills, `--yes`)
   - **C. Category-by-category** (6 categories asked in sequence)
   - **D. Custom** (enter skill names directly)
3. **Wait for user choice** — Only after explicit selection, run `--skills X Y` or `--yes`
4. **Report** — List installed skills when done

### Why

The 24 skills cover multiple toolchains (Keil / IAR / ESP-IDF / PlatformIO / CMake / Makefile). A single project typically uses only 4-7 of them. Bulk install pollutes `.claude/skills/`, adds noise to the Claude skill list, and affects project collaborators.

---

## One-Click Install

In any LLM chat that supports skills, enter:

```
Install skills from https://github.com/LeoKemp223/embed-ai-tool.git
```

The AI assistant will **first analyze your project type** (build system, debugger, protocol hints), then let you choose from 4 options:

- **A. Recommended set** — derived from project analysis, typically 5-7 skills
- **B. Install all** — 24 skills, suitable for global tooling
- **C. Category-by-category** — 6 categories asked in sequence
- **D. Custom** — enter skill names directly

Installation only runs after your choice, avoiding project directory pollution. See "AI Assistant Install Protocol" above.

## npx Install (Recommended)

Requires [Node.js](https://nodejs.org/) 14+. Uses the [skills CLI](https://github.com/vercel-labs/skills) for one-command management, supporting Claude Code, Cursor, Codex, and 50+ AI coding assistants.

### Install All Skills

```bash
npx skills add LeoKemp223/embed-ai-tool -g -y
```

### Install Specific Skills

```bash
npx skills add LeoKemp223/embed-ai-tool --skill build-cmake --skill flash-openocd -g -y
```

### Manage

```bash
npx skills ls -g            # List installed
npx skills update -g        # Update
npx skills remove -g        # Remove
```

`-g` installs globally (`~/.claude/skills/`). Omit it to install to the current project (`.claude/skills/`).

## Script Installation

### Prerequisites

- Python 3.8+ (no third-party dependencies required)
- Git

### Step 1: Analyze Project Type (default behavior)

```bash
git clone https://github.com/LeoKemp223/embed-ai-tool.git
python3 embed-ai-tool/scripts/install.py /path/to/your-project
```

The script prints project characteristics (build system, debugger, protocols) and a recommended skill set. **Writes nothing to disk.**

### Step 2: Install Recommended Set

```bash
python3 embed-ai-tool/scripts/install.py /path/to/your-project --skills build-cmake flash-openocd debug-gdb-openocd serial-monitor workflow
```

### Or Install All (when you're sure you want all 24)

```bash
python3 embed-ai-tool/scripts/install.py /path/to/your-project --yes
```

> ⚠ `--yes` copies all 24 skills into the target project's `.claude/skills/`. Only recommended for global installs or tooling scenarios.

### Install Specific Skills

```bash
python3 embed-ai-tool/scripts/install.py /path/to/your-project --skills build-cmake flash-openocd serial-monitor
```

### Update Installed Skills

```bash
cd embed-ai-tool && git pull
python3 scripts/install.py /path/to/your-project --force
```

### Auto-Detect Tool Paths

Append `--detect` during installation to automatically scan PATH for embedded tools and write them to the workspace config:

```bash
python3 embed-ai-tool/scripts/install.py /path/to/your-project --detect
```

### Check Installation Status

```bash
python3 embed-ai-tool/scripts/install.py /path/to/your-project --status
```

### Uninstall

```bash
python3 embed-ai-tool/scripts/install.py /path/to/your-project --uninstall
```

### List Available Skills

```bash
python3 embed-ai-tool/scripts/install.py --list
```

### Manual Tool Path Configuration

Some skills depend on external tools (OpenOCD, Keil, arm-none-eabi-gcc, etc.). In addition to `--detect`, you can manually configure them:

```bash
# Set tool path (workspace level)
python3 scripts/em_config.py set openocd /usr/bin/openocd

# Set global tool path
python3 scripts/em_config.py set uv4 "C:\Keil_v5\UV4\UV4.exe" --global

# View configured tools
python3 scripts/em_config.py list

# View config file location
python3 scripts/em_config.py path
```

---

Back to [README_EN.md](README_EN.md) for the project introduction.
