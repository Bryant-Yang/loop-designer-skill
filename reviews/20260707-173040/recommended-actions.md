# Recommended Actions

- Profile: `normal`
- Scope: `external`
- Review type: `documentation`
- Arbiter: `none`

This file is an action-oriented view of the cross-review findings. It does not apply fixes automatically.

## Code changes

None.

## Documentation updates

### P2: Advisory vs enforceable boundaries cleanly split into "Agent-discipline boundaries" vs "Harness-enforced boundaries," with the corrective `I
- Action: Advisory vs enforceable boundaries cleanly split into "Agent-discipline boundaries" vs "Harness-enforced boundaries," with the corrective `If there is no external harness, describe harness-enforced boundaries as requirements, not guarantees.`
- Evidence: reviewer output
- Status: `confirmed`
- Confidence: `high`

### P2: Capability Gate says what to verify, not how.
- Action: P2: Capability Gate says what to verify, not how.
- Evidence: reviewer output
- Status: `confirmed`
- Confidence: `high`

### P2: Capability boundary is explicit and consistently enforced: `SKILL.md` "Capability boundary" + "Operational Capability Gate" + "User-Facing O
- Action: Capability boundary is explicit and consistently enforced: `SKILL.md` "Capability boundary" + "Operational Capability Gate" + "User-Facing Output" (`run only after the Operational Capability Gate passes` / `If the gate fails, produce the setup plan and stop`) + Example 1 Stop Rule (`Do not claim the recurring loop is running until runner, logs, state, and cancel path are verified.`). No target-state-as-current-fact leaks for scheduled loops.
- Evidence: SKILL.md
- Status: `confirmed`
- Confidence: `high`

### P2: Contract field definitions are still incomplete, revised from peer wording.
- Action: P2: Contract field definitions are still incomplete, revised from peer wording.
- Evidence: reviewer output
- Status: `confirmed`
- Confidence: `high`

### P2: Cross-skill overlap with `diagnosing-bugs` addressed by the explicit routing sentence: `If the user reports a one-off bug, failing test, cra
- Action: Cross-skill overlap with `diagnosing-bugs` addressed by the explicit routing sentence: `If the user reports a one-off bug, failing test, crash, or performance regression, prefer a debugging skill such as diagnosing-bugs.`
- Evidence: reviewer output
- Status: `confirmed`
- Confidence: `high`

### P2: Evaluator/source-of-truth contract materially clearer: separate contract fields, evaluator constrained to four enumerated types including `H
- Action: Evaluator/source-of-truth contract materially clearer: separate contract fields, evaluator constrained to four enumerated types including `Human judgment: user decides; do not pretend this is automated.`
- Evidence: reviewer output
- Status: `confirmed`
- Confidence: `high`

### P2: Evidence: `Verification signal`, `Source of truth`, `Evaluator`, and `Pass/fail authority` are fields, but only evaluator categories and pro
- Action: `Verification signal`, `Source of truth`, `Evaluator`, and `Pass/fail authority` are fields, but only evaluator categories and progress signal are defined clearly.
- Evidence: `Verification signal`, `Source of truth`, `Evaluator`, and `Pass/fail authority` are fields, but only evaluator categories and progress signal are defined clearly.
- Status: `confirmed`
- Confidence: `high`

### P2: Evidence: examples cover time-based PR review, vague UI-quality pushback, and daily email summary; none shows a bounded goal loop actually e
- Action: examples cover time-based PR review, vague UI-quality pushback, and daily email summary; none shows a bounded goal loop actually executable in-session.
- Evidence: examples cover time-based PR review, vague UI-quality pushback, and daily email summary; none shows a bounded goal loop actually executable in-session.
- Status: `confirmed`
- Confidence: `high`

### P2: Evidence: goal-loop stop condition references “platform-specific blocked policy” without defining it.
- Action: goal-loop stop condition references “platform-specific blocked policy” without defining it.
- Evidence: goal-loop stop condition references “platform-specific blocked policy” without defining it.
- Status: `confirmed`
- Confidence: `high`

### P2: Evidence: runner, persistence, logs, cancel path, credentials, permissions, and recovery are listed as items, but no proof action is specifi
- Action: runner, persistence, logs, cancel path, credentials, permissions, and recovery are listed as items, but no proof action is specified.
- Evidence: runner, persistence, logs, cancel path, credentials, permissions, and recovery are listed as items, but no proof action is specified.
- Status: `confirmed`
- Confidence: `high`

