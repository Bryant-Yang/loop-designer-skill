---
name: loop-designer
description: Design bounded AI agent loops. Use when the user asks to 做 loop, 设计循环, create a goal-style agent loop, plan a scheduled/proactive agent routine, monitor recurring external state, or turn a repeated checkable workflow into a loop. This skill chooses the smallest loop type, defines the runner, stop condition, evaluator, source of truth, cost boundary, checkpoint format, and write-action safeguards before any execution.
---

# Loop Designer

Help the user turn repeated AI work into a bounded loop with a clear trigger, stop condition, verification signal, and cost boundary.

This skill is based on the loop model from Claude's official article "Getting started with loops": an AI agent repeatedly runs a work cycle until a predefined stop condition is met. Primary source read for this skill: "Getting started with loops", Delba de Oliveira and Michael Segner, Claude by Anthropic, published 2026-06-30, accessed 2026-07-07, https://claude.com/blog/getting-started-with-loops. Chinese reference also read: WeChat translation "从零开始玩转循环 (Getting started with loops)【译】", 宝玉AI, published 2026-07-07, https://mp.weixin.qq.com/s/lG4WU5sSykOVu4XMHtWq1g. Treat platform-specific command names from the article as concepts unless the current environment exposes those commands.

## Core Rule

Start with the smallest loop that can do the job. Do not escalate to a more autonomous or scheduled loop unless the task shape requires it.

Before running or proposing a loop, write a short loop contract. If the contract cannot be filled in, stop and name the missing piece.

If the user reports a one-off bug, failing test, crash, or performance regression, prefer a debugging skill such as `diagnosing-bugs`. Use this skill only when the user needs to turn repeated, checkable work into a bounded loop.

Capability boundary:

- Turn-based loops and bounded goal-style loops can run inside the current agent session when their verifier is available.
- Time-based and proactive loops require a runner such as a platform automation tool, cron plus CLI, GitHub Actions, a webhook handler, or another durable scheduler. This skill may design those loops, but it must not claim they are running until the runner is configured and verified.

## Loop Contract

Use this structure, trimming fields that are clearly irrelevant:

```markdown
**Loop Contract**
- Objective:
- Loop type:
- Trigger:
- Stop condition:
- Verification signal:
- Source of truth:
- Evaluator:
- Pass/fail authority:
- Progress signal:
- Max attempts or duration:
- Cost boundary:
- Runner:
- Required tools or access:
- Checkpoint format:
- Write boundary:
- Failure mode:
```

The contract should be concrete enough that another agent could execute it without guessing the user's intent.

Field meanings:

- Verification signal: the observable evidence that proves progress or completion.
- Source of truth: the place the loop reads authoritative state from.
- Evaluator: the method or reviewer that checks the verification signal.
- Pass/fail authority: the entity whose decision ends the debate.

Evaluator must be one of:

- Deterministic command: tests, build, script, query, or browser assertion.
- External system state: PR checks, review thread count, inbox state, issue status.
- Second-agent review: independent model checks the result and reports risk.
- Human judgment: user decides; do not pretend this is automated.

Progress signal means what must change between attempts to justify another pass. If a pass produces no new information, change tactic or stop.

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
- Stop: the goal is met, the turn/attempt limit is reached, or a hard blocker prevents progress, such as missing access, rate limits, quota exhaustion, safety blocks, or repeated verifier failure with no new information.
- Best for: raising a score, making tests pass, eliminating a known error, meeting a benchmark, improving a generated artifact against objective checks.
- Cost control: cap attempts; define the verifier before starting.

Good goal examples:

```text
Make the failing test suite pass. Stop after 4 attempts and report the smallest remaining failure.
Improve the homepage Lighthouse performance score to 90 or above. Stop after 5 tries.
Extract this article and produce a summary with title, author, date, and 5 key claims. Stop if the page is blocked after 3 distinct access methods.
```

Attempt caps are task-specific; do not treat these numbers as defaults.

Bad goal examples:

