---
layout: default
title: "Subagents & Background Scheduling"
permalink: /docs/subagents-and-scheduling
---

[← Back to Home](../index.md) | [← Back to Surfaces](./surfaces.md)

# 🤖 Subagent Orchestration & Background Scheduling

One of Google Antigravity's most powerful architectural advantages is its ability to **delegate work asynchronously** across dedicated subagents and scheduled background tasks.

---

## 👥 Subagent Topologies

Rather than keeping everything in a single, congested context window, Antigravity can dynamically instantiate child subagents:

```mermaid
flowchart TD
    Parent[Parent Orchestrator Agent] --> ResearchAgent[Research Subagent\n- Read-only tools\n- Web & Codebase survey]
    Parent --> BranchAgent[Branch Subagent\n- Isolated Git Worktree\n- High-risk refactoring]
    Parent --> CustomAgent[Domain Subagent\n- Specialized prompt & tools]
    
    ResearchAgent -.->|Structured Report| Parent
    BranchAgent -.->|Diff & Test Results| Parent
    CustomAgent -.->|Artifacts| Parent
```

### Standard Subagent Types:
1. **`research`**: A focused subagent equipped with read-only tools and web search. It surveys documentation, searches large codebases, and synthesizes answers without bloating the primary agent's token context.
2. **`self`**: Inherits the parent agent's configuration and tools to explore parallel solutions or branched workspaces.
3. **Custom Defined Subagents**: Dynamically declared at runtime via `define_subagent` with bespoke system prompts, capabilities, and tool groups.

---

## ⏱️ Background Task Scheduling

Antigravity natively includes a scheduler engine for both **delayed timers** and **recurring cron jobs**:

```mermaid
sequenceDiagram
    participant User
    participant Agent as Antigravity Agent
    participant Sched as Background Scheduler
    
    User->>Agent: "Start integration build and check in 10 minutes"
    Agent->>Sched: schedule(DurationSeconds=600, Prompt="Check build status")
    Agent-->>User: "Timer set. Continuing other tasks..."
    Note over Sched: 10 minutes elapse...
    Sched-->>Agent: High-priority trigger notification
    Agent->>Agent: Inspects build logs
    Agent-->>User: "Build succeeded! Ready for deployment."
```

### Scheduling Modes:

#### 1. One-Shot Delayed Timers
- Set a timer with `DurationSeconds`.
- Configurable early-termination conditions (`never`, `any`, or a specific task ID).

#### 2. Recurring Cron Expressions
- Standard 5-field cron syntax (`*/15 * * * *` for every 15 minutes).
- Automatically triggers recurring codebase health checks, dependency vulnerability scans, or staging deployments.

---

## 💻 Programmatic Example (Tool Usage)

```json
{
  "name": "schedule",
  "args": {
    "CronExpression": "0 2 * * *",
    "Prompt": "Run full regression test suite and compile daily status report.",
    "MaxIterations": 30
  }
}
```

---

[← Back to Surfaces](./surfaces.md) | [Back to Home](../index.md)
