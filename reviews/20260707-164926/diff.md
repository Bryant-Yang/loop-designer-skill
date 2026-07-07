Review target: /Users/bobo/.agents/skills/loop-designer/SKILL.md
Review lens: documentation + workflow design for a Codex/Claude skill.
User request: cross-review this skill after reading a WeChat article about Claude Code loops, including its diagrams.

--- FILE: /Users/bobo/.agents/skills/loop-designer/SKILL.md ---
---
name: loop-designer
description: Design and run practical AI agent loops. Use when the user asks to 做 loop, 设计循环, automate repeated agent work, set up /goal, /loop, /schedule, routines, monitors, recurring checks, autonomous workflows, or turn a repeated manual AI workflow into a bounded process. This skill helps choose the simplest loop type, define stop conditions, verification signals, token/time budgets, and checkpoints before execution.
---

# Loop Designer

Help the user turn repeated AI work into a bounded loop with a clear trigger, stop condition, verification signal, and cost boundary.

This skill is based on the loop model from "Getting started with loops": an AI agent repeatedly runs a work cycle until a predefined stop condition is met. The useful engineering move is not "prompt harder"; it is designing the surrounding system so the agent knows when to continue, when to stop, and how to prove progress.

## Core Rule

Start with the smallest loop that can do the job. Do not escalate to a more autonomous or scheduled loop unless the task shape requires it.

Before running or proposing a loop, write a short loop contract. If the contract cannot be filled in, stop and name the missing piece.

## Loop Contract

Use this structure, trimming fields that are clearly irrelevant:

```markdown
**Loop Contract**
- Objective:
- Loop type:
- Trigger:
- Stop condition:
- Verification signal:
- Max attempts or duration:
- Cost boundary:
- Required tools or access:
- Checkpoint format:
- Failure mode:
```

The contract should be concrete enough that another agent could execute it without guessing the user's intent.

## Choose The Loop Type

### 1. Turn-Based Loop

Use when the task is short, exploratory, one-off, or depends on user judgment after each pass.

- Trigger: user prompt.
- Work cycle: gather context, take action, verify the work, then respond.
- Stop: the agent judges the task done, asks for missing context, or the effort budget runs out.
- Best for: small code changes, reading an article, drafting, one-off diagnosis, quick local checks.
- Cost control: make the prompt specific; write verification into a skill or checklist if the same check will repeat.

Do not overbuild this into automation. A normal conversation turn is already a loop.

### 2. Goal-Based Loop

Use when there is a measurable end state and the agent should iterate until it reaches that state.

- Trigger: explicit goal or a task with objective pass/fail criteria.
- Work cycle: the agent works on the task; an evaluator checks the condition; if the condition is not met, the agent is sent back to work.
- Stop: the goal is met, the turn/attempt limit is reached, or a blocker repeats.
- Best for: raising a score, making tests pass, eliminating a known error, meeting a benchmark, improving a generated artifact against objective checks.
- Cost control: cap attempts; define the verifier before starting.

Good goal examples:

```text
Make the failing test suite pass. Stop after 5 attempts and report the smallest remaining failure.
Improve the homepage Lighthouse performance score to 90 or above. Stop after 5 tries.
Extract this article and produce a summary with title, author, date, and 5 key claims. Stop if the page is blocked after 3 distinct access methods.
```

Bad goal examples:

```text
Make it better.
Keep working until it feels polished.
Try a bunch of ideas.
```

### 3. Time-Based Loop

Use when the same task should run periodically because external state changes over time.

- Trigger: interval or schedule.
- Stop: user cancels it, the external task is finished, or the routine reaches a defined terminal state.
- Best for: checking PR reviews, monitoring CI, daily summaries, periodic inbox triage, recurring data pulls.
- Cost control: match interval to the real change rate. Do not poll every minute for something that changes daily.

Before creating a recurring loop, confirm:

- What external source changes?
- How often can it realistically change?
- What action should happen when it changes?
- What should be logged or reported each run?

### 4. Proactive Loop

