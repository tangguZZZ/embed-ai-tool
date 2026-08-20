[![English](https://img.shields.io/badge/lang-English-blue.svg)](INSTALL_EN.md)
[![简体中文](https://img.shields.io/badge/lang-%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87-red.svg)](INSTALL.md)

# 安装指南

## ⚠ 给 AI 助手的安装协议（必读）

> 当用户请求"安装 embed-ai-tool 的 skill"时，AI 助手（Claude Code / Cursor / Codex 等）**必须**遵循以下流程，**严禁**直接全量复制。

### 三条禁令

1. **不要**未经用户确认直接全量运行 `install.py` — 新版默认只分析不安装，需 `--skills X Y` 或 `--yes` 才会写入文件
2. **不要**用 `cp -r` / `xcopy` / `Copy-Item` / 文件管理器全量复制 `skills/` 目录到目标工程 — 这会污染 `.claude/skills/`
3. **不要**绕过 `install.py` 用其他工具（PowerShell、FileExplorer、Robocopy 等）做等效全量复制

### 必须执行的流程

1. **分析工程** — 在目标工程下运行 `python3 scripts/install.py /path/to/project`（不带安装参数），脚本输出工程类型探测结果 + 推荐集，**不会写入任何文件**
2. **呈现给用户** — 把分析结果 + 4 个选项展示给用户：
   - **A. 推荐集**（基于工程分析，通常 5-7 个）
   - **B. 全部安装**（24 个，`--yes`）
   - **C. 按分类逐一勾选**（6 个分类顺序询问）
   - **D. 自定义**（直接输入 skill 名）
3. **等待用户选择** — 用户明确选择后，再用 `--skills X Y` 或 `--yes` 执行实际安装
4. **汇报结果** — 安装完成后列出已装的 skill 清单

### 为什么

24 个 skill 涵盖 Keil / IAR / ESP-IDF / PlatformIO / CMake / Makefile 等多种工具链，单个工程通常只用其中 4-7 个。全量安装会污染 `.claude/skills/` 目录、增加 Claude skill 列表的噪音、影响工程协作者。

---

## 一键安装

在任意支持 skill 的大模型对话中输入：

```
帮我安装 https://github.com/LeoKemp223/embed-ai-tool.git 的 skill
```

AI 助手会**先分析你的工程类型**（构建系统、调试器、协议线索），然后让你从 4 个方案里选：

- **A. 推荐集** — 基于工程分析得出，通常 5-7 个
- **B. 全部安装** — 24 个，适合全局工具人
- **C. 按分类逐一勾选** — 6 个分类顺序询问
- **D. 自定义** — 直接输入 skill 名

你选完后才会执行安装，避免污染工程目录。详见上方的"给 AI 助手的安装协议"。

## npx 安装（推荐）

需要 [Node.js](https://nodejs.org/) 14+。使用 [skills CLI](https://github.com/vercel-labs/skills) 一键管理，支持 Claude Code、Cursor、Codex 等 50+ AI 编码助手。

### 安装全部 skill

```bash
npx skills add LeoKemp223/embed-ai-tool -g -y
```

### 安装指定 skill

```bash
npx skills add LeoKemp223/embed-ai-tool --skill build-cmake --skill flash-openocd -g -y
```

### 管理

```bash
npx skills ls -g            # 查看已安装
npx skills update -g        # 更新
npx skills remove -g        # 移除
```

`-g` 表示全局安装（`~/.claude/skills/`），去掉则安装到当前项目（`.claude/skills/`）。

## 脚本安装

### 前置条件

- Python 3.8+（无需第三方依赖）
- Git

### 第一步：分析工程类型（默认行为）

```bash
git clone https://github.com/LeoKemp223/embed-ai-tool.git
python3 embed-ai-tool/scripts/install.py /path/to/your-project
```

脚本会输出工程特征（构建系统、调试器、协议）和推荐安装的 skill 集，**不会写入任何文件**。

### 第二步：按推荐集安装

```bash
python3 embed-ai-tool/scripts/install.py /path/to/your-project --skills build-cmake flash-openocd debug-gdb-openocd serial-monitor workflow
```

### 或全量安装（确认要装全部 24 个时）

```bash
python3 embed-ai-tool/scripts/install.py /path/to/your-project --yes
```

> ⚠ `--yes` 会复制全部 24 个 skill 到目标工程的 `.claude/skills/`，仅推荐用于全局安装或工具人场景。

### 安装指定 skill

```bash
python3 embed-ai-tool/scripts/install.py /path/to/your-project --skills build-cmake flash-openocd serial-monitor
```

### 更新已安装的 skill

```bash
cd embed-ai-tool && git pull
python3 scripts/install.py /path/to/your-project --force
```

### 自动探测工具路径

安装时附加 `--detect`，自动扫描 PATH 中的嵌入式工具并写入工作区配置：

```bash
python3 embed-ai-tool/scripts/install.py /path/to/your-project --detect
```

### 查看安装状态

```bash
python3 embed-ai-tool/scripts/install.py /path/to/your-project --status
```

### 卸载

```bash
python3 embed-ai-tool/scripts/install.py /path/to/your-project --uninstall
```

### 列出可用 skill

```bash
python3 embed-ai-tool/scripts/install.py --list
```

### 手动工具路径配置

部分 skill 依赖外部工具（OpenOCD、Keil、arm-none-eabi-gcc 等），除 `--detect` 外也可手动配置：

```bash
# 设置工具路径（工作区级别）
python3 scripts/em_config.py set openocd /usr/bin/openocd

# 设置全局工具路径
python3 scripts/em_config.py set uv4 "C:\Keil_v5\UV4\UV4.exe" --global

# 查看已配置的工具
python3 scripts/em_config.py list

# 查看配置文件位置
python3 scripts/em_config.py path
```

---

返回 [README.md](README.md) 查看项目介绍。
