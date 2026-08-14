---
layout: default
title: "Frequently Asked Questions (FAQ)"
permalink: /docs/faq
---

[← Back to Home](../index.md)

# ❓ Frequently Asked Questions (FAQ)

Everything you need to know about Google Antigravity, security, compatibility, and best practices.

---

### Q1: What makes Google Antigravity different from standard AI coding tools?
**A:** Traditional coding tools operate primarily as passive autocompleters or single-turn chat dialogs that force you to manually copy and paste code. Antigravity is a **full-stack agentic runtime** that autonomously breaks down tasks into steps, writes code across multiple files, executes terminal builds and tests in a secure sandbox, self-corrects on errors, and can even spawn parallel subagents for background tasks.

---

### Q2: How does Antigravity protect my codebase and system?
**A:** Antigravity incorporates defense-in-depth security:
- **Terminal Sandboxing**: Commands can be executed in isolated sandbox environments.
- **Granular Execution Policies**: Choose between `always-proceed`, `request-review`, `strict`, or `proceed-in-sandbox`.
- **Scoped Permissions**: Explicit allow/deny rules for file paths outside your workspace and internet domain access.
- **Visual Diff Inspection**: Interactive side-by-side diff previews before changes are accepted.

---

### Q3: What is "Progressive Disclosure" and why does it matter?
**A:** When AI agents load dozens of large documentation manuals into their context window at once, token limits are exhausted and instruction adherence degrades. Antigravity uses **Progressive Disclosure**—it keeps only lightweight summaries in context and lazy-loads detailed skills and runbooks only when a specific task requires them.

---

### Q4: Can I use Antigravity with my team's custom tools and internal APIs?
**A:** Yes! Through the **Model Context Protocol (MCP)**, you can connect Antigravity to PostgreSQL databases, internal Jira/GitHub issue trackers, Slack bots, and custom corporate APIs with standardized JSON configurations.

---

### Q5: Can I run Antigravity in headless/CI environments?
**A:** Yes. The **Antigravity CLI (`agy`)** and the **Python SDK (`google-antigravity`)** can be scripted and executed directly within GitHub Actions, GitLab CI/CD, or automated test runners.

---

[← Back to Home](../index.md)
