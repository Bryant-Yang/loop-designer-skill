# loop-designer

`loop-designer` is a Codex/Claude skill for turning repeated agent work into clear, bounded loops.

`loop-designer` 是一个 Codex/Claude skill，用来把重复性的 agent 工作整理成目标明确、边界清楚、可验证的 loop。

## What It Does

This skill helps an agent decide:

- whether a loop is needed at all;
- which loop type fits the task;
- what condition should stop the loop;
- what evidence proves progress or completion;
- what can run inside the current session and what needs an external runner;
- what actions are safe for the agent to take.

它会帮助 agent 判断：

- 这件事是否真的需要 loop；
- 应该使用哪一种 loop；
- loop 什么时候停止；
- 用什么证据证明有进展或已经完成；
- 哪些事情能在当前会话里直接做，哪些需要外部调度器；
- agent 可以安全地执行哪些动作。

## Loop Types

### Turn-based

A normal conversation turn. The agent gathers context, acts, verifies, and responds.

适合一次性、探索性、短任务。普通对话回合本身就是一种 loop。

### Goal-based

A bounded loop with a measurable target. The agent keeps working until the goal is met, the attempt limit is reached, or a hard blocker prevents progress.

适合有明确验收标准的任务，例如让测试通过、把指标提升到某个阈值、提取页面内容并验证字段完整性。

### Time-based

Discrete runs triggered by a schedule. Each run wakes up, does bounded work, records the result, and exits.

适合定时检查外部状态，例如每天汇总邮件、定时检查 PR、周期性拉取数据。它需要外部 runner。

### Proactive

Event, queue, or stream driven work. Each subtask can finish, but the routine keeps watching until disabled.

适合持续处理事件流，例如 issue triage、反馈分流、依赖升级、迁移批处理。它也需要外部 runner 和清晰的取消路径。

## Install

Copy the skill into your local skills directory:

```bash
mkdir -p ~/.agents/skills/loop-designer
cp skill/loop-designer/SKILL.md ~/.agents/skills/loop-designer/SKILL.md
```

Then restart Codex so the skill list reloads.

安装方式：

```bash
mkdir -p ~/.agents/skills/loop-designer
cp skill/loop-designer/SKILL.md ~/.agents/skills/loop-designer/SKILL.md
```

然后重启 Codex，让新的 skill 生效。

## When To Use

Use this skill when you want to say things like:

- "Help me design a loop for checking this PR."
- "Turn this repeated workflow into a bounded agent routine."
- "Run a goal loop until this test passes, but stop after four attempts."
- "Plan a daily summary routine, but do not claim it is running until the scheduler is set up."

适合这些场景：

- “帮我设计一个定时检查 PR 的 loop。”
- “把这个重复流程整理成一个有边界的 agent routine。”
- “让测试通过，最多尝试 4 次。”
- “帮我规划一个每日总结任务，但在调度器配置好之前不要说它已经在运行。”

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

中文来说，它会先把 loop 的目标、触发方式、停止条件和验收方式写清楚，再决定能不能执行。没有 runner、日志、状态保存和取消方式的定时任务，只能被设计，不能被声称已经运行。

## Example

User:

```text
让测试通过，最多试 4 次，最后告诉我最小剩余失败。
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
宝玉AI
Published: 2026-07-07
Accessed: 2026-07-07
https://mp.weixin.qq.com/s/lG4WU5sSykOVu4XMHtWq1g
```
