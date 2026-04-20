---
name: karpathy-superpowers-bridge
description: Use when Karpathy-style simplicity must coexist with installed Superpowers workflows, especially to decide when Superpowers hard gates control the process and how to keep planning, review, and implementation minimal inside those workflows.
---

# Karpathy + Superpowers Bridge

## Overview

This bridge does **not** replace either system.

It assumes **Superpowers is already installed and discoverable in Codex**.

- **Karpathy** is the behavior layer: how to think, scope, edit, and define success.
- **Superpowers** is the workflow layer: which process gates must be followed for a given task.

Use this bridge to keep both systems compatible without merging them into one bloated prompt.

## Authority Model

Resolve conflicts in this order:

1. **User and project instructions** (`CLAUDE.md`, `AGENTS.md`, direct requests)
2. **Applicable Superpowers hard gates**
3. **Karpathy constraints inside the active workflow**
4. Default assistant behavior

Important consequence:

- Karpathy can make a Superpowers workflow **leaner**, but it cannot waive a Superpowers hard gate by itself.
- If the user or project rules explicitly opt out of a Superpowers workflow, follow that opt-out.

If a separate Karpathy skill or rule is installed, let it provide the detailed wording of the principles below. This bridge already includes a self-contained version, so separate installation is optional.

## Karpathy Baseline

Apply these principles to **every** task, including tasks that later enter Superpowers workflows.

### 1. Think Before Coding

- State assumptions explicitly instead of silently choosing them.
- If multiple interpretations exist, present them instead of guessing.
- If a simpler approach exists, say so.
- If something is unclear, stop and ask.

### 2. Simplicity First

- No features beyond what was requested.
- No abstractions for single-use code.
- No configurability or flexibility that was not requested.
- No defensive handling for impossible scenarios.
- If 200 lines can be 50, simplify.

### 3. Surgical Changes

- Touch only what the request requires.
- Do not improve adjacent code, formatting, or comments just because you noticed them.
- Match existing style unless the task explicitly changes style.
- Mention unrelated dead code if relevant; do not remove it unless asked.
- Remove only the unused imports, variables, or helpers that **your own change** made obsolete.

### 4. Goal-Driven Execution

- Translate vague requests into verifiable checks.
- Prefer tests or concrete commands over intuition.
- For multi-step work, name the step and the check that proves it is done.

## Superpowers Gates You Must Respect

When any of these skills clearly applies, follow its gate instead of bypassing it with "this seems simple":

- **`using-superpowers`**: if a skill applies, it must be consulted before acting.
- **`brainstorming`**: for creative work, new features, or behavior changes, no implementation before design approval.
- **`systematic-debugging`**: no fixes before root-cause investigation.
- **`test-driven-development`**: no production code before a failing test.
- **`receiving-code-review`**: no blind implementation of review feedback.
- **`verification-before-completion`**: no success claims without fresh verification evidence.

These gates determine **process entry and exit**.
Karpathy determines **how to behave inside the process**.

## When Karpathy-Only Mode Is Allowed

Stay in Karpathy-only mode only when **no rigid Superpowers skill clearly matches** and the task is one of these:

- Read-only explanation, summary, or repo orientation
- Comparison, critique, or planning discussion with no implementation
- Narrow mechanical edits with no meaningful behavior change
- Small cleanup that is already fully determined and does not trigger design, debugging, TDD, or verification gates

If a Superpowers description clearly matches, enter that workflow.

## Workflow Routing

Do not invoke the entire Superpowers stack blindly. Pick the smallest valid chain that satisfies the hard gates.

### New feature or behavior change

Default route:

1. `brainstorming`
2. `writing-plans`
3. `using-git-worktrees` before executing code in isolation
4. `subagent-driven-development` or `executing-plans`
5. `finishing-a-development-branch`
6. `verification-before-completion` for any completion claim

Inside this route, apply Karpathy by:

