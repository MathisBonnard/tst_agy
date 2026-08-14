---
layout: default
title: "Customizations, Skills & MCP"
permalink: /docs/customizations-guide
---

[← Back to Home](../index.md) | [← Back to Journey](./my-journey.md)

# ⚙️ Antigravity Customizations: Skills, Rules & MCP

The **Antigravity Customization System** enables developers and teams to teach agents specialized workflows, enforce repository guidelines, and connect directly to internal tools.

---

## 🧩 Customization Types At a Glance

| Type | File / Location | Scope | Best Used For |
| :--- | :--- | :--- | :--- |
| **Rules** | `GEMINI.md`, `AGENTS.md`, `.agents/rules/*.md` | Contextual / Hierarchical | Enforcing coding styles, architectural patterns, and security constraints. |
| **Skills** | `.agents/skills/<name>/SKILL.md` | On-Demand (Progressive) | Teaching multi-step procedures, runbooks, and tool workflows. |
| **Plugins** | `.agents/plugins/<name>/plugin.json` | Bundle | Packaging skills, rules, and MCP servers into shareable distributions. |
| **Hooks** | `hooks.json` | Lifecycle Events | Executing automated scripts pre/post tool execution. |
| **MCP Servers**| `mcp_config.json` | Tool Integration | Integrating custom external APIs, databases, and services via Model Context Protocol. |

---

## 🎯 1. Writing Project Rules (`AGENTS.md` / `GEMINI.md`)

Rules are placed at the root of a project or in subdirectories. As the agent navigates your project, it walks up the directory tree to discover applicable rules.

### Example `AGENTS.md`:
```markdown
# Repository Coding Standards

## Architecture & Frameworks
- Use vanilla CSS variables for styling. Avoid utility CSS libraries unless specified.
- Structure React components into `components/`, `hooks/`, and `utils/`.

## Testing Guidelines
- Always write comprehensive unit tests with Vitest or Jest.
- Run `npm test` before concluding a task.
```

---

## 🧠 2. Creating Custom Skills (`SKILL.md`)

Skills empower agents with procedural knowledge and step-by-step runbooks. Antigravity uses **Progressive Disclosure**: the full instructions are only loaded when triggered by task context or explicit invocation.

### Structure of a Skill:
```
my-project/
└── .agents/
    └── skills/
        └── deploy-preview/
            ├── SKILL.md          <-- Frontmatter + Instructions (Required)
            ├── scripts/          <-- Optional automation scripts
            └── references/       <-- Optional deep documentation
```

### Example `SKILL.md`:
```markdown
---
name: deploy-preview
description: Automatically builds the application and deploys a preview environment to staging.
---

# Deploy Preview Runbook

When the user asks to create a preview deployment:

1. **Verify Unit Tests**:
   ```bash
   npm run test:ci
   ```
2. **Build Distribution Bundle**:
   ```bash
   npm run build
   ```
3. **Deploy to Preview Bucket**:
   ```bash
   ./scripts/upload-preview.sh --env=staging
   ```
4. **Notify User**: Return the generated preview URL in a markdown alert.
```

---

## 🔌 3. Model Context Protocol (MCP) Integration

Antigravity natively implements the **Model Context Protocol (MCP)**, allowing agents to query databases, call GitHub APIs, interface with Jira, or communicate with local development servers.

### Configuration (`mcp_config.json`):
```json
{
  "mcpServers": {
    "postgres-db": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-postgres", "postgresql://localhost/mydb"]
    },
    "github-tools": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "${GITHUB_TOKEN}"
      }
    }
  }
}
```

---

## 🔄 4. Precedence & Priority Order

When multiple customizations are discovered, Antigravity resolves naming collisions using a strict hierarchy (highest to lowest priority):

1. **Workspace Project**: Hierarchical walk from current working directory to repository root (`.agents/`).
2. **Declared Project Configurations**: Explicitly listed in `skills.json` / `plugins.json`.
3. **Global Machine Discovery**: User directory `~/.gemini/config/`.
4. **Built-in System Customizations**: Default skills bundled with Antigravity.

---

[← Back to Journey](./my-journey.md) | [Next: Getting Started Guide →](./getting-started.md)
