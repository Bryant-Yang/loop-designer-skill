# Recommended Actions

- Profile: `normal`
- Scope: `external`
- Review type: `documentation`
- Arbiter: `none`

This file is an action-oriented view of the cross-review findings. It does not apply fixes automatically.

## Code changes

None.

## Documentation updates

### P1: "Design and run" deliverable is genuinely ambiguous — the doc claims both modes but only one executes in-turn
- Action: Add a one-line capability note up front (e.g., "§1–§2 run inside a single agent turn; §3–§4 require an external scheduler — the skill specifies them, it does not host them"). This single edit resolves the overlap between my P1 #3 and the peer's P1 capability-gate finding and scopes the word "run" honestly.
- Evidence: SKILL.md` frontmatter `description` ("Design; run practical AI agent loops"); ## User-Facing Output` ("For loop execution requests; return the contract first; then run the loop").
- Status: `one_sided`
- Confidence: `medium`

### P1: Cost/safety boundaries are presented as enforceable controls, but a skill is an advisory context doc with no enforcement harness
- Action: Split boundaries into (1) *agent-discipline checks* the model honors in-turn (max attempts, "stop after same blocker twice", dry-run-then-write, read-only-until-diagnosed) and (2) *harness-enforced controls* that require an external runner (wall-clock, token budget, scheduled cadence) — and explicitly say the skill can only enforce group 1, and that group 2 needs an external scheduler/wrapper. That removes the unverifiable-claim smell.
- Evidence: SKILL.md` → `## Cost And Safety Boundaries; ## Execution Cycle; ## User-Facing Output
- Status: `one_sided`
- Confidence: `medium`

### P1: Description promises slash commands (`/goal`, `/loop`, `/schedule`) the skill body never delivers
- Action: Either (a) drop the slash-command tokens from the description and keep plain words ("set up goal, loop, or schedule routines"), or (b) add a short section that actually explains how to ship these as custom slash commands / scheduled invocations (e.g., a command file template + a cron/CI skeleton). Don't leave the trigger promising something the skill doesn't build.
- Evidence: SKILL.md` frontmatter `description; plus `## User-Facing Output
- Status: `one_sided`
- Confidence: `medium`

### P1: Goal evaluator is under-specified
- Action: Add contract fields like `Source of truth`, `Evaluator`, and `Pass/fail authority`. Require evaluator classification: deterministic command, external system state, second-agent review, or human judgment.
- Evidence: /Users/bobo/.agents/skills/loop-designer/SKILL.md:22; /Users/bobo/.agents/skills/loop-designer/SKILL.md:56; /Users/bobo/.agents/skills/loop-designer/SKILL.md:57; /Users/bobo/.agents/skills/loop-designer/SKILL.md:128
- Status: `one_sided`
- Confidence: `medium`

### P1: Recurring/proactive loops are presented as executable without a capability gate
- Action: Add an “Operational Capability Gate” before executing time-based/proactive loops: identify runtime substrate, credentials, persistence location, logging, cancel path, write permissions, and whether this environment can actually keep running. If not, output a design plus concrete setup steps, not “run the loop.”
- Evidence: /Users/bobo/.agents/skills/loop-designer/SKILL.md:3; /Users/bobo/.agents/skills/loop-designer/SKILL.md:78; /Users/bobo/.agents/skills/loop-designer/SKILL.md:94; /Users/bobo/.agents/skills/loop-designer/SKILL.md:201
- Status: `one_sided`
- Confidence: `medium`

### P1: Time-based and proactive loops are described but no runner is named — the "how does this actually execute" prerequisite is missing
- Action: Add a short "Runner / prerequisites" note: state explicitly that §1–§2 execute inside a single Claude Code/Codex turn, while §3–§4 require an external scheduler (give 1–2 concrete examples, e.g., cron + CLI, GitHub Actions, a webhook handler) and that the skill's job for those is to *specify* the loop, not host it. This also fixes the over-broad "run" claim.
- Evidence: SKILL.md` → `### 3. Time-Based Loop; ### 4. Proactive Loop
- Status: `one_sided`
- Confidence: `medium`

### P2: **P1 — Goal-evaluator under-specified; contract has "Verification signal" but no evaluator/pass-fail authority.** Supported by the contract
- Action: **P1 — Goal-evaluator under-specified; contract has "Verification signal" but no evaluator/pass-fail authority.** Supported by the contract template (only `Verification signal`) and §2's "an evaluator checks the condition" with no schema for who/what evaluates. The suggested fields (`Source of truth`, `Evaluator`, `Pass/fail authority` with deterministic/external/second-agent/human classification) are a concrete improvement and dovetail with the skill's own (good) "don't pretend subjective revie
- Evidence: reviewer output
- Status: `confirmed`
- Confidence: `high`

### P2: **P1 — Recurring/proactive loops presented as executable without a capability gate.** This overlaps my P1; the peer's "Operational Capabilit
- Action: **P1 — Recurring/proactive loops presented as executable without a capability gate.** This overlaps my P1; the peer's "Operational Capability Gate" framing (identify runtime substrate, credentials, persistence, logging, cancel path, write perms) is concretely useful and adds real value beyond mine. Supported by the supplied body text: description claims "run"; §3/§4 cite intervals/webhooks/scheduled scans with no mechanism; `## User-Facing Output` instructs "return the contract first, then run t
- Evidence: reviewer output
- Status: `confirmed`
- Confidence: `high`