- preferring the smallest approved design
- keeping plans narrowly scoped
- rejecting speculative extensibility
- keeping diffs traceable to approved requirements

### Bug, flaky test, failing build, or unexpected behavior

Default route:

1. `systematic-debugging`
2. `test-driven-development`
3. `verification-before-completion`

If the fix grows into multi-step implementation, add:

4. `writing-plans`
5. `using-git-worktrees` before execution
6. `subagent-driven-development` or `executing-plans`

Rule of thumb:

- Unknown cause -> debugging first
- Known required behavior change -> TDD first

### Review feedback

Default route:

1. `receiving-code-review`
2. Then choose the appropriate implementation route:
   - TDD if behavior changes
   - debugging if the feedback reveals a bug or unclear failure
   - planning/execution flow if the change set is multi-step
3. `verification-before-completion`

### Pre-merge or pre-handoff review

Use `requesting-code-review`.

If the review finds issues:

- fix them with Karpathy minimalism
- do not add unrelated polish
- re-verify before continuing

### Parallel independent problems

Use `dispatching-parallel-agents` only after confirming the domains are truly independent.

Do not parallelize:

- tightly coupled changes
- edits to the same files
- failures that may share one root cause

## Karpathy Filters for Superpowers Planning

Use Karpathy to constrain `brainstorming` and `writing-plans`:

- The smallest design that satisfies the request wins.
- A plan is not allowed to invent future abstraction needs.
- A plan should not split files or layers unless the work actually needs that boundary.
- Every task should map directly to an approved requirement.
- "Nice to have" steps are scope creep unless the user approved them.

When a Superpowers plan asks for broader restructuring than the request justifies, stop and narrow the plan before execution.

## Karpathy Filters for Superpowers Review

Superpowers review prompts can drift toward heavier engineering instincts. Apply this filter:

- Missing abstractions are **not** automatically a defect.
- Missing configurability is **not** automatically a defect.
- Missing comments or documentation are **not** automatically a defect unless required or necessary for changed code comprehension.
- Scalability or extensibility concerns are **not** blocking unless the current request or current code path actually needs them.
- Scope creep is a bug, not a strength.

Treat review findings as high-signal only when they identify:

- requirement mismatch
- correctness risk
- regression risk
- missing verification or missing tests
- concrete maintainability harm in the changed code
- security or performance issues with present-day impact

## Karpathy Filters for Subagents

When a Superpowers workflow dispatches subagents or reviewers, preserve these constraints in the task framing:

- ask instead of guessing
- implement only the specified task
- avoid speculative improvements
- keep diffs surgical
- report concerns instead of bluffing certainty

Do not let subagents inherit vague goals like "improve this" or "clean this up."

## Seam Decisions

Superpowers contains a few practical seams. Use these bridge decisions to stay consistent:

### Worktrees

Treat `using-git-worktrees` as required **before executing planned implementation or starting isolated feature work**, not as a requirement for read-only design discussion.

### Debugging vs TDD

For unknown-cause failures, debugging comes before TDD.
For known required behavior changes, TDD comes before production code.

### Verification

`verification-before-completion` applies after every workflow whenever you are about to claim that work is complete, fixed, or passing.

### Internal Superpowers ambiguity

If two Superpowers documents conflict on an operational detail, do **not** silently choose the heavier path.
Name the ambiguity and ask the user, unless an existing project rule resolves it.

## Operating Sequence

For any task:

1. State assumptions, ambiguities, and the simplest viable path.
2. Check whether a rigid Superpowers skill clearly applies.
3. If yes, enter the smallest valid Superpowers chain.
4. Apply Karpathy constraints throughout planning, coding, debugging, and review.
5. Verify with fresh evidence before making any completion claim.

## When Not To Use

Do not use this bridge if:

- Superpowers is unavailable
- the goal is to replace Superpowers entirely with a lighter methodology
- the team explicitly wants Karpathy-only behavior with no workflow gates