### P2: Evidence: time-based trigger is “interval or schedule”; proactive trigger includes “routine, or scheduled scan.”
- Action: time-based trigger is “interval or schedule”; proactive trigger includes “routine, or scheduled scan.”
- Evidence: time-based trigger is “interval or schedule”; proactive trigger includes “routine, or scheduled scan.”
- Status: `confirmed`
- Confidence: `high`

### P2: File: `/Users/bobo/.agents/skills/loop-designer/SKILL.md:106`
- Action: File: `/Users/bobo/.agents/skills/loop-designer/SKILL.md:106`
- Evidence: /Users/bobo/.agents/skills/loop-designer/SKILL.md:106
- Status: `confirmed`
- Confidence: `high`

### P2: File: `/Users/bobo/.agents/skills/loop-designer/SKILL.md:145`
- Action: File: `/Users/bobo/.agents/skills/loop-designer/SKILL.md:145`
- Evidence: /Users/bobo/.agents/skills/loop-designer/SKILL.md:145
- Status: `confirmed`
- Confidence: `high`

### P2: File: `/Users/bobo/.agents/skills/loop-designer/SKILL.md:278`
- Action: File: `/Users/bobo/.agents/skills/loop-designer/SKILL.md:278`
- Evidence: /Users/bobo/.agents/skills/loop-designer/SKILL.md:278
- Status: `confirmed`
- Confidence: `high`

### P2: File: `/Users/bobo/.agents/skills/loop-designer/SKILL.md:35`
- Action: File: `/Users/bobo/.agents/skills/loop-designer/SKILL.md:35`
- Evidence: /Users/bobo/.agents/skills/loop-designer/SKILL.md:35
- Status: `confirmed`
- Confidence: `high`

### P2: File: `/Users/bobo/.agents/skills/loop-designer/SKILL.md:80`
- Action: File: `/Users/bobo/.agents/skills/loop-designer/SKILL.md:80`
- Evidence: /Users/bobo/.agents/skills/loop-designer/SKILL.md:80
- Status: `confirmed`
- Confidence: `high`

### P2: Impact: agents can conflate where state lives, what is observed, how it is checked, and who decides.
- Action: Impact: agents can conflate where state lives, what is observed, how it is checked, and who decides.
- Evidence: reviewer output
- Status: `confirmed`
- Confidence: `high`

### P2: Impact: an agent may merely assert the gate instead of proving it.
- Action: Impact: an agent may merely assert the gate instead of proving it.
- Evidence: reviewer output
- Status: `confirmed`
- Confidence: `high`

### P2: Impact: recurring scan tasks can be classified either way.
- Action: Impact: recurring scan tasks can be classified either way.
- Evidence: reviewer output
- Status: `confirmed`
- Confidence: `high`

### P2: Impact: the primary executable capability lacks a complete model output.
- Action: Impact: the primary executable capability lacks a complete model output.
- Evidence: reviewer output
- Status: `confirmed`
- Confidence: `high`

### P2: Impact: this is a safety/stop condition, so ambiguity matters.
- Action: Impact: this is a safety/stop condition, so ambiguity matters.
- Evidence: reviewer output
- Status: `confirmed`
- Confidence: `high`

### P2: Missing runnable in-session worked example stands.
- Action: P2: Missing runnable in-session worked example stands.
- Evidence: reviewer output
- Status: `confirmed`
- Confidence: `high`

### P2: Slash-command ambiguity handled: no `/...` triggers; skill triggers via natural-language `description` phrases; article command names treate
- Action: Slash-command ambiguity handled: no `/...` triggers; skill triggers via natural-language `description` phrases; article command names treated as concepts (`Treat platform-specific command names from that article as concepts unless the current environment exposes those commands`).
- Evidence: reviewer output
- Status: `confirmed`
- Confidence: `high`

### P2: Suggested fix: add one worked goal-loop example with contract, execution plan, checkpoint, and stop evidence.
- Action: add one worked goal-loop example with contract, execution plan, checkpoint, and stop evidence.
- Evidence: reviewer output
- Status: `confirmed`
- Confidence: `high`

### P2: Suggested fix: add one-line glosses for those four fields.
- Action: add one-line glosses for those four fields.
- Evidence: reviewer output
- Status: `confirmed`
- Confidence: `high`

