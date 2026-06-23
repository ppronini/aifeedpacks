---
title: How to Get Better Output from AI Using Structured Markdown Files
tags: ai, chatgpt, productivity, opensource, markdown
published: false
---

If you use ChatGPT, Claude, or any AI agent for code reviews, API design, or writing PRDs, you've probably noticed something: **the quality of AI output depends almost entirely on the quality of your context.**

The problem is, every time you start a new task, you have to re-explain the rules, conventions, and expected format. It gets repetitive fast.

I've been solving this with something I call FeedPacks — reusable Markdown bundles that package everything an AI needs for a specific task.

## The Problem

When I asked Claude to review my PHP code, I'd write something like:

> Review this PHP code for best practices, PSR-12 compliance, security issues, and performance problems.

The result was okay, but inconsistent. Claude didn't know my exact standards, what to prioritize, or how to format the output.

## The Solution: Structured Context

Instead of a single prompt, I now use a set of Markdown files with a fixed structure:

```
task-name/
├── instructions.md    — AI role, behavior, rules
├── workflow.md        — step-by-step process
├── inputs.md          — what information to ask for
├── output-format.md   — expected output structure
├── examples.md        — good and bad examples
├── constraints.md     — hard limits
└── checklist.md       — quality validation
```

I feed these files into the AI session before starting the task. The difference is night and day.

## Example: PHP Code Review FeedPack

Just the `instructions.md` file alone gives the AI complete context:

> You are a senior PHP developer specializing in code review. Analyze code for PSR-12 compliance, security vulnerabilities, performance issues, and modern PHP 8.x practices. Be constructive. Prioritize by severity. Support each finding with code examples.

Paired with `output-format.md`, the AI knows exactly how to structure the response — severity rating, explanation, code example, fix suggestion. No more guessing.

## Why Not Just Save a Prompt?

A prompt is a one-shot instruction. A FeedPack is a **knowledge bundle**:

| Aspect | Single Prompt | FeedPack |
|--------|--------------|----------|
| Instructions | Yes | Yes |
| Workflow | Maybe | Yes |
| Inputs | No | Yes |
| Output format | Sometimes | Yes |
| Examples | Rarely | Yes |
| Constraints | No | Yes |
| Checklist | No | Yes |

## Real Results

Since I switched to this approach:
- Less back-and-forth with the AI
- More consistent output formatting
- Easier to reuse work across sessions
- Simple to share with teammates (it's just Markdown)

## Try It Yourself

I've published 10 FeedPacks covering PHP development, Laravel code review, API design, bug triage, SEO audits, PRD creation, and more.

All free, no restrictions: [https://github.com/ppronini/aifeedpacks](https://github.com/ppronini/aifeedpacks)

The format works with ChatGPT, Claude, Cursor, Copilot — any AI that accepts Markdown input.

## How to Create Your Own

1. Pick a task you do repeatedly with AI
2. Create a folder with the 7 .md files listed above
3. Fill each one with your specific context
4. Next time you need that task done, feed the files to the AI

Start simple. Even just `instructions.md` + `output-format.md` will improve your results dramatically.

---

*This approach has saved me hours of re-explaining context to AI tools. Hopefully it helps you too.*
