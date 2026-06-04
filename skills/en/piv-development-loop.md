---
name: PIV Development Loop
description: A Plan-Implement-Verify loop for reliable Claude Code implementation. Human approval gates between phases prevent overconfident wrong implementations and silent failures.
tags: [workflow, implementation, verification, quality-control]
difficulty: Beginner
source_videos:
  - https://www.youtube.com/watch?v=vFepZE_wrfg
  - https://www.youtube.com/watch?v=9YpHBUmwY5M
---

> **In one line:** Make Claude plan first and wait for your approval before writing code — so a confidently wrong implementation never ships. Use it for any multi-file or high-risk task.

# Purpose

Split every implementation task into three clear, non-overlapping phases. Separating planning from execution is the single highest-leverage pattern in Claude Code: "if the plan is good, the code is good."

# When to use

- Any implementation task that spans multiple files
- Working from a ticket, issue, or PRD
- High-risk changes (auth, payments, data migrations)
- Whenever the cost of a confidently wrong implementation is high

# When not to use

- Trivial one-line fixes
- Pure exploration / read-only tasks
- Throwaway prototypes

# Inputs

- A ticket/issue description with acceptance criteria
- File scope (the list of files in play)
- Location of the test suite
- Jira/Linear ticket ID (optional)

# Workflow

## Phase 1 — Plan (read-only, no file changes)

```
Load ticket [ID]. Start the planning phase only.
Read every relevant file. Identify all files that need to change.
Produce a numbered plan with the rationale for each change.
Do not modify any file. Wait for approval.
```

**Include in the plan:**
- Files to change (with line numbers)
- The rationale for each change
- An explicit list of what will NOT change
- The expected test command

**Gate:** A human reviews and approves before moving to Phase 2.

## Phase 2 — Implement (execute the approved plan only)

```
Plan approved. Execute items 1 through N in order.
Before each file change, confirm it matches the plan.
If you hit anything unexpected, stop and report.
Do not improvise beyond the approved plan.
```

**Rules:**
- Execute plan items in order
- No scope creep beyond the approved plan
- Stop immediately when you hit an unexpected state
- Report the deviation. Do not auto-fix.

## Phase 3 — Verify (run tests, confirm acceptance criteria)

```
Implementation complete. Run verification:
1. Run: [test command]
2. Check each acceptance criterion: [list from the ticket]
   - Report pass/fail explicitly for each criterion
3. If any criterion fails: stop. Report the failure. Wait for instructions.
   Do not auto-fix the failure.
```

**Rules:**
- Report pass/fail explicitly for every criterion
- Do not auto-fix a verification failure
- Update the ticket status only after all criteria pass

## Verification-failure protocol

```
Stop. Do not attempt an auto-fix.
Report:
- Which criterion failed
- Why it failed (root cause)
- A proposed fix as a new plan item
Wait for human approval before implementing the fix.
```

# Success criteria

- No file changes occur during the planning phase
- The implementation matches the approved plan exactly
- Verification reports an explicit pass/fail for each criterion
- No silent failures (every failure is reported to a human)

# Failure patterns

- **Skipping plan approval**: jumping from planning to implementation without human review
- **Auto-fixing verification failures**: patching silently instead of reporting
- **Plan bloat during implementation**: adding scope that wasn't in the approved plan
- **Missing acceptance criteria**: running tests without checking every criterion
- **Untestable criteria**: "it should work correctly" is not a criterion

# Best practices

- Write acceptance criteria as binary, testable assertions before you start
- Save the plan to `docs/plan-[feature].md` for an audit trail
- Use `/plan` mode (Shift+Tab) to enforce the read-only phase
- Maintain `docs/ticket-map.json` for multi-ticket tracking
- Update CLAUDE.md with what you learned after each cycle

# Anti-patterns

- Running PIV without written acceptance criteria
- Letting Claude "tidy things up a little" during implementation
- Treating verification failures as trivial — always gate them behind human approval
- PIV with no human checkpoints (defeats the purpose)

# Example prompts

```
Load ticket AUTH-42. Start the PIV loop.

Planning phase: read all auth-related files. Identify every file that
needs to change to implement JWT refresh-token rotation.
Produce a change list with line numbers. Do not modify any file.
Present the plan and wait for approval.
```

```
Plan approved. Start the implementation phase.
Execute plan items 1–5 in order.
If you hit anything not in the plan, stop.
```

```
Implementation complete. Start the verification phase.
Run: npm test -- --grep "auth"
Then check each criterion:
1. Refresh tokens rotate on use (pass/fail)
2. Expired refresh tokens return 401 (pass/fail)
3. Old tokens are invalidated after rotation (pass/fail)
If any fail: stop and report. Do not auto-fix.
```

# Example commands

```bash
# Enter plan-only mode
claude --permission-mode plan

# Or, mid-session, via slash command
/plan

# Switch to implementation mode after approval
/implement
```

# Related skills

- [[Agentic Workflow Pattern Selection]] — PIV is the core of the Sequential pattern
- [[Workspace Initialization]] — CLAUDE.md as a living document for the PIV protocol
- [[Context Management Strategy]] — keeping context clean between PIV phases
- [[Spec-Driven Development]] — integrating the planning phase with spec-driven work
- [[End-to-End SDLC Feedback Loop]] — a closed Fail→Paste→Fix→Rerun loop inside the PIV Implement phase
