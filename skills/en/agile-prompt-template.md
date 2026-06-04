---
name: Agile Prompt Template
description: A pattern that applies agile ticket structure (Context / To-dos / Not-to-dos / Acceptance Criteria) to AI prompts. Combined with the Role→Goal→Constraints framework, it solves the "vague prompts produce vague implementations" problem. Also covers how to design a reusable, team-shareable prompt library.
tags: [prompt-engineering, templates, agile, quality, reuse]
difficulty: Beginner
sources:
  - https://medium.com/google-cloud/taming-vibe-coding-the-engineers-guide-fff70b6d807a
  - https://www.augmentcode.com/guides/master-prompt-engineering-techniques-for-ai-coding
  - https://dev.to/raghavyuva/the-art-of-vibe-coding-with-actual-discipline-lo
---

> **In one line:** Give Claude a ticket, not a wish — Context / To-dos / Not-to-dos / Acceptance Criteria — so vague prompts stop producing vague code. Use it when asking for any feature, fix, or refactor.

# Purpose

Based on the principle "prompt quality = the ceiling on implementation quality," convert vague prompts into a structured format. Once you write a good prompt, turn it into a template and reuse it across the team.

# When to use

- Asking for a new feature or component implementation
- Asking for a bug fix or refactoring
- Standardizing AI-development prompt quality across a team
- Repeating the same kind of work (adding tests, generating docs, etc.)

# When not to use

- Exploratory questions / research requests (no structure needed)
- One-line changes or self-evident tasks

# Inputs

- The task's goal
- Relevant constraints and conventions
- Success criteria

# Workflow

## Framework 1: The agile-ticket prompt

Apply the agile ticket format to your prompt:

```markdown
## Context
[Why this task is needed, relevant constraints, architectural context]

## To-dos
- [Concrete task 1]
- [Concrete task 2]
- [Concrete task 3]

## Not-to-dos
- [Out-of-scope changes]
- [Approaches you must not use]
- [Files you must not touch]

## Acceptance Criteria
- [ ] [Verifiable condition 1]
- [ ] [Verifiable condition 2]
- [ ] [The test command passes]
```

**Concrete example (implementing auth):**
```markdown
## Context
Implement JWT refresh tokens. There is existing session management in
src/auth/session.ts — follow that pattern.
Manage tokens via RedisCache (no direct DB writes).

## To-dos
- Add a JWT refresh-token endpoint at POST /auth/refresh
- Set token expiry to 15 minutes (security requirement)
- Make refresh tokens valid for 7 days

## Not-to-dos
- Do not modify src/auth/oauth.ts (handled in a separate PR)
- Do not write custom encryption (use the jsonwebtoken library)
- Do not write to the DB directly (always go through RedisCache)

## Acceptance Criteria
- [ ] npm test -- --grep "jwt refresh" passes
- [ ] Expired tokens return 401
- [ ] A valid refresh returns a new token pair
- [ ] Zero TypeScript type errors
```

## Framework 2: Role→Goal→Constraints

Make the AI's role, goal, and constraints explicit:

```markdown
# Role
You are a senior engineer with expertise in [domain].
You are well-versed in [team conventions].

# Goal
Build [concrete deliverable].

# Constraints
- Language/framework: [specified]
- Test requirements: [specified]
- Style guide: [specified]
- Performance requirements: [specified]
```

**Concrete example (implementing an API endpoint):**
```markdown
# Role
You are a senior backend engineer specializing in TypeScript and Node.js.
This team uses Express.js + Prisma + Jest.

# Goal
Implement the user profile-update API endpoint (PUT /users/:id).

# Constraints
- Validate the request body with Zod
- Always go through the auth middleware (see src/middleware/auth.ts)
- A user can update only their own profile (others get 403)
- Write the Jest tests alongside the endpoint
- Sanitize all input values (XSS protection)
```

## Framework 3: Few-shot (example-driven)

Use this for transformations that follow the same pattern:

```markdown
# Instruction
Implement [task] following the examples below:

# Example 1
Input: [input example 1]
Output: [expected output 1]

# Example 2
Input: [input example 2]
Output: [expected output 2]

# Target
Input: [the actual input]
```

## Building a prompt library

Template and save the prompts you use often:

```markdown
# .claude/commands/implement-feature.md (the /implement-feature command)
---
name: implement-feature
description: Template for implementing a new feature
---

Implement the feature following this template:

## Context
For the feature described in $ARGUMENTS:
- Read the relevant files and grasp the existing patterns
- Identify the files that need to change

## To-dos
Implement what's listed in the To-dos section of $ARGUMENTS

## Not-to-dos
Do not modify what's listed in the Not-to-dos section of $ARGUMENTS

## Acceptance Criteria
Satisfy all of the Acceptance Criteria in $ARGUMENTS
Run [test command] afterward to confirm
```