### P2: Suggested fix: explicitly define time-based as fixed discrete runs, proactive as event/queue/stream handling or always-on processing.
- Action: explicitly define time-based as fixed discrete runs, proactive as event/queue/stream handling or always-on processing.
- Evidence: reviewer output
- Status: `confirmed`
- Confidence: `high`

### P2: Suggested fix: replace with plain observable blockers like rate limit, quota, safety block, missing access, or repeated verifier failure.
- Action: replace with plain observable blockers like rate limit, quota, safety block, missing access, or repeated verifier failure.
- Evidence: reviewer output
- Status: `confirmed`
- Confidence: `high`

### P2: Suggested fix: require a controlled dry-run/wake-up test, log check, state write/read check, and cancel-path verification where applicable.
- Action: require a controlled dry-run/wake-up test, log check, state write/read check, and cancel-path verification where applicable.
- Evidence: reviewer output
- Status: `confirmed`
- Confidence: `high`

### P2: Time-based vs proactive boundary remains blurry.
- Action: P2: Time-based vs proactive boundary remains blurry.
- Evidence: reviewer output
- Status: `confirmed`
- Confidence: `high`

### P2: Undefined “platform-specific blocked policy” still stands.
- Action: P2: Undefined “platform-specific blocked policy” still stands.
- Evidence: reviewer output
- Status: `confirmed`
- Confidence: `high`

### P2: Write-action safeguards concrete: `Write boundary` contract field, the "For any loop that writes, define: Allowed files... Dirty-worktree ha
- Action: Write-action safeguards concrete: `Write boundary` contract field, the "For any loop that writes, define: Allowed files... Dirty-worktree handling... Rollback strategy. Per-pass acceptance criteria. Final human gate for merge, publish, send, delete, payment, or irreversible state changes," plus a Failure-Mode stop on `broad surface without a rollback story`.
- Evidence: reviewer output
- Status: `confirmed`
- Confidence: `high`

### P2: (Reaffirmed) "blocked policy" stop term undefined — keep or replace
- Action: Replace with `or a hard platform block (rate limit, safety block, quota) prevents progress,` or drop it.
- Evidence: SKILL.md; section "2. Goal-Based Loop"; Stop:` line
- Status: `one_sided`
- Confidence: `medium`

### P2: (Reaffirmed) Add one in-session runnable example
- Action: One bounded goal-loop example (contract + execution plan + one checkpoint + stop-with-evidence) e.g. "make the failing suite pass, ≤4 attempts, report smallest remaining failure."
- Evidence: SKILL.md; section "Examples"
- Status: `one_sided`
- Confidence: `medium`

### P2: (Reaffirmed) Capability Gate needs verification actions, not just items
- Action: For load-bearing items, add a concrete check, e.g. "verify Runner and Cancel path by triggering one controlled test run and confirming it both fires and can be halted."
- Evidence: SKILL.md; section "Operational Capability Gate"
- Status: `one_sided`
- Confidence: `medium`

### P2: (Reaffirmed) Inline-gloss the four overlapping contract fields
- Action: One line each — source of truth = where; verification signal = what you observe; evaluator = how you check; pass/fail authority = who decides.
- Evidence: SKILL.md; section "Loop Contract"
- Status: `one_sided`
- Confidence: `medium`

### P2: (Reaffirmed) Sharpen Time-Based vs Proactive trigger contrast
- Action: One contrasting sentence — time-based = discrete runs that each complete; proactive = continuous/event-stream where each subtask exits but the routine runs until disabled.
- Evidence: SKILL.md; sections "3. Time-Based Loop" / "4. Proactive Loop"
- Status: `one_sided`
- Confidence: `medium`

### P2: (Reaffirmed) Strengthen source attribution
- Action: Cite canonical English source if it exists; label `accessed 2026-07-07`; inline-summarize the model so the skill doesn't depend on the link.
- Evidence: SKILL.md; intro after "Getting started with loops"
- Status: `one_sided`
- Confidence: `medium`

### P2: **"blocked policy" stop condition still undefined.** Section "2. Goal-Based Loop": `Stop: the goal is met, the turn/attempt limit is reached
- Action: **"blocked policy" stop condition still undefined.** Section "2. Goal-Based Loop": `Stop: the goal is met, the turn/attempt limit is reached, or the platform-specific blocked policy says to stop.` The intro caveat only neutralizes article *command names*, not borrowed *policies*. An agent that hasn't read the source will ignore or hallucinate this third stop condition.
- Evidence: reviewer output
- Status: `one_sided`
- Confidence: `medium`