Use when the system should react to events or continuously process a stream of well-defined work without the user present.

- Trigger: event, queue, webhook, routine, or scheduled scan.
- Stop: each subtask exits when done; the overall routine continues until disabled.
- Best for: issue triage, bug report routing, dependency upgrades, migration batches, repeated review/fix flows.
- Cost control: split deterministic routing from judgment-heavy work. Use scripts or smaller models for classification and reserve stronger models for ambiguous decisions.

Proactive loops need more infrastructure. If the user only needs a one-off or short repeated run, choose goal-based or time-based instead.

A strong proactive loop separates roles:

- Trigger watches the external source, such as Slack, GitHub, an inbox, or a queue.
- Main agent runs the goal loop until the verification skill passes.
- The system opens or updates the output artifact, such as a PR, report, issue update, or migration batch.
- Second agent reviews the result and reports risk.
- Human decides what to merge, publish, send, or delete.

Keep the human at irreversible boundaries unless the user explicitly authorizes automatic final action.

## Execution Cycle

For each loop pass:

1. Read the current state from the real source of truth.
2. Decide the next smallest action that moves toward the objective.
3. Execute that action.
4. Run the verification signal.
5. Compare the result to the stop condition.
6. Either stop with evidence, continue with a changed tactic, or fail loud with the blocker.

Do not keep retrying the same failed action without new information.

## Verification Signals

Prefer deterministic checks:

- Tests passing or failing.
- Typecheck or build result.
- HTTP status and response shape.
- Browser DOM assertion, screenshot, console error count, or performance metric.
- File exists with expected schema.
- Count, score, threshold, or exact extracted fields.
- External system state, such as PR check status or review thread count.

If verification requires human judgment, make that explicit in the checkpoint. Do not pretend subjective review is automated.

## Cost And Safety Boundaries

Every non-trivial loop needs at least one boundary:

- Max attempts.
- Max wall-clock duration.
- Max token budget.
- Max number of files touched.
- Read-only mode until diagnosis is complete.
- Dry run before write actions.
- Stop after the same blocker appears twice unless the user explicitly asked for persistence.

Use scripts for deterministic work. If code can route, count, transform, diff, or retry, let code do it and reserve the model for judgment.

## Checkpoints

After each significant pass, report:

```markdown
**Checkpoint**
- Current state:
- Action taken:
- Verification result:
- Decision:
- Remaining risk:
```

Keep checkpoints short. They exist to preserve control of the loop, not to narrate every tool call.

## Failure Modes

Stop and surface the problem when:

- The stop condition is vague.
- The verifier cannot observe the real target.
- The loop is about to touch a broad surface without a rollback story.
- External access, credentials, or browser state blocks the actual objective.
- The same failure repeats without a new hypothesis.
- The loop would spend unbounded time or tokens.

When stopping, report what was tried, what evidence was collected, and the smallest next input needed from the user.

## User-Facing Output

For loop design requests, return:

```markdown
**Loop Contract**
...

**Execution Plan**
1. ...
2. ...
3. ...

**Stop Rule**
...
```

For loop execution requests, return the contract first, then run the loop. Final answers should include the evidence that the stop condition was met or the reason it was not.

## Examples

**User:** "帮我做个 loop，每 10 分钟看一下 PR review，有评论就修。"

**Response shape:**
- Choose time-based loop.
- Verify PR identity, review source, CI source, write permission, and maximum runtime.
- Define stop as "no unresolved actionable review comments and CI green" or "same blocker appears twice".
- Use a 10 minute interval only if the PR review cadence justifies it.

**User:** "让它一直改到 UI 好看。"

**Response shape:**
- Push back: "好看" is not a stop condition.
- Convert to measurable checks: screenshot review, layout overlap checks, mobile/desktop viewport checks, no console errors, explicit design direction.
- Use a goal-based loop with max attempts.

**User:** "每天早上总结我的邮件。"

**Response shape:**
- Choose time-based loop.
- Trigger daily at the requested time.
- Stop each run after producing the summary.
- Define buckets, output format, and what counts as urgent.