```markdown
# .claude/commands/write-tests.md (the /write-tests command)
---
name: write-tests
description: Standard template for writing tests
---

Read @$ARGUMENTS and write tests against these criteria:

Targets:
- Happy path (at least 2 cases)
- Error cases (at least 3: invalid input, no permission, nonexistent ID)
- Edge cases (empty string, null, max value, etc.)

Constraints:
- Minimize mocks (use the real DB)
- Tests run independently (no ordering dependency)
- Nest describe/it at most 2 levels deep
```

## Bad prompt → good prompt transformation

```
❌ Bad (vague):
"Add a payment feature"

✅ Good (agile-ticket format):
## Context
Implement monthly subscription billing with Stripe.
Reference the existing src/billing/ and use the same pattern.

## To-dos
- Add a POST /api/subscribe endpoint
- Create a Stripe Checkout session
- Handle payment completion via webhook (/api/webhooks/stripe)

## Not-to-dos
- Do not write custom payment logic (use the Stripe SDK only)
- Do not test with production Stripe keys (test keys only)
- Do not modify the existing src/billing/invoice.ts

## Acceptance Criteria
- [ ] The payment flow completes in test mode
- [ ] Webhook signature verification is implemented (security required)
- [ ] A proper error is returned on payment failure
- [ ] npm test passes
```

## Making step execution explicit

Break a task into stages to keep control of the AI:

```markdown
Implement in the following steps:

Step 1: Read src/auth/ to understand the existing patterns
→ Summarize what you understood before moving on

Step 2: Present an implementation plan (do not write code)
→ Confirm the plan before moving on

Step 3: Implement according to the Step 2 plan

Step 4: Run the tests and report the results
```

# Verification step

```
Self-check after writing a prompt:
□ Is the "why" (Context) included?
□ Are the "to-dos" concrete (verb + object)?
□ Are the "not-to-dos" spelled out?
□ Are the "acceptance criteria" verifiable?
□ Are the libraries / patterns to use specified?
□ Can this be implemented from the prompt alone (no follow-up questions)?
```

# Success criteria

- The AI understands "what to do" from a single prompt
- The implementation satisfies all Acceptance Criteria
- No out-of-scope changes occur (the Not-to-dos are respected)

# Failure patterns

- **Unmeasurable Acceptance Criteria**: "write good code" — how do you confirm it?
- **No Not-to-dos**: the AI "improves" out-of-scope files
- **No Context**: with no "why," the AI takes the shortest path and contradicts existing patterns
- **Stale examples**: few-shot examples differ from current code, so it implements the wrong pattern

# Best practices

- Make Acceptance Criteria objective, like "[test command] passes"
- Write specific file paths in Not-to-dos for "files you must not touch"
- Save your first good prompt as a template in `.claude/commands/`
- Include not just the "why" in Context but also "the path to the related existing implementation"

# Anti-patterns

- Acceptance Criteria that are only "it works" / "tests pass" (which tests?)
- Skipping Context and assuming "it'll figure it out"
- Copy-pasting the same prompt over and over (never templating it)

# Example prompt

```markdown
## Context
[Why this task is needed]
[Path to the existing implementation to reference]
[Libraries / patterns to use]

## To-dos
- [Concrete implementation]
- [Write tests]

## Not-to-dos
- [Files you must not touch]
- [Approaches you must not use]

## Acceptance Criteria
- [ ] [test command] passes
- [ ] [type-check command] passes
- [ ] [concrete behavioral check]
```

# Example commands

```bash
# Register the prompt template as the /implement-feature command
cat > .claude/commands/implement-feature.md << 'EOF'
---
name: implement-feature
description: Implement a feature in Context/To-dos/Not-to-dos/Acceptance Criteria format
---
Implement following this template: $ARGUMENTS
EOF

# Usage
/implement-feature "
## Context: Add JWT auth
## To-dos: Implement the login endpoint
## Not-to-dos: Do not modify existing session management
## Acceptance Criteria: npm test passes
"
```

# Related skills

- [[CLAUDE.md Architecture]] — setting global rules in CLAUDE.md
- [[CLAUDE.md Curation Protocol]] — periodic review of prompt rules
- [[Workspace Initialization]] — initial setup of command files
- [[Vibe-Coding Escape Protocol]] — the relationship between prompt quality and implementation quality