### P2: **Four contract fields illustrated but not defined inline.** Only `Progress signal` gets an inline gloss; `Verification signal`, `Source of
- Action: **Four contract fields illustrated but not defined inline.** Only `Progress signal` gets an inline gloss; `Verification signal`, `Source of truth`, `Evaluator`, `Pass/fail authority` overlap conceptually and in Example 1 produce near-duplicate entries.
- Evidence: reviewer output
- Status: `one_sided`
- Confidence: `medium`

### P2: **Intra-doc trigger overlap between Time-Based and Proactive.** "scheduled scan"/"routine" appear in the Proactive trigger while "schedule"
- Action: **Intra-doc trigger overlap between Time-Based and Proactive.** "scheduled scan"/"routine" appear in the Proactive trigger while "schedule" defines Time-Based; "periodic inbox triage" (time-based) vs "issue triage" (proactive) are near-indistinguishable.
- Evidence: reviewer output
- Status: `one_sided`
- Confidence: `medium`

### P2: **No runnable in-session example** despite in-session execution being the primary capability (turn-based + bounded goal-based). All three Ex
- Action: **No runnable in-session example** despite in-session execution being the primary capability (turn-based + bounded goal-based). All three Examples are non-runnable (time-based / pushback / time-based); the "Good goal examples" block gives statements, not a worked contract→plan→checkpoint→stop trace.
- Evidence: reviewer output
- Status: `one_sided`
- Confidence: `medium`

### P2: **Operational Capability Gate lists what to verify, not how.** Each item (Runner, Persistence, Logs, Cancel path, Credentials, Write permiss
- Action: **Operational Capability Gate lists what to verify, not how.** Each item (Runner, Persistence, Logs, Cancel path, Credentials, Write permissions, Recovery) names the thing to confirm; none names the verification action, so "verify" can be read as "list."
- Evidence: reviewer output
- Status: `one_sided`
- Confidence: `medium`

### P2: **Source attribution fragile/unverifiable.** Intro: single walled-garden WeChat link, no canonical English original, no "accessed" date labe
- Action: **Source attribution fragile/unverifiable.** Intro: single walled-garden WeChat link, no canonical English original, no "accessed" date label, and the skill's foundational taxonomy depends on that link resolving.
- Evidence: reviewer output
- Status: `one_sided`
- Confidence: `medium`

### P2: Capability Gate lists what to verify but not how
- Action: Add a concrete check for the load-bearing items, e.g., "verify Runner and Cancel path by triggering one controlled test run and confirming it both fires and can be halted."
- Evidence: SKILL.md` (section "Operational Capability Gate")
- Status: `one_sided`
- Confidence: `medium`

### P2: Four contract fields are illustrated but not defined inline
- Action: One-line gloss for each, or a single sentence: source of truth = where; verification signal = what you observe; evaluator = how you check; pass/fail authority = who decides.
- Evidence: SKILL.md` (section "Loop Contract")
- Status: `one_sided`
- Confidence: `medium`

### P2: Intra-doc trigger overlap between Time-Based and Proactive
- Action: One contrasting sentence — time-based = fixed discrete runs that each complete; proactive = continuous/event-stream processing where each subtask exits but the routine runs until disabled.
- Evidence: SKILL.md` (sections "3. Time-Based Loop"; "4. Proactive Loop")
- Status: `one_sided`
- Confidence: `medium`

### P2: No P0/P1/P2 findings.
- Action: No P0/P1/P2 findings.
- Evidence: reviewer output
- Status: `one_sided`
- Confidence: `medium`

### P2: No new file-grounded findings beyond the standing P2 set. The supplied text is unchanged on the lines the prior findings target, and the rea
- Action: No new file-grounded findings beyond the standing P2 set. The supplied text is unchanged on the lines the prior findings target, and the read limitation prevented discovering anything new.
- Evidence: reviewer output
- Status: `one_sided`
- Confidence: `medium`

### P2: No previous defect findings stood because my prior review reported no P0/P1/P2 issues.
- Action: No previous defect findings stood because my prior review reported no P0/P1/P2 issues.
- Evidence: reviewer output
- Status: `one_sided`
- Confidence: `medium`

### P2: No runnable in-session example despite in-session execution being the primary capability
- Action: Add one bounded goal-loop example that can execute in-session (e.g., "make the failing suite pass, ≤4 attempts, report smallest remaining failure") showing contract, plan, a checkpoint, and the stop-with-evidence rule.
- Evidence: SKILL.md` (section "Examples")
- Status: `one_sided`
- Confidence: `medium`

