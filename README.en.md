# loop-designer

[中文](README.md) | English

`loop-designer` is a Codex/Claude skill for turning repeated agent work into clear, bounded loops.

## What It Does

This skill helps an agent decide:

- whether a loop is needed at all;
- which loop type fits the task;
- when the loop should stop;
- what evidence proves progress or completion;
- what can run inside the current session;
- what needs an external runner;
- what actions are safe for the agent to take.

## Loop Types

### Turn-based

A normal conversation turn. The agent gathers context, acts, verifies the result, and responds.

Use it for one-off, exploratory, or short tasks. A normal conversation turn is already a loop.

### Goal-based

A bounded loop with a measurable target. The agent keeps working until the goal is met, the attempt limit is reached, or a hard blocker prevents progress.

Use it for tasks with objective acceptance criteria, such as making tests pass, improving a score, or extracting page content and verifying required fields.

### Time-based

Discrete runs triggered by a schedule. Each run wakes up, does bounded work, records the result, and exits.

Use it for scheduled checks of external state, such as daily email summaries, PR checks, or periodic data pulls. It needs an external runner.

### Proactive

Event, queue, or stream driven work. Each subtask can finish, but the routine keeps watching until disabled.

Use it for ongoing event streams, such as issue triage, feedback routing, dependency upgrades, or migration batches. It also needs an external runner and a clear cancellation path.

## Install

Copy the skill into your local skills directory:

```bash
mkdir -p ~/.agents/skills/loop-designer
cp skill/loop-designer/SKILL.md ~/.agents/skills/loop-designer/SKILL.md
```

Then restart Codex so the skill list reloads.

## When To Use

Use this skill when you want to say things like:

- "Help me design a loop for checking this PR."
- "Turn this repeated workflow into a bounded agent routine."
- "Run a goal loop until this test passes, but stop after four attempts."
- "Plan a daily summary routine, but do not claim it is running until the scheduler is set up."

## Output

For a loop design request, a typical output starts with a contract:

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

**Execution Plan**
1. ...
2. ...
3. ...

**Stop Rule**
...
```

The skill first makes the loop's goal, trigger, stop condition, and verification method explicit. A scheduled loop without a runner, logs, persisted state, and cancellation path can be designed, but it should not be claimed as running.

## Example

User:

```text
Make the tests pass. Try at most 4 times and report the smallest remaining failure.
```

Typical output:

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
- Runner: Current agent session.

**Execution Plan**
1. Run the test once to capture the current failure.
2. Make the smallest evidence-backed change.
3. Re-run the same command.
4. Stop when it passes or after attempt 4.
```

## Source

This skill was inspired by the WeChat translation of "Getting started with loops":

```text
从零开始玩转循环 (Getting started with loops)【译】
Published: 2026-07-07
Accessed: 2026-07-07
https://mp.weixin.qq.com/s/lG4WU5sSykOVu4XMHtWq1g
```