### P2: **P2 — Write-action safeguards not operational enough for autonomous fix loops; PR-review example lacks path scope, dirty-worktree handling,
- Action: **P2 — Write-action safeguards not operational enough for autonomous fix loops; PR-review example lacks path scope, dirty-worktree handling, rollback, per-pass acceptance.** Supported by the supplied example at the Examples section ("每 10 分钟看一下 PR review"), which verifies identity/sources/CI/write-permission/runtime but no containment. Reasonable and file-grounded.
- Evidence: reviewer output
- Status: `confirmed`
- Confidence: `high`

### P2: Source attribution is missing. `SKILL.md:10` cites only `"Getting started with loops"` with no URL, author, date, primary-vs-translation not
- Action: Source attribution is missing. `SKILL.md:10` cites only `"Getting started with loops"` with no URL, author, date, primary-vs-translation note, or access date.
- Evidence: SKILL.md
- Status: `confirmed`
- Confidence: `high`

### P2: The cost/safety boundary concern is supported. `SKILL.md:146-152` lists wall-clock duration and token budget beside agent-discipline checks,
- Action: The cost/safety boundary concern is supported. `SKILL.md:146-152` lists wall-clock duration and token budget beside agent-discipline checks, but the skill has no harness section explaining which limits are best-effort versus externally enforced.
- Evidence: SKILL.md
- Status: `confirmed`
- Confidence: `high`

### P2: The missing runner prerequisite is supported. `SKILL.md:78-103` describes time-based and proactive loops, but no cron, CLI, daemon, cloud ro
- Action: The missing runner prerequisite is supported. `SKILL.md:78-103` describes time-based and proactive loops, but no cron, CLI, daemon, cloud routine, webhook receiver, or scheduler substrate is named.
- Evidence: SKILL.md
- Status: `confirmed`
- Confidence: `high`

### P2: The slash-command mismatch is supported in a narrowed form. `SKILL.md:3` says “set up /goal, /loop, /schedule”, but the body never states wh
- Action: The slash-command mismatch is supported in a narrowed form. `SKILL.md:3` says “set up /goal, /loop, /schedule”, but the body never states whether these are literal Claude Code commands, Codex concepts, trigger phrases, or commands to create.
- Evidence: SKILL.md
- Status: `confirmed`
- Confidence: `high`

### P2: "5 attempts" / "5 tries" in goal examples look like a norm, not an illustration
- Action: Vary the numbers across examples (e.g., 3, 5, 8) and add a parenthetical that the cap is task-specific.
- Evidence: SKILL.md` → `### 2. Goal-Based Loop; "Good goal examples"
- Status: `one_sided`
- Confidence: `medium`

