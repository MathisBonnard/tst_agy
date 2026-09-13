# Mathis × Google Antigravity — What We Built Together

A GitHub Pages site showcasing real engineering projects built with [Google Antigravity](https://antigravity.google), an AI-first agentic coding assistant by Google DeepMind.

## 🌐 Live Site

> **[mathisbonnard.github.io/tst_agy2](https://mathisbonnard.github.io/tst_agy2)**

## 📂 Projects

### 🔧 [ChipGPT — 4-bit SoC in SystemVerilog](docs/chipgpt.md)
A complete System-on-Chip designed from a single sentence:
- 4-bit RISC CPU with a custom 30-instruction ISA
- UART controller, GPIO, Timer, Bus Interconnect
- 4 self-checking testbenches (Icarus Verilog)
- Yosys synthesis + Graphviz schematic export (RTL & gate-level netlist)
- Python assembler for the custom ISA
- GitHub Actions CI/CD

→ **[github.com/MathisBonnard/chipGPT](https://github.com/MathisBonnard/chipGPT)**

### 🍓 [Raspberry Pi RAID SysAdmin](docs/rpi-sysadmin.md)
Antigravity SSHed into a Raspberry Pi, diagnosed repeated RAID failure alerts caused by JMicron USB adapters running on the UAS driver, and applied a kernel-level fix via udev rules.

### 💡 [About Google Antigravity](docs/about-antigravity.md)
What Antigravity is, how agentic sessions work, and what makes it different from traditional AI assistants.

## 🏗️ Site Structure

```
.
├── index.md                      # Landing page
├── _config.yml                   # Jekyll / GitHub Pages config
├── docs/
│   ├── chipgpt.md               # ChipGPT project deep-dive
│   ├── rpi-sysadmin.md          # Raspberry Pi RAID debugging
│   └── about-antigravity.md     # What is Google Antigravity?
└── assets/css/                  # Custom styles
```

## 🚀 Publishing on GitHub Pages

1. Push this repo to GitHub
2. Go to **Settings → Pages → Source: Deploy from branch → `main` / `root`**
3. The site will be live at `https://<username>.github.io/<repo-name>`

---

*Built with Google Antigravity. Hosted on GitHub Pages.*
