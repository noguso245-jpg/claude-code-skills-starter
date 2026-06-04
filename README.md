# Claude Code Skills — Starter Pack 🛠️

> Battle-tested skills to make Claude Code reliable, fast, and safe — distilled from cross-source research (YouTube, GitHub, X, engineering blogs).

![License: CC BY 4.0](https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg)
![Free skills](https://img.shields.io/badge/Free%20skills-4-brightgreen)
![Full library](https://img.shields.io/badge/Full%20library-79%20skills-blue)
![PRs welcome](https://img.shields.io/badge/PRs-welcome-orange)

🇯🇵 日本語版: [README.ja.md](./README.ja.md)

---

## Why this exists

Most people use Claude Code like a faster autocomplete — and wonder why their productivity didn't change.
The difference isn't the model. It's the **workflow**: how you plan, how you structure prompts, how you commit, and how you configure `CLAUDE.md`.

This repo gives you **4 free, ready-to-use skills** that form one complete loop of professional Claude Code development. Copy them into your project, follow the steps, and feel the difference today.

> These 4 are a curated starter slice of a larger **79-skill library**. Everything here is genuinely useful on its own — no crippled demos.

---

## The 4 free skills (one complete loop)

| Skill | Level | What it gives you |
|---|---|---|
| [PIV Development Loop](./skills/en/piv-development-loop.md) | Beginner | Split every task into Plan → Implement → Verify with a human approval gate. The single highest-leverage pattern: "if the plan is good, the code is good." |
| [CLAUDE.md Architecture](./skills/en/claude-md-architecture.md) | Intermediate | WHAT/WHY/HOW framework + scope cascade. Change *only the config* and watch answer quality jump. |
| [AI Commit Strategy](./skills/en/ai-commit-strategy.md) | Beginner | 1 task = 1 commit (save points for your AI session). Conventional Commits + diff-injection so you can always roll back. |
| [Agile Prompt Template](./skills/en/agile-prompt-template.md) | Beginner | Context / To-dos / Not-to-dos / Acceptance Criteria. Stop getting vague output — give Claude a ticket, not a wish. |

> 🇯🇵 Original Japanese versions live in [`skills/ja/`](./skills/ja/).

**How they fit together:**
`Structure the prompt (4)` → `Plan, implement, verify (1)` → `Commit safely as you go (3)` → `Govern it all with CLAUDE.md (2)`.

---

## Quick start (2 minutes)

```bash
# 1. Clone
git clone https://github.com/noguso245-jpg/claude-code-skills-starter

# 2. Read a skill, then drop it into your project's .claude/skills/ (or just keep it open as a reference)
cp claude-code-skills-starter/skills/en/piv-development-loop.md your-project/.claude/skills/

# 3. Launch Claude Code and apply the workflow
claude
```

No setup, no config hunting. Each skill is a self-contained markdown playbook.

---

## Also included

Beyond the 4 skills, this repo ships a few ready-to-use starters:

| File | What it is |
|---|---|
| [`templates/CLAUDE.md`](./templates/CLAUDE.md) | A fill-in-the-blanks `CLAUDE.md` for a web app — role, coding standards, workflow rules, and a "what NOT to do" section. |
| [`hooks/pre-commit.md`](./hooks/pre-commit.md) | Sample Claude Code Hooks (lint-on-commit, `.env` write-block) you can paste into `.claude/settings.json`. |
| [`docs/quickstart.md`](./docs/quickstart.md) | A 30-minute setup guide that ties the template and skills together. |

All free, CC BY 4.0 — copy and adapt freely.

---

## The full library (79 skills)

These 4 are the entrance. The full library covers the parts that are hard to get right — parallelism, autonomy, MCP, and team scale.

<details>
<summary><b>Click to see all 79 skills (titles only)</b></summary>

**Workflow & context**
- Agentic Workflow Pattern Selection · PIV Development Loop ✅free · Workspace Initialization · Context Management Strategy · Complexity-Based Routing · Session Scope Discipline

**CLAUDE.md & memory**
- CLAUDE.md Architecture ✅free · CLAUDE.md Curation Protocol · Path-Scoped Rules · Org Memory Architecture · Claude Code Auto-Memory

**Automation & hooks**
- Hook System Architecture · Advanced Hook Patterns · Conditional Hooks · Hooks Exit-Code Mastery · Headless Pipeline Automation

**Multi-agent**
- Subagent Delegation · Director/Worker/QA · Multi-Agent Observability · Agent Communication Protocol · Durable Agent Workflow · Agent Loop Guard · Agent Contract Design · Agent Teams Coordination · Fork Subagent · Multi-Agent Local Coordination · Software Factory Architecture

**MCP (Model Context Protocol)**
- MCP Integration Patterns · MCP Security Hardening · Custom MCP Server Build · MCP Tool Budget · MCP Orchestration Patterns · MCP Server Stack · MCP Tool-Search Dynamic Loading

**Quality, testing & debugging**
- TDD × AI Hybrid · Writer/Reviewer Dual Session · Diagnose-Only Debugging · Vibe-Coding Escape Protocol · AI Code Security Audit · 9-Agent Parallel Code Review · Property-Based Testing · AgentRx Failure Diagnosis · Circuit-Breaker Error Recovery · SDLC Feedback Loop

**Cost, autonomy & ops**
- Cost Optimization & Model Tiering · Token Optimization 3-Layer · Permission & Security Model · Auto Mode Permission Design · Graduated Autonomy · Autonomous Loop Design · Cloud Routine Patterns · Context-Rot Prevention · ContextOps for Teams

**Commit, spec & DevOps**
- AI Commit Strategy ✅free · Agile Prompt Template ✅free · Spec-Driven Development · Human-in-the-Loop Approval Gates · Terraform Safe 4-Phase · DB Migration Safe Deploy · Project Wiki Auto-Maintenance · Code Archaeology Workflow

*(…and more — 79 skills + 3 workflows + 10 reusable prompts in total.)*

</details>

---

## Get the full pack

If the 4 free skills helped, the full library removes the guesswork from the hard parts.

| Product | What's inside | Price | Link |
|---|---|---|---|
| **Starter Pack** | 7 CLAUDE.md templates · Hooks · MCP config — the fastest first setup | **$14** | [Gumroad (EN)](https://streamsolty.gumroad.com/l/wrtgun) |
| **Workflow OS** | The full **79-skill** library + 3 workflows + 10 prompts | **$65** | [Gumroad (EN)](https://streamsolty.gumroad.com/l/guuhox) |

🇯🇵 日本語の方は [BOOTH](https://streamsolty.booth.pm/) または Gumroad（[Starter ¥1,980](https://streamsolty.gumroad.com/l/gliwz) / [Workflow OS ¥9,800](https://streamsolty.gumroad.com/l/vhcysn)）からどうぞ。

7-day money-back guarantee. Try first, decide later.

---

## ⭐ Star this repo

New free skills are added periodically. Star/watch to get notified.
Have a skill you wish existed? [Open an issue](../../issues/new).

---

*Built solo + AI.*
