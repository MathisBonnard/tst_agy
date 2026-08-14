---
layout: default
title: "Model Context Protocol (MCP) Showcase"
permalink: /docs/mcp-showcase
---

[← Back to Home](../index.md) | [← Back to Customizations](./customizations-guide.md)

# 🔌 Model Context Protocol (MCP) in Antigravity

The **Model Context Protocol (MCP)** is an open standard that allows Antigravity agents to securely connect to external data sources, databases, developer tools, and cloud platforms.

---

## 🏗️ How MCP Works in Antigravity

```mermaid
flowchart LR
    AGY[Google Antigravity Agent] <--> MCP[MCP Protocol Client]
    
    subgraph Servers[External MCP Servers]
        MCP <--> DB[(PostgreSQL / SQLite)]
        MCP <--> GH[GitHub / GitLab API]
        MCP <--> FS[Local / Remote File Storage]
        MCP <--> CLOUD[Cloud Monitoring & Logs]
    end
```

Rather than building brittle one-off connectors, MCP standardizes how tools and resources are exposed to the LLM runtime.

---

## ⚙️ Configuration Setup

MCP servers are configured in `mcp_config.json` inside your project's `.agents/` folder or globally in `~/.gemini/config/mcp_config.json`:

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "${GITHUB_TOKEN}"
      }
    },
    "postgres": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-postgres", "postgresql://user:pass@localhost:5432/production_db"]
    },
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "/Users/developer/secure_datasets"]
    }
  }
}
```

---

## 🎯 Practical Use Cases

### 1. 🐙 GitHub PR Audits & Automated Code Reviews
With the GitHub MCP server active, you can prompt Antigravity:
> *"Review open PR #142, run the diff through our security checklist, and post a review summary comment directly on GitHub."*

### 2. 🗄️ Database Schema Introspection & Query Optimization
With the Postgres MCP server connected:
> *"Inspect the indexes on our `orders` and `users` tables, identify slow JOIN queries, and generate a migration script to add missing composite indexes."*

### 3. 📊 Live System Metrics & Sentry Logs
Connect directly to application observability tools to debug production errors:
> *"Fetch the top 5 unresolved error traces from Sentry from the last 2 hours and locate the corresponding code in our repository."*

---

[← Back to Customizations](./customizations-guide.md) | [Back to Home](../index.md)
