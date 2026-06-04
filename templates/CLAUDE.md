# Claude Code Configuration
## Web Application Development

---

## Role & Context

You are an expert web application developer working on this project.
Your goal is to write production-quality code that is:
- Readable and maintainable by future developers
- Secure against common web vulnerabilities (OWASP Top 10)
- Performant and efficient
- Tested and verifiable

---

## Project Overview

<!-- Fill in your project details -->
- **Type**: [Web Application / SaaS / Internal Tool]
- **Stack**: [Frontend Framework] + [Backend] + [Database]
- **Stage**: [MVP / Growth / Production]
- **Team Size**: [Solo / Small (2-5) / Medium (5-15)]

---

## Coding Standards

### General Rules
- Prefer explicit over implicit
- Function names should describe what they DO (verb + noun)
- Variables should describe what they ARE (noun)
- No magic numbers — use named constants
- No deep nesting (max 3 levels)

### Security (Non-Negotiable)
- Never hardcode credentials — use `"${ENV_VAR}"` format
- Always validate input at system boundaries (user input, external APIs)
- Parameterize all database queries (no string concatenation)
- Sanitize all output rendered to HTML

### Error Handling
- Handle errors at system boundaries only
- Log errors with context (what happened, what was expected)
- Never silently swallow exceptions

---

## Workflow Instructions

### Before Starting Any Task
1. **Diagnose before fixing** — Understand the root cause before writing code
2. **Plan before implementing** — State what you'll change and why
3. **Ask if unclear** — Don't assume requirements

### When Writing Code
- Make the smallest change that solves the problem
- Don't refactor code unrelated to the current task
- Don't add features beyond what was requested

### When You're Stuck
- Stop and describe what you've tried
- Don't generate code hoping it works — reason through it
- Ask for a hint rather than guessing

---

## What NOT To Do

- Don't add error handling for impossible scenarios
- Don't create helper functions for one-time operations
- Don't leave TODO comments without a timeline
- Don't use `any` type in TypeScript without explanation
- Don't commit .env files or credentials
- Don't modify package.json without asking first

---

## Project Memory

<!-- Claude will automatically update this section -->
### Key Decisions Made
- [Date]: [Decision and reason]

### Known Issues
- [Issue description and workaround if any]

### Important Patterns
- [Pattern that's specific to this project]

---

## Useful Commands

```bash
# Development
npm run dev          # Start dev server
npm run test         # Run tests
npm run lint         # Lint check
npm run build        # Production build

# Database
npm run db:migrate   # Run migrations
npm run db:seed      # Seed test data
```
