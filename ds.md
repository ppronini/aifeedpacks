# DeepSeek V4 Flash — Session Notes

**Model**: DeepSeek V4 Flash (`deepseek/deepseek-v4-flash`)
**App**: OpenWork desktop app
**Date**: 2026-06-21

## What I (DeepSeek) Can Do

- **File editing** — read, write, edit files in the workspace
- **Git/GitHub** — add, commit, push, create repos, change visibility (via `gh` CLI)
- **Browser** — open URLs, take screenshots, click/fill forms (built-in browser)
- **Web fetching** — fetch and summarize web content
- **Search code** — grep, glob for files and patterns
- **Run commands** — bash, npm, etc. in the workspace
- **Create artifacts** — Markdown, HTML, CSV, Excel, images via SVG
- **OpenWork Cloud API** — manage org members, plugins, configs, LLM providers, etc.

## Project: AI FeedPacks

**Path**: `/home/ikeen/projects/aifeedpacks`
**Repo**: `ppronini/aifeedpacks` (private on GitHub)
**Branch**: `master`
**Concept**: Reusable Markdown bundles (FeedPacks) for AI systems — context, instructions, workflows, examples. Not prompts.

### Files
| File | Description |
|------|-------------|
| `concept.md` | Full vision document |
| `index.html` | One-page dark-theme website |
| `logos/icon.svg` | SVG brand icon |
| `logo1.png`–`logo4.png` | Brand logos |
| `ds.md` | This file — session notes |

## Useful Git Commands
```bash
# Stage all and commit
git add . && git commit -m "message"

# Push
git push

# Create private repo and push
gh repo create ppronini/aifeedpacks --private --push --source .

# Change visibility
gh repo edit ppronini/aifeedpacks --visibility private
```

## Next Session — Quick Start
```bash
cd /home/ikeen/projects/aifeedpacks
# I'm ready to go
```