### P2: **P1 — Cost/safety boundaries presented as enforceable, but a skill is advisory.** "Max wall-clock duration," "Max token budget," etc. are l
- Action: **P1 — Cost/safety boundaries presented as enforceable, but a skill is advisory.** "Max wall-clock duration," "Max token budget," etc. are listed flat as controls the loop will have, then the skill says "run the loop." A single-turn skill cannot reliably enforce wall-clock or token caps without an external harness.
- Evidence: reviewer output
- Status: `one_sided`
- Confidence: `medium`

### P2: **P1 — Description promises `/goal`/`/loop`/`/schedule` the skill body never delivers.** The body produces a Loop Contract + Execution Plan
- Action: **P1 — Description promises `/goal`/`/loop`/`/schedule` the skill body never delivers.** The body produces a Loop Contract + Execution Plan + Stop Rule; it contains no guidance for authoring custom slash commands or `.claude/commands/` / Codex command files, and no scheduling surface. In a stock Claude Code/Codex install those slash tokens are not built-in commands, so the trigger reads as a deliverable the skill doesn't produce. (Confirmed against `writing-great-skills/SKILL.md`: skills here ar
- Evidence: writing-great-skills/SKILL.md
- Status: `one_sided`
- Confidence: `medium`

### P2: **P1 — Time-based + proactive loops are described but no runner is named; "Design and run" is only honest for §1–§2.** §3/§4 concede "need m
- Action: **P1 — Time-based + proactive loops are described but no runner is named; "Design and run" is only honest for §1–§2.** §3/§4 concede "need more infrastructure" but name no concrete mechanism (cron, GitHub Action, webhook receiver, background task). Nothing in `writing-great-skills/SKILL.md` supplies a runner convention either, so this gap is not covered elsewhere.
- Evidence: writing-great-skills/SKILL.md
- Status: `one_sided`
- Confidence: `medium`

### P2: **P2 — Proactive role separation / goal-evaluator flow rendered as flat prose; article diagrams absent.**
- Action: **P2 — Proactive role separation / goal-evaluator flow rendered as flat prose; article diagrams absent.**
- Evidence: reviewer output
- Status: `one_sided`
- Confidence: `medium`

### P2: **P2 — Source attribution unverifiable; no primary-source link.**
- Action: **P2 — Source attribution unverifiable; no primary-source link.**
- Evidence: reviewer output
- Status: `one_sided`
- Confidence: `medium`

### P2: **P2 — Two of three goal examples fix attempts at "5," reading like a norm.**
- Action: **P2 — Two of three goal examples fix attempts at "5," reading like a norm.**
- Evidence: reviewer output
- Status: `one_sided`
- Confidence: `medium`

### P2: Examples do not demonstrate the required output contract
- Action: Replace at least one example with a full minimal `Loop Contract` + `Execution Plan` + `Stop Rule`, including runner/cancel/logging for time-based loops and action boundaries for write loops.
- Evidence: /Users/bobo/.agents/skills/loop-designer/SKILL.md:184
- Status: `one_sided`
- Confidence: `medium`

### P2: Loop Contract trims fields "clearly irrelevant" but gives no rule for the recursion/exit pair that most causes runaway loops
- Action: Add a contract field like `Progress signal / what-new-info-per-pass` (or fold into `Verification signal` with an explicit "must change between attempts" clause).
- Evidence: SKILL.md` → `## Loop Contract; ## Failure Modes`.
- Status: `one_sided`
- Confidence: `medium`

### P2: P1 goal evaluator is still under-specified. `SKILL.md:22-34` has only `Verification signal`; `56-60` mentions an evaluator but does not requ
- Action: P1 goal evaluator is still under-specified. `SKILL.md:22-34` has only `Verification signal`; `56-60` mentions an evaluator but does not require source of truth, evaluator type, or pass/fail authority.
- Evidence: SKILL.md
- Status: `one_sided`
- Confidence: `medium`

### P2: P1 recurring/proactive loops are still presented as runnable without a capability gate. `SKILL.md:3`, `78-99`, and `201` say “Design and run
- Action: P1 recurring/proactive loops are still presented as runnable without a capability gate. `SKILL.md:3`, `78-99`, and `201` say “Design and run”, interval/schedule/event triggers, then “run the loop”; the doc still does not require a runner, persistence, logs, cancel path, or proof the current environment can keep executing.
- Evidence: SKILL.md
- Status: `one_sided`
- Confidence: `medium`

