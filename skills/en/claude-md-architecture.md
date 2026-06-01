---
name: CLAUDE.md Architecture
description: Design CLAUDE.md as an onboarding document for your team's AI colleague. Use the WHAT/WHY/HOW framework, a 5-scope cascade, @imports modularization, and a Compound Engineering loop to build a knowledge base that improves itself across sessions.
tags: [CLAUDE.md, context, architecture, documentation, team]
difficulty: Intermediate
sources:
  - https://www.obviousworks.ch/en/designing-claude-md-right-the-2026-architecture-that-finally-makes-claude-code-work/
  - https://www.generative.inc/the-complete-claude-code-guide-2026-planning-context-engineering-and-high-leverage-development
---

# Purpose

CLAUDE.md is not a README for humans — it is an onboarding document for an AI teammate. Every time you correct Claude in a PR, you add that correction to CLAUDE.md, which automatically builds a "never-repeat-this-bug list" (Compound Engineering).

# When to use

- At the start of a new project (design it in the first 30 minutes)
- When CLAUDE.md grows past ~150 lines (time to refactor)
- When Claude keeps making the same mistake (add a rule)
- When a team needs a unified code of conduct for AI behavior

# When not to use

- One-off scripts or throwaway projects
- When you just want to "vaguely improve" a CLAUDE.md that already works (run `/init` to diagnose first)

# Inputs

- Project name, purpose, and tech stack (including versions)
- Conventions to follow and things to forbid
- Build, test, and deploy commands
- A list of mistakes Claude has made before

# Workflow

## Step 1 — Design the 5-scope cascade

| Scope | Path | Purpose | Git-tracked |
|---|---|---|---|
| Global | `~/.claude/CLAUDE.md` | Personal defaults (all projects) | No |
| Project root | `./CLAUDE.md` | Shared project rules | ✅ |
| Local secret | `./CLAUDE.local.md` | Personal notes / sensitive paths | ❌ .gitignore |
| Folder | `./src/CLAUDE.md` | Module-specific rules (lazy-loaded) | ✅ |
| Subagent | `./AGENTS.md` | For multi-agent use (cross-tool compatible) | ✅ |

**Last-wins rule:** a deeper scope overrides those above it.

## Step 2 — Write with the WHAT/WHY/HOW framework

```markdown
# [Project Name] CLAUDE.md

## WHAT (what we are building)
- Project: [one sentence]
- Stack: React 18.3 + TypeScript 5.4 + Vite 5 + Prisma 5.2
- Structure: src/components/, src/api/, src/utils/, tests/
- Critical file: src/middleware/auth.ts (auth — always read before changing)

## WHY (why we decided this way)
- camelCase for variables, PascalCase for React components
- MUST use TypeScript strict mode. MUST NOT use the `any` type
- NEVER commit to main directly. Always create a feature branch
- Conventional Commits: feat:, fix:, refactor:, docs:

## HOW (how to run it)
- Build: `npm run build`
- Test: `npm test` — after every code change
- Lint: `eslint . --fix` — before every commit
- PR: `gh pr create` when work is complete
```

## Step 3 — Apply the precision rule

**Compliance rate:** specific rules 89% vs. vague rules 35%.

| ❌ Ignored (vague) | ✅ Followed (specific) |
|---|---|
| "Write clean code" | "camelCase for variables, PascalCase for components" |
| "Test everything" | "Run `npm test` after every change. utils/ requires 80% coverage" |
| "Use TypeScript" | "strict mode required. The `any` type is forbidden" |
| "Be careful with git" | "New branch per task. Direct commits to main are forbidden" |

**Forcing vocabulary:** using `MUST` / `NEVER` / `ALWAYS` raises the compliance rate.

## Step 4 — Modularize with @imports

```markdown
# CLAUDE.md (root)
@.claude/rules/git-conventions.md
@.claude/rules/security-rules.md
@docs/architecture.md
```

- Distribute file size to keep the root slim
- Split rules into separate files so they're reusable across projects
- Put API-specific rules in `./src/CLAUDE.md` for lazy loading

## Step 5 — Set up the Compound Engineering loop

```
Find a Claude mistake in PR review
 ↓
Add that mistake to CLAUDE.md as a specific rule
 ↓
The same mistake never happens again
 ↓
After 1 month: common mistakes disappear
After 3 months: CLAUDE.md = your team's tacit knowledge, documented
```

**Rule:** when you find a mistake, don't get emotional — add one line to CLAUDE.md.

## Step 6 — Stay within the size guidelines

| Metric | Recommended | If exceeded |
|---|---|---|
| Lines | ≤200 | split with @imports |
| Tokens | ≤2,000 | remove low-priority rules |
| Sections | 3–5 | focus on WHAT/WHY/HOW |

**Boris Cherny's (Claude Code author) CLAUDE.md:** ~2,500 tokens (100 lines).

# Verification step

```
Run /init and review Claude's suggestions for improving the CLAUDE.md
→ If the suggestions are sharp, the structure is right
→ If it returns "everything looks fine," you've over-written it
```

# Success criteria

- Claude doesn't repeat the same mistake more than twice
- A new team member can read CLAUDE.md and start working without confusion
- CLAUDE.md stays under 200 lines
- Running `/init` returns "no improvements suggested"

# Failure patterns

- **Bloated CLAUDE.md**: past 500 lines, key rules get buried and skipped
- **Vague instructions**: "write good code" has a 35% compliance rate. Be specific
- **Duplicated rules**: repeating in the prompt what's already in CLAUDE.md
- **Forgetting to update**: not adding a PR-fixed mistake to CLAUDE.md → the same mistake reappears next week
- **Confusing it with a README**: CLAUDE.md is an onboarding doc for the AI, not for humans

# Best practices

- Run `/init` first to generate a baseline, then trim it down
- Put WIP notes and sensitive paths in `CLAUDE.local.md` (must be in .gitignore)
- Use folder-level `CLAUDE.md` to scope module-specific rules
- Review monthly and delete stale rules (rotten rules don't get followed)
- Use `/dx:review-claudemd` to auto-extract improvement suggestions from conversation history

# Anti-patterns

- Copy-pasting README content meant for humans
- Stuffing in "seems good" conventions until you blow past 200 lines
- Not writing version numbers ("React 18.3," not just "use React")
- Putting project-specific rules in the global CLAUDE.md (use the right scope)

# Example prompt

```
Run /init to generate a CLAUDE.md for the current project.
After generation, restructure it with the WHAT/WHY/HOW framework and keep it under 200 lines.
Tell me about any patterns that have caused errors before (I'll add them to CLAUDE.md).
```

# Example commands

```bash
# Generate CLAUDE.md for the first time
/init

# Get suggestions to improve CLAUDE.md
/dx:review-claudemd

# Add folder-specific rules (only read under src/)
touch src/CLAUDE.md
```

# Related skills

- [[Workspace Initialization]] — setting up the whole workspace, including CLAUDE.md
- [[Context Management Strategy]] — managing the token cost of CLAUDE.md
- [[Hook System Architecture]] — using hooks to enforce rules CLAUDE.md can't
