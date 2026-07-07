# loop-designer

中文 | [English](README.en.md)

`loop-designer` 是一个 Codex/Claude skill，用来把重复性的 agent 工作整理成目标明确、边界清楚、可验证的 loop。

## 它解决什么问题

它会帮助 agent 判断：

- 这件事是否真的需要 loop；
- 应该使用哪一种 loop；
- loop 什么时候停止；
- 用什么证据证明有进展或已经完成；
- 哪些事情能在当前会话里直接做；
- 哪些事情需要外部调度器；
- agent 可以安全地执行哪些动作。

## Loop 类型

### Turn-based

普通对话回合。agent 收集上下文、执行动作、验证结果，然后回复。

适合一次性、探索性、短任务。普通对话回合本身就是一种 loop。

### Goal-based

有明确目标的有界 loop。agent 会持续工作，直到目标达成、尝试次数耗尽，或遇到无法继续推进的硬阻断。

适合有明确验收标准的任务，例如让测试通过、把指标提升到某个阈值、提取页面内容并验证字段完整性。

### Time-based

由时间触发的离散运行。每次被唤醒后完成一段有边界的工作，记录结果，然后退出。

适合定时检查外部状态，例如每天汇总邮件、定时检查 PR、周期性拉取数据。它需要外部 runner。

### Proactive

由事件、队列或数据流触发的持续性工作。单个子任务会结束，但 routine 本身会持续监听，直到被关闭。

适合持续处理事件流，例如 issue triage、反馈分流、依赖升级、迁移批处理。它也需要外部 runner 和清晰的取消路径。

## 安装

把 skill 文件复制到本地 skills 目录：

```bash
mkdir -p ~/.agents/skills/loop-designer
cp skill/loop-designer/SKILL.md ~/.agents/skills/loop-designer/SKILL.md
```

然后重启 Codex，让新的 skill 生效。

## 什么时候用

适合这些场景：

- “帮我设计一个定时检查 PR 的 loop。”
- “把这个重复流程整理成一个有边界的 agent routine。”
- “让测试通过，最多尝试 4 次。”
- “帮我规划一个每日总结任务，但在调度器配置好之前不要说它已经在运行。”

## 输出形式

对于 loop 设计请求，它通常会先产出一份 contract：

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

它会先把 loop 的目标、触发方式、停止条件和验收方式写清楚，再决定能不能执行。没有 runner、日志、状态保存和取消方式的定时任务，只能被设计，不能被声称已经运行。

## 示例

用户输入：

```text
让测试通过，最多试 4 次，最后告诉我最小剩余失败。
```

典型输出：

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

## 来源

这个 skill 的设计主要参考 Claude 官方文章：

```text
Getting started with loops
作者：Delba de Oliveira, Michael Segner
发布：2026-06-30
访问：2026-07-07
https://claude.com/blog/getting-started-with-loops
```

中文阅读参考：

```text
从零开始玩转循环 (Getting started with loops)【译】
宝玉AI
发布：2026-07-07
https://mp.weixin.qq.com/s/lG4WU5sSykOVu4XMHtWq1g
```
