---
layout: default
title: "Getting Started with Antigravity"
permalink: /docs/getting-started
---

[← Back to Home](../index.md) | [← Back to Customizations](./customizations-guide.md)

# 🏁 Getting Started with Google Antigravity

This guide covers everything you need to set up Google Antigravity, configure your local environment, and execute your first agentic workflow.

---

## ⚡ 1. Fast Track Installation

### Antigravity CLI (`agy`)
To install the standalone CLI:
```bash
# Verify installation & authenticate
agy --version
agy
```

### Antigravity Python SDK
To integrate agents into Python scripts:
```bash
pip install google-antigravity
```

### Antigravity Desktop 2.0 & IDE
Download the installers directly from the official portal at [https://antigravity.google/docs](https://antigravity.google/docs).

---

## ⌨️ 2. Essential Slash Commands

When working in the Antigravity chat canvas or CLI, slash commands activate specialized behaviors:

| Slash Command | Purpose | Example Use Case |
| :--- | :--- | :--- |
| `/plan` | Generate and iterate on a detailed step-by-step roadmap before making code edits. | Planning a major database migration or framework upgrade. |
| `/grill-me` | Interactive requirement gathering and design interview. | Clarifying ambiguous edge-cases before starting UI design. |
| `/learn` | Persist developer corrections and learnings into project rules for the future. | Teaching the agent your team's specific test runner flags. |
| `/schedule` | Set up one-shot delayed timers or recurring cron tasks. | Running health checks or recurring lint tasks in the background. |
| `/help` | List all available slash commands and keybindings. | Discovering newly installed skills and features. |

---

## 🎛️ 3. Quick Keyboard Shortcuts

### In Antigravity IDE:
- <kbd>Tab</kbd>: Accept next-intent autocomplete or Supercomplete diff.
- <kbd>⌘</kbd>+<kbd>→</kbd> / <kbd>Ctrl</kbd>+<kbd>→</kbd>: Accept suggestion word-by-word.
- <kbd>⌘</kbd>+<kbd>I</kbd> / <kbd>Ctrl</kbd>+<kbd>I</kbd>: Open Inline Command Lens on highlighted code.
- <kbd>Esc</kbd>: Dismiss active inline prediction.

### In Antigravity CLI (`agy`):
- `Ctrl+D Ctrl+D`: Exit session cleanly.
- `Ctrl+C`: Interrupt current tool execution or thinking process.

---

## 🚀 4. Your First Multi-Step Workflow

Follow this quick walkthrough to experience the full power of Antigravity:

### Step 1: Initialize Project Rules
Create an `AGENTS.md` file in your repository root:
```markdown
# Project Guidelines
- We use TypeScript and ESM modules.
- Every utility function must include JSDoc comments.
- Tests are written in Vitest.
```

### Step 2: Launch Agent in Planning Mode
In the chat prompt:
```text
/plan I want to create a lightweight URL slug generator with unit tests.
```

### Step 3: Review and Execute
Review the generated plan, click **Proceed**, and watch Antigravity write the code, run the unit tests in the sandbox, and verify the results automatically!

---

[← Back to Customizations](./customizations-guide.md) | [Back to Home](../index.md)
