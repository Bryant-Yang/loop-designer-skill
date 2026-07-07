我会按 documentation review 做只读核查：先确认目标文件和周边技能约定，不会修改、暂存或提交任何文件。
目标文件已核对到 226 行。接下来我会只读检查技能元数据/触发写法的本地约定，以及是否存在与 Codex 当前可用机制不匹配的描述。
STATUS: NEEDS_REVISION

## Scope
- Reviewed `/Users/bobo/.agents/skills/loop-designer/SKILL.md` against the supplied documentation/workflow lens.
- Also inspected nearby skill-writing guidance and local skill conventions. No edits were made.
- Context gap: the original WeChat article/full diagrams were not provided, so I could verify only against the supplied article summary and current local/Codex tool surface.

## Findings
### P1: Recurring/proactive loops are presented as executable without a capability gate
- File: `/Users/bobo/.agents/skills/loop-designer/SKILL.md:3`, `/Users/bobo/.agents/skills/loop-designer/SKILL.md:78`, `/Users/bobo/.agents/skills/loop-designer/SKILL.md:94`, `/Users/bobo/.agents/skills/loop-designer/SKILL.md:201`
- Evidence: The description says “Design and run practical AI agent loops” and includes `/schedule`, recurring monitors, and proactive workflows. The execution section says “return the contract first, then run the loop.” But the current Codex tool surface has goal-state helpers, not a durable scheduler/background worker. The examples also include “每 10 分钟看一下 PR review” and “每天早上总结我的邮件” without requiring an execution substrate.
- Impact: The skill can cause an agent to imply that a scheduled/proactive loop is actually running when only a design was produced, or when persistence/logging/cancel behavior is unavailable.
- Suggested fix: Add an “Operational Capability Gate” before executing time-based/proactive loops: identify runtime substrate, credentials, persistence location, logging, cancel path, write permissions, and whether this environment can actually keep running. If not, output a design plus concrete setup steps, not “run the loop.”

### P1: Goal evaluator is under-specified
- File: `/Users/bobo/.agents/skills/loop-designer/SKILL.md:22`, `/Users/bobo/.agents/skills/loop-designer/SKILL.md:56`, `/Users/bobo/.agents/skills/loop-designer/SKILL.md:57`, `/Users/bobo/.agents/skills/loop-designer/SKILL.md:128`
- Evidence: The document mentions an evaluator checking the condition, but the Loop Contract has only “Verification signal,” not who or what evaluates it. There is no required source of truth, evaluator type, or pass/fail authority.
- Impact: For goal loops, this weakens the central contract. The agent may treat subjective self-judgment as an objective verifier, especially for artifact quality, UI quality, summaries, reviews, or “make it better” style requests.
- Suggested fix: Add contract fields like `Source of truth`, `Evaluator`, and `Pass/fail authority`. Require evaluator classification: deterministic command, external system state, second-agent review, or human judgment.

### P2: Stop rules conflict with platform-specific goal behavior and are too easy to apply prematurely
- File: `/Users/bobo/.agents/skills/loop-designer/SKILL.md:58`, `/Users/bobo/.agents/skills/loop-designer/SKILL.md:152`
- Evidence: The skill says stop when “a blocker repeats” and later “Stop after the same blocker appears twice.” Current Codex goal handling is stricter for marking a goal blocked: a repeated blocker must recur across at least three consecutive goal turns before setting blocked.
- Impact: If this skill is used with Codex `/goal`, it can teach premature blocked termination and mismatch the actual implementation.
- Suggested fix: Separate generic loop stop policy from Codex goal status policy. For Codex, explicitly defer to current goal-tool semantics; for generic loops, define “repeats” as an observed count in the contract.

### P2: Write-action safeguards are present but not operational enough for autonomous fix loops
- File: `/Users/bobo/.agents/skills/loop-designer/SKILL.md:105`, `/Users/bobo/.agents/skills/loop-designer/SKILL.md:113`, `/Users/bobo/.agents/skills/loop-designer/SKILL.md:142`, `/Users/bobo/.agents/skills/loop-designer/SKILL.md:205`
- Evidence: The doc says keep humans at irreversible boundaries and use dry runs/read-only modes, but the PR-review example only asks to verify PR identity, sources, write permission, and runtime. It does not require clean worktree checks, branch/commit ownership, rollback plan, allowed file scope, or explicit “do not merge/send/delete” boundaries.
- Impact: A repeated “review comments then fix” loop can mutate broad repo state over multiple passes without enough containment.
- Suggested fix: For any loop that writes, require an action boundary: allowed paths/actions, dirty-worktree handling, rollback strategy, final human gate, and acceptance criteria for each pass.

## Questions
- Was the intended deliverable a design-only skill, or a skill that may actually start/run loops in Codex/Claude? The current text claims both.

## Notes
- The basic taxonomy is useful and compact: turn-based, goal-based, time-based, proactive.
- The strongest section is “Verification Signals”; it correctly prefers deterministic checks and calls out subjective review.
- Trigger wording is slightly broad. Consider narrowing “automate repeated agent work” so this does not steal ordinary debugging feedback-loop tasks from `diagnosing-bugs`.
