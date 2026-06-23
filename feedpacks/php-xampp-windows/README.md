# PHP on Windows (XAMPP) FeedPack

**Category:** Development  
**Version:** 1.0.0  
**Description:** A complete guide for developing PHP applications on Windows with XAMPP, based on real production experience. Covers setup, project structure, cross-platform deployment, security, and common Windows-specific issues.

## Purpose

Provide AI with battle-tested knowledge for PHP development on Windows that deploys to Linux servers. This FeedPack captures patterns from a production PHP application built entirely on XAMPP and deployed to production Linux hosting.

## Why This FeedPack Exists

Windows + XAMPP is the most accessible PHP dev environment, but it comes with unique challenges:

- Case-insensitive filesystem (Windows) vs case-sensitive (Linux)
- Path separators: `\` vs `/`
- Line endings: CRLF vs LF
- File permissions model differences
- Deployment friction

This FeedPack solves those issues so AI can generate Windows-compatible code that deploys to Linux without surprises.

## Intended Audience

- PHP developers working on Windows
- Teams using XAMPP for local development
- Developers deploying from Windows to Linux hosting
- AI-assisted PHP development on Windows

## Usage

1. Add this FeedPack to your AI project context
2. Reference `instructions.md` as the primary system prompt
3. Use `workflow.md` for the development workflow
4. Refer to specific files for deep dives on each topic