### P2: P2 Codex-specific stop semantics still conflict. `SKILL.md:58` and `152` allow stopping when a blocker repeats / after twice; current Codex
- Action: P2 Codex-specific stop semantics still conflict. `SKILL.md:58` and `152` allow stopping when a blocker repeats / after twice; current Codex goal handling requires stricter blocked semantics, so this should be platform-scoped.
- Evidence: SKILL.md
- Status: `one_sided`
- Confidence: `medium`

### P2: P2 write-action safeguards still need operational boundaries. `SKILL.md:105-113`, `142-154`, and `205-211` mention human gates/dry runs, but
- Action: P2 write-action safeguards still need operational boundaries. `SKILL.md:105-113`, `142-154`, and `205-211` mention human gates/dry runs, but the PR review loop example still lacks allowed paths/actions, dirty-worktree handling, rollback, and final non-merge/send/delete boundaries.
- Evidence: SKILL.md
- Status: `one_sided`
- Confidence: `medium`

### P2: Proactive role separation is a pipeline but rendered as a flat list; the article's diagrams are absent
- Action: Add two compact ASCII diagrams (goal-evaluator loop; proactive role pipeline with the human-at-irreversible-boundary gate). Keep them tiny; prose can stay.
- Evidence: SKILL.md` → `### 4. Proactive Loop` ("A strong proactive loop separates roles:")
- Status: `one_sided`
- Confidence: `medium`

### P2: Source attribution is unverifiable; primary-source link missing
- Action: Add a one-line citation with URL and access date, and note it is a translated/secondary source so readers know to confirm against the primary.
- Evidence: SKILL.md` → intro paragraph
- Status: `one_sided`
- Confidence: `medium`

### P2: Stop rules conflict with platform-specific goal behavior and are too easy to apply prematurely
- Action: Separate generic loop stop policy from Codex goal status policy. For Codex, explicitly defer to current goal-tool semantics; for generic loops, define “repeats” as an observed count in the contract.
- Evidence: /Users/bobo/.agents/skills/loop-designer/SKILL.md:58; /Users/bobo/.agents/skills/loop-designer/SKILL.md:152
- Status: `one_sided`
- Confidence: `medium`

### P2: Trigger description is overloaded and likely to overtrigger
- Action: Split the description into fewer branches: design a bounded agent loop; execute a bounded in-turn/goal loop; plan scheduled/proactive loops only when a runner exists or must be specified.
- Evidence: /Users/bobo/.agents/skills/loop-designer/SKILL.md:3
- Status: `one_sided`
- Confidence: `medium`

### P2: Trigger overlap with `diagnosing-bugs` — "automate repeated agent work" is broad enough to capture ordinary debug-fix cycles
- Action: Narrow the trigger to *loop-shaped* repeated work — e.g., "turn a repeated, checkable workflow into a bounded loop" and/or "set up scheduled or goal-iterated agent routines" — and explicitly defer one-off debugging to `diagnosing-bugs`. (This confirms the peer's non-blocking note and grounds it in the sibling file.)
- Evidence: SKILL.md` frontmatter `description` ("automate repeated agent work; … autonomous workflows"); cross-checked against `/Users/bobo/.agents/skills/diagnosing-bugs/SKILL.md`.
- Status: `one_sided`
- Confidence: `medium`

### P2: Write-action safeguards are present but not operational enough for autonomous fix loops
- Action: For any loop that writes, require an action boundary: allowed paths/actions, dirty-worktree handling, rollback strategy, final human gate, and acceptance criteria for each pass.
- Evidence: /Users/bobo/.agents/skills/loop-designer/SKILL.md:105; /Users/bobo/.agents/skills/loop-designer/SKILL.md:113; /Users/bobo/.agents/skills/loop-designer/SKILL.md:142; /Users/bobo/.agents/skills/loop-designer/SKILL.md:205
- Status: `one_sided`
- Confidence: `medium`