```text
Make it better.
Keep working until it feels polished.
Try a bunch of ideas.
```

### 3. Time-Based Loop

Use when the same task should run periodically because external state changes over time.

A time-based loop is a sequence of discrete runs. Each run wakes on a clock, does bounded work, records its result, and exits.

- Trigger: interval or schedule.
- Stop: user cancels it, the external task is finished, or the routine reaches a defined terminal state.
- Best for: checking PR reviews, monitoring CI, daily summaries, periodic inbox triage, recurring data pulls.
- Cost control: match interval to the real change rate. Do not poll every minute for something that changes daily.

Before creating a recurring loop, confirm:

- What external source changes?
- How often can it realistically change?
- What action should happen when it changes?
- What should be logged or reported each run?
- What runner will keep the loop alive?
- Where are logs, state, and cancellation instructions stored?

### 4. Proactive Loop

Use when the system should react to events or continuously process a stream of well-defined work without the user present.

A proactive loop is event, queue, or stream driven. Individual subtasks exit, but the routine itself keeps watching until disabled.

- Trigger: event, queue, webhook, routine, or scheduled scan.
- Stop: each subtask exits when done; the overall routine continues until disabled.
- Best for: issue triage, bug report routing, dependency upgrades, migration batches, repeated review/fix flows.
- Cost control: split deterministic routing from judgment-heavy work. Use scripts or smaller models for classification and reserve stronger models for ambiguous decisions.

Proactive loops need more infrastructure. If the user only needs a one-off or short repeated run, choose goal-based or time-based instead.

A strong proactive loop separates roles:

```text
Trigger watches source
  -> Main agent runs goal loop until verifier passes
  -> System opens or updates artifact
  -> Second agent reviews risk
  -> Human approves irreversible action
```

Keep the human at irreversible boundaries unless the user explicitly authorizes automatic final action.

## Operational Capability Gate

Before executing a time-based or proactive loop, verify:

- Runner: name the concrete mechanism that will wake the agent, then trigger one controlled dry run.
- Persistence: write and read back loop state, last run, and dedupe keys.
- Logs: confirm the dry run records input, action, verifier result, and errors in the expected location.
- Cancel path: prove the user can stop or pause the loop before enabling recurring execution.
- Credentials: identify the account or token and verify only the required access works.
- Write permissions: perform a dry-run or harmless write check for the allowed target only.
- Recovery: state what happens after crash, reboot, network failure, or repeated verifier failure, then verify the runner can resume or fail visibly.

If any required item is missing, output a design plus setup steps. Do not say the loop is running.

## Execution Cycle

For each loop pass:

1. Read the current state from the real source of truth.
2. Decide the next smallest action that moves toward the objective.
3. Execute that action.
4. Run the verification signal.
5. Compare the result to the stop condition.
6. Either stop with evidence, continue with a changed tactic, or fail loud with the blocker.

Do not keep retrying the same failed action without new information.

Goal-style evaluator flow:

```text
Agent works
  -> Evaluator checks condition
    -> pass: stop with evidence
    -> fail with new information: continue
    -> fail without new information: change tactic or stop
```

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

Every non-trivial loop needs boundaries. Separate best-effort agent discipline from externally enforced controls.

Agent-discipline boundaries:

- Max attempts in the current session.
- Max number of files touched.
- Read-only mode until diagnosis is complete.
- Dry run before write actions.
- Stop after the same blocker repeats according to the contract, unless the user explicitly asked for persistence.

Harness-enforced boundaries:

- Wall-clock duration.
- Token budget.
- Scheduled cadence.
- Persistent retries after reboot or network failure.

If there is no external harness, describe harness-enforced boundaries as requirements, not guarantees.

For any loop that writes, define:

- Allowed files, systems, or actions.
- Dirty-worktree handling before edits.
- Rollback strategy.
- Per-pass acceptance criteria.
- Final human gate for merge, publish, send, delete, payment, or irreversible state changes.

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

For loop execution requests, return the contract first. Then run only the loop types supported by the current environment:

