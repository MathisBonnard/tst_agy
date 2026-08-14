---
layout: default
title: "Antigravity Surfaces & Tools"
permalink: /docs/surfaces
---

[← Back to Home](../index.md) | [← Back to Overview](./overview.md)

# 🛠️ Antigravity Surfaces & Ecosystem

Google Antigravity is delivered across four distinct, interoperable surfaces, tailored for different developer workflows and environments.

---

## 1. 🖥️ Antigravity 2.0 (Desktop App)

**Antigravity 2.0** is a standalone desktop application built on Electron, serving as the central orchestration cockpit for agents running independently of any single editor.

```
+-----------------------------------------------------------------------------------+
|  [Left Sidebar]        |  [Chat Canvas / Agent Feed]       | [Auxiliary Pane]     |
|  - New Conversation    |  - Natural language instructions  | - Subagents          |
|  - Projects Switcher   |  - Slash Commands (/plan, etc.)   | - Background Tasks   |
|  - Scheduled Tasks     |  - Interactive code diffs         | - Artifacts & Logs   |
|  - Skills & MCPs       |  - Image & asset drag-and-drop    | - File Changes       |
|  - Settings            |  - Human-in-the-loop review       | - Live Terminals     |
+-----------------------------------------------------------------------------------+
```

### Key Capabilities:
- **Auxiliary Pane**: Dedicated tabs to monitor Subagents, Background Tasks, Artifacts, File Changes, and Terminals.
- **Media & File Drop**: Drag-and-drop mockups, screenshots, or data files directly into the prompt canvas.
- **Scheduled & Recurring Tasks**: Define cron expressions (e.g., nightly test suites) or delayed execution timers.
- **Global & Project Settings**: Override permissions, sandbox policies, and model selections per repository.

---

## 2. 💻 Antigravity IDE (AI-First Editor)

Built on top of a streamlined VS Code core, **Antigravity IDE** integrates agent intelligence directly into the code canvas.

- **Antigravity Tab**: Context-aware autocomplete and "Supercomplete" that predicts code insertions, deletions, and multi-file jumps with a single <kbd>Tab</kbd> keypress.
- **Inline Command Lens (<kbd>⌘</kbd>+<kbd>I</kbd>)**: Highlight any function or block to trigger targeted refactors, unit test generation, or documentation.
- **Code Lenses & Action Buttons**: Clickable lenses hovering above classes and functions for instant fixes and explanations.
- **Visual Diff Overlays**: Side-by-side red/green diff previews with one-click acceptance or rejection.
- **Diagnostic Auto-Fix**: Automatically resolves compiler errors and lint warnings straight from the Problems panel.

---

## 3. ⚡ Antigravity CLI (`agy`)

The lightweight, lightning-fast terminal surface designed for keyboard-driven workflows and remote server environments.

```bash
# Launch the Antigravity CLI
agy

# Inspect all flags and command-line options
agy --help

# Run non-interactive agent script
agy --prompt "Scaffold a new markdown docs site"
```

### Top CLI Features:
- **TUI Interface**: High-density terminal dashboard with full keyboard navigation.
- **Slash Commands**: Instant workflow shortcuts (`/help`, `/plan`, `/schedule`, `/exit`).
- **Piped Output**: Seamlessly pipes terminal logs and command outputs into the agent context.
- **Configuration**: Local configuration managed under `~/.gemini/antigravity-cli/settings.json`.

---

## 4. 🐍 Antigravity Python SDK (`google-antigravity`)

The official Python SDK enables programmatic agent leasing, multi-agent orchestration, and custom agent toolchains.

### Installation:
```bash
pip install google-antigravity
```

### Async Agent Lifecycle Example:
```python
import asyncio
import sys
from google.antigravity import Agent, LocalAgentConfig, CapabilitiesConfig

async def main():
    # Configure agent with full write/command capabilities
    config = LocalAgentConfig(
        system_instructions="You are an expert full-stack developer.",
        capabilities=CapabilitiesConfig(),
    )

    # Spawn agent inside an async context manager
    async with Agent(config) as agent:
        response = await agent.chat("Analyze repository dependencies and generate report.")
        
        # Real-time token streaming
        async for token in response:
            sys.stdout.write(token)
            sys.stdout.flush()
            
        # Stream thoughts and tool calls in real time
        async for thought in response.thoughts:
            print(f"[Thinking] {thought}")
            
        async for tool_call in response.tool_calls:
            print(f"[Tool Execution] {tool_call.name}: {tool_call.args}")

if __name__ == "__main__":
    asyncio.run(main())
```

---

[← Back to Overview](./overview.md) | [Next: What I've Done So Far →](./my-journey.md)
