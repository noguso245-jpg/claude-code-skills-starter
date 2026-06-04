# Claude Code Skills — Starter Pack 🛠️

> Battle-tested skills to make Claude Code reliable, fast, and safe — distilled from cross-source research (YouTube, GitHub, X, engineering blogs).

**What you get here — all free, CC BY 4.0:**
- ✅ **4 self-contained skills** that form one complete dev loop (plan → implement → commit → govern)
- ✅ A fill-in-the-blanks **`CLAUDE.md` template** + sample **pre-commit hook**
- ✅ A **5-minute quickstart** that gets one skill working in your project today

*If this saved you time, a ⭐ helps other people find it.*

![License: CC BY 4.0](https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg)
![Free skills](https://img.shields.io/badge/Free%20skills-4-brightgreen)
![PRs welcome](https://img.shields.io/badge/PRs-welcome-orange)

🇯🇵 日本語版: [README.ja.md](./README.ja.md)

---

## Why this exists

Most people use Claude Code like a faster autocomplete — and wonder why their productivity didn't change.
The difference isn't the model. It's the **workflow**: how you plan, how you structure prompts, how you commit, and how you configure `CLAUDE.md`.

This repo gives you **4 free, ready-to-use skills** that form one complete loop of professional Claude Code development. Copy them into your project, follow the steps, and feel the difference today. No crippled demos — each skill is genuinely useful on its own.

---

## The 4 free skills (one complete loop)

| Skill | Level | What it gives you |
|---|---|---|
| [PIV Development Loop](./skills/en/piv-development-loop.md) | Beginner | Split every task into Plan → Implement → Verify with a human approval gate. The single highest-leverage pattern: "if the plan is good, the code is good." |
| [CLAUDE.md Architecture](./skills/en/claude-md-architecture.md) | Intermediate | WHAT/WHY/HOW framework + scope cascade. Change *only the config* and watch answer quality jump. |
| [AI Commit Strategy](./skills/en/ai-commit-strategy.md) | Beginner | 1 task = 1 commit (save points for your AI session). Conventional Commits + diff-injection so you can always roll back. |
| [Agile Prompt Template](./skills/en/agile-prompt-template.md) | Beginner | Context / To-dos / Not-to-dos / Acceptance Criteria. Stop getting vague output — give Claude a ticket, not a wish. |

> 🇯🇵 Japanese versions live in [`skills/ja/`](./skills/ja/).

**How they fit together:**
`Structure the prompt (4)` → `Plan, implement, verify (1)` → `Commit safely as you go (3)` → `Govern it all with CLAUDE.md (2)`.

---

## Quick start — feel one skill working in 5 minutes

```bash
# 1. Clone the repo
git clone https://github.com/noguso245-jpg/claude-code-skills-starter

# 2. Drop one skill into your project (PIV is the best first one)
mkdir -p your-project/.claude/skills
cp claude-code-skills-starter/skills/en/piv-development-loop.md your-project/.claude/skills/

# 3. Launch Claude Code in your project
cd your-project
claude
```

Then paste this into Claude Code:

```
Read .claude/skills/piv-development-loop.md and use it for the next task.
First, give me a PLAN only for: <your task here>. Wait for my approval before coding.
```

You'll get a reviewable plan before any code is written — that approval gate is the whole point. That's the difference between "autocomplete" and a real workflow. Full walkthrough: [`docs/quickstart.md`](./docs/quickstart.md).

---

## Also included

Beyond the 4 skills, this repo ships a few ready-to-use starters:

| File | What it is |
|---|---|
| [`templates/CLAUDE.md`](./templates/CLAUDE.md) | A fill-in-the-blanks `CLAUDE.md` for a web app — role, coding standards, workflow rules, and a "what NOT to do" section. |
| [`hooks/pre-commit.md`](./hooks/pre-commit.md) | Sample Claude Code Hooks (lint-on-commit, `.env` write-block) you can paste into `.claude/settings.json`. |
| [`docs/quickstart.md`](./docs/quickstart.md) | A short setup guide that ties the template and skills together. |

All free, CC BY 4.0 — copy and adapt freely.

---

## ⭐ Star / 🔭 Watch

These 4 skills are a curated starter slice of a larger set I'm building, and new free skills are added periodically.

- **⭐ Star** — if this saved you time, a star helps other people find it.
- **🔭 Watch** — to actually get notified when new skills are added (Watch → Custom → Releases/Activity is GitHub's notification feature; Star alone does not send notifications).

Have a skill you wish existed? [Open an issue](../../issues/new).

---

*Built solo + AI.*
