---
name: verification-before-completion
description: Use when about to claim that code or task work is complete, fixed, or passing, before committing, opening a PR, or reporting a task as done. Requires running the verification command and reading its output before making the claim.
---

# Verification Before Completion

## Overview

**Core principle:** evidence before claims.

A claim about the state of the work (tests pass, the bug is fixed, the build succeeds) is only as good as the check behind it. If you have not run that check in this session, after your last change, you do not yet know the answer.

## The gate

Before claiming any status of the work:

1. **Identify** the command that proves the claim.
2. **Run** the full command, fresh, after the last change.
3. **Read** the output: exit code, failure count, warnings.
4. **Compare**: does the output confirm the claim?
   - If not, report the actual status with the evidence.
   - If it does, make the claim and include the evidence.

If you cannot run the check (no environment, no permission, no test exists), say so and report the claim as unverified instead of making it.

## What counts as evidence

| Claim | Requires | Not sufficient |
|-------|----------|----------------|
| Tests pass | Test command output: 0 failures | An earlier run, "should pass" |
| Linter clean | Linter output: 0 errors | A partial check, extrapolation |
| Build succeeds | Build command: exit 0 | Linter passing, logs looking fine |
| Bug fixed | The original symptom, re-tested, passes | Code changed, assumed fixed |
| Regression test works | Red-green cycle verified | Test passes once |
| Agent completed | VCS diff shows the changes | The agent reports success |
| Requirements met | Line-by-line checklist against the request | Tests passing |

## Warning signs

Stop and run the check when you notice:

- "should", "probably" or "seems to" in a claim about the work
- About to say "Done", "Fixed" or "All green" without output from this session
- About to commit, push or open a PR without running the checks
- Trusting a subagent's or tool's success report without looking at the diff
- Relying on a partial check (one test file, the linter instead of the build)

## Common rationalizations

| Thought | Reality |
|---------|---------|
| "Should work now" | Run the check |
| "I'm confident" | Confidence is not evidence |
| "Linter passed" | The linter does not compile or run the code |
| "The agent said success" | Verify the diff yourself |
| "A partial check is enough" | It proves only the part it covered |

## Patterns

**Tests**
```
Good: [run test command] [see: 34/34 pass] "All tests pass (34/34)"
Bad:  "Should pass now" / "Looks correct"
```

**Regression tests (red-green)**
```
Good: write -> run (pass) -> revert fix -> run (must fail) -> restore -> run (pass)
Bad:  "I've written a regression test" (never seen failing)
```

**Build**
```
Good: [run build] [see: exit 0] "Build passes"
Bad:  "Linter passed" (the linter doesn't check compilation)
```

**Requirements**
```
Good: re-read the request -> checklist -> verify each item -> report gaps or completion
Bad:  "Tests pass, phase complete"
```

**Delegated work**
```
Good: agent reports success -> check the VCS diff -> verify -> report the actual state
Bad:  pass the agent's report on as-is
```

## Scope

Apply this before:

- claiming tests, builds, lint or type checks pass
- saying a bug is fixed or a feature works
- committing, pushing, opening a PR, or marking a task complete
- moving on to the next task in a multi-step plan
- reporting the result of work you delegated

It does not apply to ordinary conversation, explanations or opinions that make no claim about the state of the work.
