[![English](https://img.shields.io/badge/lang-English-blue.svg)](README_EN.md)
[![简体中文](https://img.shields.io/badge/lang-%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87-red.svg)](README.md)

# Fully Automated AI-Driven Hardware Product Development, Debugging, and Testing

A skill set for AI coding assistants, providing LLMs with full-lifecycle MCU firmware development capabilities. Covers multi-toolchain builds (Keil / IAR / CMake / PlatformIO / ESP-IDF / Makefile), flashing (OpenOCD / J-Link / Keil, etc.), GDB debugging, RTOS thread-aware debugging, serial monitoring and shell interaction, Modbus / CAN / VISA protocol debugging, logic analyzer, memory and static analysis, and pipeline orchestration — supporting Linux, macOS, and Windows.

<img width="1922" height="1091" alt="image" src="https://github.com/user-attachments/assets/6b23bfb1-8755-4f28-b510-abb7cc80d18f" />

> **⚠ AI assistants**: when asked to install skills from this repo, you **must** first read the "AI Assistant Install Protocol" in [INSTALL_EN.md](INSTALL_EN.md) — analyze the project, present options, and wait for the user's explicit choice. **Never** bulk-copy the `skills/` directory.

## Quick Start

In any LLM chat that supports skills, enter:

```
Install skills from https://github.com/LeoKemp223/embed-ai-tool.git
```

The AI assistant will first analyze your project type (build system, debugger, protocol hints), then let you choose from a recommended set / install all / category-by-category / custom — installation only runs after your choice, avoiding project directory pollution.

For other install methods (npx / Python script / tool path configuration / update & uninstall), see [INSTALL_EN.md](INSTALL_EN.md).

## Skill List

24 skills in 6 categories:

| Category | Skill | Description |
|----------|-------|-------------|
| Build | `build-cmake` | Configure and build CMake-based MCU firmware projects |
| Build | `build-keil` | Configure and build Keil MDK firmware projects |
| Build | `build-iar` | Configure and build IAR EWARM firmware projects |
| Build | `build-platformio` | Configure and build PlatformIO firmware projects |
| Build | `build-idf` | Configure target chip and build ESP-IDF firmware projects |
| Build | `build-makefile` | Configure and build bare Makefile embedded projects |
| Flash | `flash-keil` | Flash firmware via Keil MDK built-in debugger |
| Flash | `flash-openocd` | Flash ELF/HEX/BIN artifacts via OpenOCD |
| Flash | `flash-platformio` | Flash firmware via PlatformIO upload mechanism |
| Flash | `flash-idf` | Flash firmware via ESP-IDF toolchain with JTAG debug support |
| Flash | `flash-jlink` | Flash firmware via SEGGER J-Link with RTT log capture |
| Debug | `debug-gdb-openocd` | Attach GDB via OpenOCD — supports post-flash debug, attach-only, and crash triage |
| Debug | `debug-jlink` | On-chip debugging and crash analysis via J-Link GDB Server |
| Debug | `debug-platformio` | Debug via PlatformIO built-in GDB |
| Debug | `rtos-debug` | FreeRTOS/RT-Thread/Zephyr thread-aware debugging, stack watermark, and deadlock detection |
| Comm | `serial-monitor` | Select serial port and capture runtime logs |
| Comm | `serial-shell` | Open an interactive shell session over serial; run single commands or batch scripts |
| Comm | `modbus-debug` | Modbus RTU/TCP register read/write, slave scanning, and continuous monitoring |
| Comm | `can-debug` | CAN bus frame monitoring, sending, and node scanning |
| Comm | `visa-debug` | VISA instrument SCPI communication, waveform capture, and screenshots |
| Comm | `logic-analyzer` | Capture digital waveforms with a Saleae logic analyzer and decode I2C/SPI/UART/CAN |
| Analysis | `memory-analysis` | Parse .map files or ELF to generate memory usage reports and symbol size rankings |
| Analysis | `static-analysis` | cppcheck/clang-tidy/GCC analyzer static analysis with MISRA-C compliance |
| Orchestration | `workflow` | Pipeline orchestration chaining multiple skills (build + flash + monitor/debug) |

## Usage Examples

After installing skills, trigger them with natural language or direct skill commands in your chat.

### Natural Language

```
👤 Build and flash
🤖 Detected Keil project Project.uvprojx, target chip STM32F103C8.
   ✅ Build succeeded, artifact: build/Project.axf (48.2 KB)
   Detected ST-Link V2, flashing...
   ✅ Flash succeeded, verified and reset.

👤 Check serial output
🤖 Detected COM3 (115200), capturing logs:
   [00:00.000] System Start
   [00:00.012] UART initialized

👤 Debug UART
🤖 Breakpoint set at HAL_UART_Transmit entry.
   Breakpoint hit: huart->Init.BaudRate = 9600, expected 115200.
   Suggest checking the baud rate setting in UART initialization code.

```

### Skill Commands

```bash
# Keil project: build + flash + serial
/build-keil
/flash-keil
/serial-monitor

# CMake project: build + flash + debug
/build-cmake
/flash-openocd
/debug-gdb-openocd

# ESP-IDF project: build + flash
/build-idf
/flash-idf

# One-click pipeline (build → flash → monitor)
/workflow
```

## Repository Structure

```text
.
├── skills/       # 24 skill modules (each with SKILL.md + scripts + references)
├── shared/       # Shared conventions (contracts / failure-taxonomy / platform-compatibility)
│                 # and reusable modules (project_detect.py / profile_store.py / tool_config.py)
├── templates/    # skill-template/ for new skills
└── scripts/
    ├── install.py         # Install / uninstall / status check
    ├── validate_repo.py   # Repository structure validation
    └── em_config.py       # Tool path config CLI
```

<img width="2955" height="1955" alt="PixPin_2026-04-26_22-31-41" src="https://github.com/user-attachments/assets/e62e3118-929e-494c-8d24-c9dcebec22c3" />

## Shared Conventions

All skills share a common set of core context for input and output:

- **Project Profile** — Standardized metadata for workspace, target, build system, debug probe, and artifacts
- **Skill Handoff Contract** — Context that downstream skills can directly inherit
- **Command Outcome Schema** — Unified format for success, failure, or blocked results
- **Failure Taxonomy** — Standard failure classification with recommended follow-up actions

See [shared/contracts.md](shared/contracts.md) and [shared/failure-taxonomy.md](shared/failure-taxonomy.md).

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) (includes skill structure requirements and the pre-commit validation flow). Use the [templates/skill-template/](templates/skill-template/) template when creating new skills.

## Future Extensions

The repository structure is designed for future expansion — for example, `flash-pyocd`, `vendor-tools`, `fault-triage`, `trace-analysis` — without changes to core conventions.


Thanks to the LinuxDo community for their support!
[![LinuxDo](https://img.shields.io/badge/LinuxDo-Community_Support-blue)](https://linux.do/)
