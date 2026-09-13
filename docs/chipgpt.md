---
layout: default
title: "ChipGPT — 4-bit SoC in SystemVerilog"
permalink: /docs/chipgpt
---

[← Back to Home](../index.md)

# 🔧 ChipGPT — A 4-bit System-on-Chip in SystemVerilog

> **Prompt that started it all:** *"Create a chip in SystemVerilog, 4-bit, that can do UART communication."*

From this single sentence, Antigravity autonomously designed, implemented, verified, synthesized, and committed a **complete System-on-Chip** to GitHub — including CPU, peripherals, testbenches, firmware, assembler, CI/CD, and schematic export.

---

## 🏛️ Architecture

```mermaid
graph TD
    subgraph SoC ["chip_top.sv — 4-bit SoC"]
        CPU["cpu_4bit.sv\n4-bit RISC CPU Core\n30-instruction ISA"]
        ALU["alu_4bit.sv\n4-bit ALU\nZ · C · N · V flags"]
        REG["regfile_4bit.sv\nR0 – R3\nRegister File"]
        MEM["ram_sync.sv\n256B ROM + 128-nibble RAM"]
        UART["uart_controller.sv\nTX/RX · 16× oversampling\n8-bit byte MMIO mode"]
        GPIO["gpio.sv\n4-bit I/O\nDirection register"]
        TIMER["timer.sv\nAuto-reload\nIRQ enable"]
        BUS["bus_interconnect.sv\nAddress decoder\nMMIO map"]
    end
    CPU --> ALU
    CPU --> REG
    CPU --> BUS
    BUS --> MEM
    BUS --> UART
    BUS --> GPIO
    BUS --> TIMER
```

---

## 📋 Memory Map

| Address Range | Size | Peripheral |
|---|---|---|
| `0x00 – 0x7F` | 128 nibbles | Internal Scratchpad RAM |
| `0x80 – 0x88` | 9 nibbles | UART Controller (TX, RX, Status, Baud, Byte mode) |
| `0x90 – 0x92` | 3 nibbles | GPIO (Data out, Data in, Direction) |
| `0xA0 – 0xA4` | 5 nibbles | Timer (Counter, Control, Reload) |
| `0xF0` | 1 nibble | CPU Flags (V · N · C · Z) |

---

## 📐 Instruction Set (ISA)

The CPU executes a custom **30-instruction** ISA with 1- and 2-byte encodings:

| Category | Instructions |
|---|---|
| **Arithmetic** | `ADD`, `SUB`, `INC`, `DEC` |
| **Logic** | `AND`, `OR`, `XOR`, `NOT`, `SHL`, `SHR` |
| **Data Movement** | `MOV`, `LDI`, `LD`, `ST`, `LDR`, `STR` |
| **Control Flow** | `JMP`, `JZ`, `JNZ`, `JC`, `JNC`, `JN`, `JV`, `CALL`, `RET`, `HLT`, `NOP` |
| **UART** | `UART_TX`, `UART_RX`, `UART_ST` |

---

## ✅ Verification — 4 Self-Checking Testbenches

All testbenches run with **Icarus Verilog** and are fully self-checking (PASS/FAIL printed to stdout).

| Testbench | What's tested |
|---|---|
| `tb/tb_uart.sv` | 8-bit TX serialization, 16× oversampling RX, noise glitch rejection, framing errors, baud accuracy |
| `tb/tb_cpu.sv` | All 30 ISA instructions, flag updates, branch conditions, subroutine call/return |
| `tb/tb_chip_top.sv` | Full SoC boot sequence, UART banner output, full-duplex echo loop, GPIO updates |
| `tb/tb_math_demo.sv` | Fibonacci sequence computed in firmware running on the actual hardware |

```bash
make all      # build + run all 4 testbenches
make waves    # generate .vcd waveforms for GTKWave / Surfer
make lint     # Verilator static analysis
make synth    # Yosys synthesis
make schematics  # RTL + netlist schematics → .svg via Graphviz
```

---

## 🔬 Synthesis & Schematics

Antigravity debugged the Yosys synthesis pipeline (including a cross-platform fix for Debian/Raspberry Pi OS compatibility) and added automatic `.dot → .svg` conversion via Graphviz.

Four schematics are generated:

| File | Content |
|---|---|
| `synth/schematic_alu.svg` | 4-bit ALU — arithmetic, logic, flag generation |
| `synth/schematic_cpu.svg` | CPU datapath, register file, control unit |
| `synth/schematic_elaborated.svg` | Full SoC elaborated RTL block diagram |
| `synth/schematic_netlist.svg` | Synthesized gate-level netlist (`$_AND_`, `$_DFF_`, `$_MUX_`…) |

---

## 🛠️ Python Assembler

A custom **Python assembler** (`tools/asm.py`) converts `.asm` source files into `.hex` machine code for the boot ROM. Two firmware programs are included:

- **`firmware/boot_rom.asm`** — Boot banner printed over UART + interactive full-duplex echo loop
- **`firmware/math_demo.asm`** — Fibonacci sequence computed and streamed over UART

---

## 🔄 Cross-Platform Fixes

During development, the project was tested on both macOS and a **Raspberry Pi** running Debian. Antigravity identified and fixed two compatibility issues:

1. **Icarus Verilog 11.x (Debian)** — SystemVerilog array initializer syntax not supported → converted to compatible `for`-loop initialization
2. **Yosys on Debian** — `import chip_pkg::*` not recognized → replaced package imports with guarded `\`include` headers

---

## 🚀 CI/CD — GitHub Actions

A `.github/workflows/ci.yml` workflow automatically runs on every push:
- Assemble firmware
- Run all 4 testbenches
- Verilator lint check
- Yosys synthesis

---

## 📂 Repository

> **[github.com/MathisBonnard/chipGPT](https://github.com/MathisBonnard/chipGPT)**

---

[← Back to Home](../index.md) | [Next: Raspberry Pi SysAdmin →](./rpi-sysadmin.md)