## Design decisions

None.

## Architecture decisions

None.

## Disputed - verify before acting

None.

## Human confirmation needed

### P2: **Codex goal-status blocked threshold.** The peer's "≥3 consecutive goal turns" claim needs authoritative confirmation against current Codex
- Action: **Codex goal-status blocked threshold.** The peer's "≥3 consecutive goal turns" claim needs authoritative confirmation against current Codex docs/runtime before it can be treated as a real mismatch.
- Evidence: reviewer output
- Status: `needs_human`
- Confidence: `low`

### P2: **Intended runner for §3/§4.** Is there a Claude background-task/Codex scheduler feature, a sibling scheduler skill, or an expectation that
- Action: **Intended runner for §3/§4.** Is there a Claude background-task/Codex scheduler feature, a sibling scheduler skill, or an expectation that the user supplies cron/CI? The doc's "run" claim is only salvageable if a real substrate exists; this is a product-intent question.
- Evidence: reviewer output
- Status: `needs_human`
- Confidence: `low`

### P2: **No slash-command convention confirmed in-tree.** I could not read `find-skills` or `codex-claude-cross-review`. Author should confirm whet
- Action: **No slash-command convention confirmed in-tree.** I could not read `find-skills` or `codex-claude-cross-review`. Author should confirm whether `/goal`/`/loop`/`/schedule` are defined elsewhere in `.agents` (which would weaken my P1 #1) or are merely aspirational triggers (which strengthens it). If the latter, decide: drop the tokens, or add command-authoring + scheduler scaffolding.
- Evidence: reviewer output
- Status: `needs_human`
- Confidence: `low`

### P2: Confirm whether proactive loops are intended to actually run in this local agent environment, or only produce implementation plans for an ex
- Action: Confirm whether proactive loops are intended to actually run in this local agent environment, or only produce implementation plans for an external scheduler/runner.
- Evidence: reviewer output
- Status: `needs_human`
- Confidence: `low`

### P2: Confirm whether the WeChat translation is the desired citation source, or whether the skill should cite the original article directly and ma
- Action: Confirm whether the WeChat translation is the desired citation source, or whether the skill should cite the original article directly and mark translated claims separately.
- Evidence: reviewer output
- Status: `needs_human`
- Confidence: `low`

### P2: Confirm whether this skill is meant to operate real Claude Code `/goal`, `/loop`, `/schedule` commands, Codex goal tooling, or a platform-ne
- Action: Confirm whether this skill is meant to operate real Claude Code `/goal`, `/loop`, `/schedule` commands, Codex goal tooling, or a platform-neutral loop-design checklist. The current document blends all three.
- Evidence: reviewer output
- Status: `needs_human`
- Confidence: `low`

### P2: For the `/goal`/`/loop`/`/schedule` triggers: are these meant to be (a) custom commands the skill *helps the user create*, or (b) commands t
- Action: For the `/goal`/`/loop`/`/schedule` triggers: are these meant to be (a) custom commands the skill *helps the user create*, or (b) commands the skill expects to *be invoked by*? The answer determines whether P1 #1 is "drop the tokens" or "add command-authoring guidance" — I couldn't verify against sibling skills whether a command-authoring convention exists here.
- Evidence: reviewer output
- Status: `needs_human`
- Confidence: `low`

### P2: For §3/§4: is there an intended runner in this environment (a scheduler skill, a Claude background-task feature, an external cron) that the
- Action: For §3/§4: is there an intended runner in this environment (a scheduler skill, a Claude background-task feature, an external cron) that the skill should point to? Without that I can't confirm whether the "run" claim is salvageable as-is or must be scoped down.
- Evidence: reviewer output
- Status: `needs_human`
- Confidence: `low`

### P2: Was the intended deliverable a design-only skill, or a skill that may actually start/run loops in Codex/Claude? The current text claims both
- Action: Was the intended deliverable a design-only skill, or a skill that may actually start/run loops in Codex/Claude? The current text claims both.
- Evidence: reviewer output
- Status: `needs_human`
- Confidence: `low`
