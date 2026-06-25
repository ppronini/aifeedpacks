# AI FeedPacks

**Reusable Markdown bundles for AI systems.**

[![License: WTFPL](https://img.shields.io/badge/License-WTFPL-brightgreen.svg)](LICENSE)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](CONTRIBUTING.md)

---

FeedPacks are structured Markdown files containing context, instructions, workflows, examples, constraints, and quality checklists that help AI tools perform specific tasks consistently and effectively.

> **Feed better context. Get better output.**

## What is a FeedPack?

A **FeedPack** is **not** a prompt. It is a reusable bundle of knowledge that gives an AI system everything it needs to complete a task:

| Component | Description |
|---|---|
| 🎯 **Purpose** | What the FeedPack is for |
| 📋 **Instructions** | Role, behavior, rules for the AI |
| 🔄 **Workflow** | Step-by-step process |
| 📥 **Inputs** | Required information |
| 📤 **Output Format** | Expected output structure |
| 📐 **Examples** | Good and bad examples |
| 🚫 **Constraints** | Hard rules and limitations |
| ✅ **Checklist** | Quality validation |

## Available FeedPacks

| FeedPack | Category | Description |
|---|---|---|
| [PHP Best Practices](feedpacks/php-best-practices/) | Development | Modern PHP standards, PSR-12, security |
| [Laravel Code Review](feedpacks/laravel-code-review/) | Development | Laravel-specific code review guidelines |
| [API Design](feedpacks/api-design/) | Development | RESTful API design standards |
| [Bug Triage](feedpacks/bug-triage/) | Development | Structured bug analysis and prioritization |
| [OpenWork on Windows](feedpacks/openwork-windows/) | AI Agents | Complete setup guide for OpenWork AI agent on Windows 10/11 |
| [PRD Creator](feedpacks/prd-creator/) | Product | Product requirements document generation |
| [SEO Audit](feedpacks/seo-audit/) | Marketing | SEO analysis and recommendations |
| [Customer Interview](feedpacks/customer-interview/) | Research | Customer interview analysis framework |
| [Meeting Analysis](feedpacks/meeting-analysis/) | Business | Meeting notes summarization and action items |
| [Startup Landing Page](feedpacks/startup-landing-page/) | Marketing | Landing page copy and structure |
| [PHP on Windows (XAMPP)](feedpacks/php-xampp-windows/) | Development | Complete guide for XAMPP-based PHP dev on Windows with Linux deployment |

## How to Use

1. Browse the [FeedPacks](#available-feedpacks) above
2. Open the folder of the FeedPack you need
3. Feed the `.md` files into your AI tool:
   - **ChatGPT** / **Claude** — copy-paste or attach files
   - **Cursor** / **Windsurf** — add to project context
   - **GitHub Copilot** — reference in comments
   - **OpenWebUI** / **Local LLMs** — use as system prompt
   - **AI Agents** — load as instructions
4. Or download the whole repo:  
   `git clone https://github.com/ppronini/aifeedpacks.git`

## Categories

- **Development** — coding standards, code review, architecture, API design
- **Product** — PRD, user stories, prioritization
- **Marketing** — SEO, content, landing pages
- **Research** — competitive analysis, interviews, data analysis
- **Business** — SOPs, meetings, strategy
- **DevOps** — deployment, CI/CD, infrastructure
- **AI Agents** — setup, configuration, workflow automation

## Contribute

Contributions are welcome!

- **Submit a FeedPack** — add a new folder with the standard structure
- **Improve existing** — fix or enhance any FeedPack
- **Report issues** — open a GitHub issue

See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

## Philosophy

AI FeedPacks aims to become the standard repository for reusable AI operating knowledge.

Just as GitHub became the home of reusable source code, FeedPacks aims to become the home of reusable AI context, instructions, workflows, and expertise.

## License

**WTFPL — Do What The Fuck You Want To Public License**

All content in this repository — every FeedPack, every file, every commit — is released under the [WTFPL](LICENSE).

You are free to:
- Use it for any purpose, commercial or not
- Modify it however you want
- Redistribute it, with or without changes
- Sell it, give it away, burn it, frame it on your wall

**There are no restrictions.** No attribution required (though appreciated). No strings attached.

> All contributions to this repository are accepted under the same WTFPL terms.