- Run turn-based and bounded goal-style loops when the verifier is available.
- For time-based or proactive loops, run only after the Operational Capability Gate passes.
- If the gate fails, produce the setup plan and stop.

Final answers should include the evidence that the stop condition was met or the reason it was not.

## Examples

**User:** "帮我做个 loop，每 10 分钟看一下 PR review，有评论就修。"

**Response shape:**

```markdown
**Loop Contract**
- Objective: Keep one named PR free of actionable review comments and failing CI.
- Loop type: Time-based loop with a goal-style inner loop.
- Trigger: Every 10 minutes, only if a durable runner is available.
- Stop condition: No unresolved actionable review comments and CI green for this run.
- Verification signal: GitHub review thread count plus CI status.
- Source of truth: GitHub PR checks and review threads.
- Evaluator: Deterministic GitHub API queries; human review for ambiguous comments.
- Pass/fail authority: GitHub state for checks; user for disputed interpretation.
- Progress signal: New review comment, changed CI status, pushed commit, or new failure detail.
- Max attempts or duration: 3 fix attempts per run unless user changes it.
- Cost boundary: Touch only files needed for the reviewed issue.
- Runner: Must be named before execution, such as platform automation, cron plus CLI, or GitHub Actions.
- Required tools or access: GitHub read/write access to the PR branch.
- Checkpoint format: PR state, action taken, CI result, remaining comments.
- Write boundary: Respect dirty worktree, do not merge, do not force-push, keep rollback path.
- Failure mode: If the same blocker repeats without new information, stop and report.

**Stop Rule**
Do not claim the recurring loop is running until runner, logs, state, and cancel path are verified.
```

**User:** "让它一直改到 UI 好看。"

**Response shape:**
- Push back: "好看" is not a stop condition.
- Convert to measurable checks: screenshot review, layout overlap checks, mobile/desktop viewport checks, no console errors, explicit design direction.
- Use a goal-based loop with a task-specific max attempt cap.

**User:** "让测试通过，最多试 4 次，最后告诉我最小剩余失败。"

**Response shape:**

```markdown
**Loop Contract**
- Objective: Make the named test command pass, or isolate the smallest remaining failure.
- Loop type: Bounded goal-style loop.
- Trigger: Current user request.
- Stop condition: Test command passes, 4 attempts are used, or a hard blocker prevents progress.
- Verification signal: Exact test command exit code and failure output.
- Source of truth: Local test runner output.
- Evaluator: Deterministic command.
- Pass/fail authority: Test process exit code.
- Progress signal: Fewer failing tests, narrower failing case, new error evidence, or a code change tied to a failure.
- Max attempts or duration: 4 attempts.
- Cost boundary: Touch only files needed for the failing behavior.
- Runner: Current agent session.
- Required tools or access: Local repo, dependency install already present, test command available.
- Checkpoint format: attempt number, change made, command run, result, next decision.
- Write boundary: Respect dirty worktree, do not change unrelated files, keep rollback path.
- Failure mode: If no new evidence appears after an attempt, change tactic or stop with the blocker.

**Execution Plan**
1. Run the test command once to capture the current failure.
2. Minimize or classify the failure.
3. Apply the smallest fix tied to that evidence.
4. Re-run the same command and checkpoint the result.
5. Stop when the command passes or after attempt 4.

**Checkpoint**
- Current state: attempt 1, test command fails with [specific error].
- Action taken: [smallest relevant change].
- Verification result: [command output summary].
- Decision: continue because failure narrowed / stop because tests pass / stop because blocker appeared.
- Remaining risk: [what is still unverified].

**Stop Rule**
Final answer includes the passing command output, or the smallest remaining failure with the exact command that reproduces it.
```

**User:** "每天早上总结我的邮件。"

**Response shape:**
- Choose time-based loop.
- Trigger daily at the requested time only after a runner is configured.
- Stop each run after producing the summary.
- Define buckets, output format, what counts as urgent, where run logs live, and how to cancel the routine.