### P2: None beyond the confirmed/revised peer findings.
- Action: None beyond the confirmed/revised peer findings.
- Evidence: reviewer output
- Status: `one_sided`
- Confidence: `medium`

### P2: Prior positive observations still stand: scheduled/proactive capability gating is explicit, write safeguards exist, advisory vs harness-enfo
- Action: Prior positive observations still stand: scheduled/proactive capability gating is explicit, write safeguards exist, advisory vs harness-enforced boundaries are separated, and `diagnosing-bugs` overlap is mostly handled.
- Evidence: reviewer output
- Status: `one_sided`
- Confidence: `medium`

### P2: Source attribution is present but fragile and unverifiable
- Action: If a canonical English source exists, cite it alongside the WeChat translation; label the date as "accessed 2026-07-07" to disambiguate from publish date; and inline-summarize the model so the skill does not depend on the link resolving.
- Evidence: SKILL.md` (intro; after "Getting started with loops")
- Status: `one_sided`
- Confidence: `medium`

### P2: Undefined borrowed concept "blocked policy" embedded in a stop condition
- Action: Replace with plain language, e.g. "or a hard platform block (rate limit, safety block, quota) prevents progress," or drop it since the other two conditions already cover the realistic cases.
- Evidence: SKILL.md` (section "2. Goal-Based Loop"; Stop:` line)
- Status: `one_sided`
- Confidence: `medium`

## Design decisions

None.

## Architecture decisions

None.

## Disputed - verify before acting

None.

## Human confirmation needed

### P2: Does "Getting started with loops" have a canonical (non-translation) source? If so it should be cited so the model's fidelity can be checked
- Action: Does "Getting started with loops" have a canonical (non-translation) source? If so it should be cited so the model's fidelity can be checked independently of the WeChat link (see P2 #2).
- Evidence: reviewer output
- Status: `needs_human`
- Confidence: `low`

### P2: For the "platform-specific blocked policy" stop condition — is this intended to map to a concrete platform behavior (e.g., Codex/Claude turn
- Action: For the "platform-specific blocked policy" stop condition — is this intended to map to a concrete platform behavior (e.g., Codex/Claude turn limits, rate limits) that should be named, or can it be dropped? (see P2 #1)
- Evidence: reviewer output
- Status: `needs_human`
- Confidence: `low`

### P2: Sibling `description:` lines were unreadable this session; a human should confirm no current trigger-overlap remains between loop-designer's
- Action: Sibling `description:` lines were unreadable this session; a human should confirm no current trigger-overlap remains between loop-designer's `description` and `diagnosing-bugs`/`tdd`/`handoff`/`prototype`/`agently-mail` for recurring-check and triage phrasings.
- Evidence: reviewer output
- Status: `needs_human`
- Confidence: `low`

### P2: Whether "Getting started with loops" has a canonical (non-translation) English source to cite alongside the WeChat link, so the taxonomy's f
- Action: Whether "Getting started with loops" has a canonical (non-translation) English source to cite alongside the WeChat link, so the taxonomy's fidelity becomes independently checkable (prior P2 #2).
- Evidence: reviewer output
- Status: `needs_human`
- Confidence: `low`

### P2: Whether "platform-specific blocked policy" is meant to map to a concrete platform behavior (Codex/Claude turn limits, rate limits, safety bl
- Action: Whether "platform-specific blocked policy" is meant to map to a concrete platform behavior (Codex/Claude turn limits, rate limits, safety blocks) that should be named, or dropped (prior P2 #1).
- Evidence: reviewer output
- Status: `needs_human`
- Confidence: `low`

### P2: Whether there is a canonical non-WeChat original for “Getting started with loops” needs source intent or web/source confirmation.
- Action: Whether there is a canonical non-WeChat original for “Getting started with loops” needs source intent or web/source confirmation.
- Evidence: reviewer output
- Status: `needs_human`
- Confidence: `low`

### P2: Whether “platform-specific blocked policy” maps to a real Codex/Claude policy needs author intent unless it is rewritten in generic observab
- Action: Whether “platform-specific blocked policy” maps to a real Codex/Claude policy needs author intent unless it is rewritten in generic observable terms.
- Evidence: reviewer output
- Status: `needs_human`
- Confidence: `low`
