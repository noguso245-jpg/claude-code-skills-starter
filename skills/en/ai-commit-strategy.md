---
name: AI Commit Strategy
description: A git commit strategy tuned for AI coding sessions. Combine "1 task = 1 commit (a game save point)," Conventional Commits format, git-diff injection, and checkpoint commits before risky operations to manage progress safely and roll back fast.
tags: [git, commits, version-control, save-points, conventional-commits]
difficulty: Beginner
sources:
  - https://addyosmani.com/blog/ai-coding-workflow/
  - https://github.com/awattar/claude-code-best-practices
  - https://addyosmani.com/blog/self-improving-agents/
---

> **In one line:** Commit 1 task = 1 commit so every AI session has frequent save points you can roll back to instantly. Use it in any AI coding session, especially refactors and large tasks.

# Purpose

In AI coding sessions, changes are fast and far-reaching, so commit granularity matters more than in human-only development. "I'll clean it up later in one big commit" is fatal in an AI session. Fine-grained save points let you recover from failure instantly.

# When to use

- Any AI coding session (always on)
- Refactoring legacy code
- Working through a large task in small pieces
- Before trying an experimental change

# When not to use

- A one-line typo fix (too fine-grained for a commit)
- WIP (work-in-progress) commits — though checkpoint commits are fine

# Inputs

- What the completed task was
- The list of changed files
- The task type (feat / fix / refactor / chore / test / docs)

# Workflow

## Core principle: the game save-point mindset

```
Human dev:    a day of work → one big commit
AI coding:    one task done → commit immediately → next task

Why:
- AI changes a lot, fast
- The smaller the commit when a problem surfaces, the cheaper the rollback
- With save points, you can try experimental changes without fear
```

## Pattern 1: Commit immediately on task completion

```bash
# Commit immediately after each task (aim for under 30-minute intervals)
git add src/auth/jwt.ts tests/auth/jwt.test.ts
git commit -m "feat(auth): implement JWT refresh-token rotation"

# Move to the next task (save point established)
```

**Rule to add to CLAUDE.md:**
```markdown
## Commit discipline
- Commit immediately after each task completes
- "I'll batch it later" is forbidden
- Run npm test before committing
```

## Pattern 2: Conventional Commits format

Keep Claude's generated commit messages consistent:

```
Format: <type>(<scope>): <description>

type:
  feat     - new feature
  fix      - bug fix
  refactor - refactoring
  test     - add/change tests
  docs     - documentation update
  chore    - config / build / dependencies
  perf     - performance improvement

Examples:
feat(auth): implement JWT refresh-token rotation
fix(payments): fix duplicate-processing bug in Stripe webhook
refactor(user): switch UserService to dependency injection
test(api): add integration tests for /users endpoint
```

**Prompt for Claude:**
```
Analyze the changes and propose a commit message in Conventional Commits format.
format: <type>(<scope>): <description> (50 chars or fewer)
```

## Pattern 3: Checkpoint commits (before risky operations)

```bash
# Required step before refactors, dependency changes, or migrations
git add -A
git commit -m "chore: [checkpoint] stable state before refactor"

# Make the change
# If it fails, roll back instantly
git reset --hard HEAD~1
```

**Instruction for Claude:**
```
Before running this task:
1. Commit the current files with git add -A && git commit -m "[checkpoint]..."
2. Then execute the task
3. If something goes wrong, return with git reset --hard HEAD~1
```

## Pattern 4: git-diff injection (reinforce the AI's context)

```bash
# Provide change history at the start of an AI session
git diff HEAD~5..HEAD  # diff of the last 5 commits
git log --oneline -10  # log of the last 10 commits

# To the AI:
"Review the git diff output to understand the current direction of changes.
Continue implementing in the same pattern."
```

## Pattern 5: A dedicated flow for legacy refactoring

```bash
# Step 1: create a feature branch
git checkout -b refactor/legacy-auth

# Step 2: analysis commit (no code changes)
claude -p "Analyze the structure of src/auth/ and generate analysis.md.
Do not change any code. Produce the analysis document only."
git add .claude/analysis.md && git commit -m "docs: analyze auth module"

# Step 3: extract one module at a time (1 concern = 1 commit)
# "Refactor everything at once" is forbidden
claude -p "Based on analysis.md, extract only JwtService.
Do not touch other modules."
git add src/auth/jwt.service.ts && git commit -m "refactor(auth): split out JwtService"

# Step 4: next module
# ...repeat
```

## A smart-commit command for the AI

```bash
# Register as .claude/commands/smart-commit.md
# Run it with /smart-commit
```

```markdown
---
name: smart-commit
description: Analyze the git diff and create a Conventional Commits-format commit
---

Create a commit with the following steps:

1. Run `git diff` to review the changes
2. Run `git status` to confirm the changed files
3. Decide a Conventional Commits-format message based on the changes
4. Separate test files from implementation files and identify the right scope
5. Stage only the related files with `git add [changed files]`
6. Make the commit

Rules:
- Do NOT use `git add -A` or `git add .` (risk of including unrelated files)
- NEVER commit .env files
- Keep the commit message to 50 characters or fewer
```

# Verification step

```
Before committing, confirm:
□ Does npm test pass?
□ Are there no unrelated files in the commit? (check git status)
□ Are .env or secrets/ not staged?
□ Is the message in Conventional Commits format?
□ Is this an operation that needs a checkpoint commit first?
```

# Success criteria

- git log reads as a clear record of progress
- You can roll back to any commit within 5 seconds
- Each commit corresponds to "one reason to change"

# Failure patterns

- **"Batch it later" commits**: the rollback unit is too large when a problem appears
- **Overusing `git add -A`**: generated test files or temp files sneak in
- **Risky operations with no checkpoint**: you lose your bearings during a big refactor
- **Commit messages that are just "WIP" or "fix"**: git log becomes an uninformative record
- **One commit per session**: too coarse for the AI's fast pace of change

# Best practices

- Task done = commit (commit per task, not per unit of time)
- git log is the story of "what happened in this project" — keep it readable enough that an AI can follow it
- Provide `git diff HEAD~5` at the start of an AI session so the AI grasps the past direction of changes
- Create a "smart-commit" command you can invoke with /smart-commit

# Anti-patterns

- Telling the AI to "change everything, then commit it all at the end"
- Letting the AI modify your git config (.gitconfig)
- Treating merge commits as standalone commits

# Example prompt

```
When the task is complete, commit it with these steps:
1. Run npm test to confirm tests pass
2. Analyze the changes and decide a Conventional Commits-format message
3. git add only the related files (exclude .env and node_modules)
4. Make the commit

Commit message format:
<type>(<scope>): <description (50 chars or fewer)>
```

# Example commands

```bash
# Review changes
git status
git diff --stat

# Scoped commit
git add src/auth/ tests/auth/
git commit -m "feat(auth): shorten JWT expiry from 1h to 15m"

# Checkpoint commit
git add -A && git commit -m "chore: [checkpoint] stable state before DB migration"

# Roll back (to the checkpoint)
git reset --hard HEAD~1

# Provide context to the AI
git log --oneline -10
git diff HEAD~3..HEAD
```

# Related skills

- [[PIV Development Loop]] — when to commit at the end of each phase
- [[Session Scope Discipline]] — confirming commits before ending a session
- [[Git Worktree Parallel Development]] — managing commits across worktrees
- [[Complexity-Based Routing]] — per-phase commits for Large tasks
