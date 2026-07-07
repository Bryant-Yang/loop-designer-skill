# loop-designer skill

`loop-designer` is a local Codex/Claude skill for designing bounded AI agent loops.

It helps an agent decide whether a task should be handled as a simple turn, a bounded goal loop, a scheduled loop, or a proactive workflow. The skill forces the loop to define a runner, stop condition, evaluator, source of truth, pass/fail authority, progress signal, cost boundary, checkpoint format, and write-action safeguards before execution.

## What It Is For

Use this skill when you want to:

- turn repeated, checkable agent work into a bounded loop;
- design a goal-style loop with objective pass/fail criteria;
- plan a scheduled routine that checks external state;
- design a proactive workflow for event, queue, or stream driven work;
- prevent an agent from claiming a loop is running before the runner, logs, state, and cancel path are verified.

This skill intentionally does not replace focused debugging workflows. If the user reports a one-off bug, crash, failing test, or performance regression, use a debugging skill first.

## Active Skill

The active installed copy on this machine is:

```text
/Users/bobo/.agents/skills/loop-designer/SKILL.md
```

The copy in this repository is a snapshot:

```text
skill/loop-designer/SKILL.md
```

To install or update the local active skill from this repository:

```bash
mkdir -p ~/.agents/skills/loop-designer
cp skill/loop-designer/SKILL.md ~/.agents/skills/loop-designer/SKILL.md
```

Restart Codex after installing or updating a skill so the session can pick it up.

## Repository Layout

```text
.
├── README.md
├── skill/
│   └── loop-designer/
│       └── SKILL.md
├── review-inputs/
│   ├── loop-designer-review-context.md
│   └── loop-designer-review-context-v2.md
├── reviews/
│   ├── 20260707-164926/
│   └── 20260707-173040/
└── article-assets/
    ├── cover.jpg
    ├── article-image-0.jpg
    ├── article-image-1.jpg
    └── article-image-2.jpg
```

## Review History

Two local Codex/Claude cross-reviews are included for auditability.

### First Review

Directory:

```text
reviews/20260707-164926/
```

Result: `NEEDS_REVISION`

Primary findings:

- scheduled/proactive loops were too easy to read as already executable;
- `/goal`, `/loop`, and `/schedule` were ambiguous as literal commands;
- evaluator/source-of-truth fields were under-specified;
- safety boundaries mixed advisory guidance with harness-enforced controls;
- write-action safeguards needed more operational detail.

### Second Review

Directory:

```text
reviews/20260707-173040/
```

Result: no P0/P1 blockers. Remaining feedback was P2 documentation polish.

The current `SKILL.md` incorporates the final polish pass:

- replaces ambiguous blocked-policy language with observable hard blockers;
- defines `Verification signal`, `Source of truth`, `Evaluator`, and `Pass/fail authority`;
- requires concrete capability-gate proof actions;
- sharpens time-based vs proactive loop boundaries;
- adds a runnable in-session bounded goal-loop example.

## Source Article

The skill was based on a WeChat translation of "Getting started with loops":

```text
从零开始玩转循环 (Getting started with loops)【译】
宝玉AI
Published: 2026-07-07
Accessed: 2026-07-07
URL: https://mp.weixin.qq.com/s/lG4WU5sSykOVu4XMHtWq1g
```

The `article-assets/` directory contains local copies of the article cover and three loop diagrams that were used while revising the skill.

## Verification

The latest local validation checked:

```text
frontmatter: ok
accessed_date: ok
field_glosses: ok
blocked_policy_removed: ok
plain_hard_blocker: ok
gate_verification_actions: ok
time_vs_proactive: ok
in_session_example: ok
snapshot_synced: ok
line_budget: ok
```

The active installed skill and the repository snapshot matched at the time this repository was prepared.
