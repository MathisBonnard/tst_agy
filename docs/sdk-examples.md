---
layout: default
title: "Python SDK Recipes & Code Examples"
permalink: /docs/sdk-examples
---

[← Back to Home](../index.md) | [← Back to Surfaces](./surfaces.md)

# 🐍 Antigravity Python SDK: Recipes & Cookbooks

The official Python SDK (`google-antigravity`) enables programmatic agent execution, tool wiring, event streaming, and custom multi-agent topologies.

---

## 📦 1. Installation & Environment

```bash
pip install google-antigravity
```

---

## ⚡ 2. Core Recipes

### Recipe A: Asynchronous Chat with Real-Time Thought Streaming

Stream the model's internal reasoning chain alongside the final text output:

```python
import asyncio
import sys
from google.antigravity import Agent, LocalAgentConfig, CapabilitiesConfig

async def stream_agent_thoughts():
    config = LocalAgentConfig(
        system_instructions="You are an expert algorithms engineer.",
        capabilities=CapabilitiesConfig(allow_read_tools=True),
    )

    async with Agent(config) as agent:
        response = await agent.chat("Explain the difference between A* and Dijkstra's algorithm.")

        # Stream internal reasoning steps
        async for thought in response.thoughts:
            print(f"🧠 [Reasoning] {thought}")

        print("\n💬 [Answer]:")
        # Stream user-facing tokens
        async for token in response:
            sys.stdout.write(token)
            sys.stdout.flush()
        print()

if __name__ == "__main__":
    asyncio.run(stream_agent_thoughts())
```

---

### Recipe B: Intercepting and Logging Tool Calls

Monitor strongly-typed tool invocations before and after they execute in the sandbox:

```python
import asyncio
from google.antigravity import Agent, LocalAgentConfig, CapabilitiesConfig

async def monitor_tool_executions():
    # Enable full write & command capabilities
    config = LocalAgentConfig(capabilities=CapabilitiesConfig(allow_all_tools=True))

    async with Agent(config) as agent:
        response = await agent.chat("Check disk usage and find the 3 largest files in the current folder.")

        async for tool_call in response.tool_calls:
            print(f"🔧 Executing: {tool_call.name}")
            print(f"   Arguments: {tool_call.args}")
            print(f"   Status: {tool_call.status}")

        async for token in response:
            print(token, end="", flush=True)

if __name__ == "__main__":
    asyncio.run(monitor_tool_executions())
```

---

### Recipe C: Interactive Command-Line Loop

Spin up a terminal REPL loop powered by Antigravity in just 5 lines:

```python
import asyncio
from google.antigravity import Agent, LocalAgentConfig, CapabilitiesConfig
from google.antigravity.utils.interactive import run_interactive_loop

async def start_repl():
    config = LocalAgentConfig(capabilities=CapabilitiesConfig())
    async with Agent(config) as agent:
        print("🚀 Starting Antigravity Interactive REPL (Press Ctrl+C to exit)...")
        await run_interactive_loop(agent)

if __name__ == "__main__":
    asyncio.run(start_repl())
```

---

### Recipe D: Programmatic Batch Code Audits

Process multiple repositories or directories in batch:

```python
import asyncio
from pathlib import Path
from google.antigravity import Agent, LocalAgentConfig

async def audit_file(agent: Agent, file_path: Path):
    content = file_path.read_text()
    prompt = f"Analyze this Python module for security risks and performance bottlenecks:\n\n```python\n{content}\n```"
    response = await agent.chat(prompt)
    
    output = []
    async for token in response:
        output.append(token)
    return "".join(output)

async def main():
    config = LocalAgentConfig()
    async with Agent(config) as agent:
        for py_file in Path("./src").glob("*.py"):
            print(f"Auditing {py_file}...")
            audit_result = await audit_file(agent, py_file)
            print(audit_result)

if __name__ == "__main__":
    asyncio.run(main())
```

---

[← Back to Surfaces](./surfaces.md) | [Back to Home](../index.md)
