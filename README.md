# 🚀 Google Antigravity Showcase & Developer Journey

A modern, Markdown-first website designed for seamless publishing on **GitHub Pages**, introducing **Google Antigravity (AGY)** and showcasing real-world agentic workflows, projects, and achievements.

[![GitHub Pages Status](https://img.shields.io/badge/GitHub%20Pages-Ready%20to%20Publish-brightgreen?style=for-the-badge&logo=github)](https://pages.github.com)
[![Google Antigravity](https://img.shields.io/badge/Google-Antigravity-4285F4?style=for-the-badge&logo=google)](https://antigravity.google/docs)
[![Gemini](https://img.shields.io/badge/Gemini-Powered-8E75B2?style=for-the-badge)](https://deepmind.google/technologies/gemini)
[![Markdown](https://img.shields.io/badge/Built%20With-Markdown%20%26%20Jekyll-blue?style=for-the-badge&logo=markdown)](https://jekyllrb.com)

---

## 🌐 Live Website & Structure

This repository is configured out-of-the-box for **GitHub Pages**. All content is written in clean, standard GitHub Flavored Markdown (GFM):

- **🏠 [index.md](./index.md)**: Main landing page with ecosystem overview, key pillars, feature comparison, and navigation.
- **💡 [docs/overview.md](./docs/overview.md)**: Architecture deep-dive, Gemini foundation models, and security sandbox principles.
- **🛠️ [docs/surfaces.md](./docs/surfaces.md)**: Breakdown of Antigravity 2.0 Desktop, IDE, CLI (`agy`), and Python SDK.
- **🏆 [docs/my-journey.md](./docs/my-journey.md)**: Showcase of projects built, workflow automations, milestones, and developer learnings.
- **⚙️ [docs/customizations-guide.md](./docs/customizations-guide.md)**: Comprehensive guide on Skills, Rules (`AGENTS.md`), Plugins, and Model Context Protocol (MCP).
- **🐍 [docs/sdk-examples.md](./docs/sdk-examples.md)**: Python SDK code recipes (thought streaming, tool interception, interactive loops).
- **🔌 [docs/mcp-showcase.md](./docs/mcp-showcase.md)**: Model Context Protocol (MCP) integrations with GitHub, PostgreSQL, and filesystems.
- **🤖 [docs/subagents-and-scheduling.md](./docs/subagents-and-scheduling.md)**: Subagent orchestration topologies and background cron scheduling.
- **🏁 [docs/getting-started.md](./docs/getting-started.md)**: Quickstart guide covering installation, slash commands (`/plan`, `/learn`, `/schedule`), and shortcuts.
- **❓ [docs/faq.md](./docs/faq.md)**: Frequently asked questions and security considerations.

---

## 🚀 How to Publish on GitHub Pages (1-Minute Setup)

You can publish this website on GitHub in 3 simple steps:

### Step 1: Push to GitHub
```bash
git init
git add .
git commit -m "feat: complete Google Antigravity markdown showcase site"
git branch -M main
git remote add origin https://github.com/<YOUR-USERNAME>/<YOUR-REPO-NAME>.git
git push -u origin main
```

### Step 2: Enable GitHub Pages
1. Go to your repository on GitHub.
2. Click on **Settings** (⚙️) > **Pages** (in the left sidebar).
3. Under **Build and deployment**:
   - **Source**: Select either `Deploy from a branch` (Branch: `main` / `root`) OR select `GitHub Actions` (the repository includes an automated `.github/workflows/pages.yml` workflow ready to go).
4. Click **Save**.

### Step 3: View Your Live Website!
GitHub will automatically build and publish your Markdown site. Within seconds, your site will be live at:
`https://<YOUR-USERNAME>.github.io/<YOUR-REPO-NAME>/`

---

## ⚙️ Configuration & Customization

- **Site Metadata**: Edit `_config.yml` to customize the site title, description, repository URL, and theme.
- **Add Your Projects**: Edit `docs/my-journey.md` to add your specific projects, metrics, and screenshots.
- **Custom Styling**: Add custom CSS rules in `assets/css/style.scss`.

---

## 📜 License

MIT © [Your Name / Organization]
