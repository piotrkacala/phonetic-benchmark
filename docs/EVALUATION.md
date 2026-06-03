# Evaluation

## Purpose

This document defines how completed benchmark implementations should be evaluated.

The benchmark should compare:
- whether the implementation satisfies the fixed contract
- how complete and reliable the output is
- how strong the implementation and review process appears from repository evidence

This document defines the evaluation model.
Use `docs/REVIEW_RUBRIC.md` as the lightweight scoring aid for human reviewers.

## Evaluation Model

Evaluation has two layers:
1. contract verification
2. comparative scoring

The contract-verification layer decides whether an implementation is meaningfully comparable.
The comparative-scoring layer distinguishes stronger and weaker implementations that clear the
baseline contract.

## Evidence Sources

Primary evidence should come from:
- the submitted repository
- benchmark docs included in that repository
- the submission `README.md`
- `docs/ai/IMPLEMENTATION_REPORT.md`
- automated test artifacts
- documented run commands
- the observable application behavior

Chat history may help explain intent, but it should not be required to understand the submission.
Commit history may help explain process, but it is optional rather than mandatory evidence.

## Evaluation Principle

The benchmark should prefer reviewable evidence over reviewer taste.

That means:
- hard requirements should be judged against the contract
- open questions should be judged by coherence of the chosen solution
- visual design should be judged mainly for clarity and consistency, not for personal taste
- process quality should be judged from repository evidence, not from assumed intent

## Layer 1: Contract Verification

Use `docs/TEST_CASES.md` as the contract checklist.

A submission should be marked:

### Unrunnable

Use this status when:
- the reviewer cannot access the implemented app behavior at all
- the project cannot be installed or started because the Node.js-based workflow is missing or broken
- the basic submission contract is missing in a way that prevents the reviewer from determining a
  viable entry point, for example no `package.json`
- the app fails before the reviewer can exercise its implemented behavior

Missing documented commands alone do not make a submission `Unrunnable` when clear `package.json`
scripts still allow the reviewer to install and exercise the app. That remains a contract failure,
but it is distinct from an inaccessible implementation.

### Contract-Failing

Use this status when:
- the project runs, but one or more must-pass benchmark behaviors are missing or incorrect
- fixed benchmark data is ignored or replaced improperly
- the required exercise modes, i18n support, hint behavior, or scoring behavior do not match the
  benchmark contract
- submission-contract evidence is incomplete, for example install, run, or test commands are not
  documented even though the reviewer can infer a working workflow from clear `package.json` scripts
- required submission artifacts such as `README.md` or `docs/ai/IMPLEMENTATION_REPORT.md` are
  missing or materially incomplete
- a documented test command is present but does not complete successfully
- documented decisions for open product questions do not match the implemented UI behavior
- the required implementation date is generated dynamically rather than preserved as a fixed date
  for the run

### Comparable

Use this status when:
- the submission is runnable
- the contract tests pass well enough to compare quality and judgment across implementations

Only `Comparable` submissions should receive the full comparative score.

Contract status records whether the formal contract cleared, not the practical magnitude of a
failure. For every `Contract-Failing` submission, also record the failure type and describe its
functional impact. A submission-documentation miss in an otherwise usable app should remain
distinguishable from a blocked core flow.

## Layer 2: Comparative Scoring

Score comparable submissions across five dimensions.

Each dimension is rated on a `0-5` scale:
- `0` = unusable or absent
- `1` = very weak
- `2` = partial
- `3` = solid
- `4` = strong
- `5` = excellent

Weighted dimensions:
- Contract fidelity and behavioral correctness: `35`
- Product judgment and UX clarity: `20`
- Technical judgment and codebase coherence: `20`
- Testing discipline and regression resistance: `15`
- Documentation and process quality: `10`

Final weighted score:
- `sum((dimension_score / 5) * dimension_weight)`

This produces a final score on a `0-100` scale.

## Dimension Guidance

### 1. Contract Fidelity And Behavioral Correctness

Review whether the implementation:
- preserves required behaviors without silent omissions
- matches the benchmark scoring rule exactly
- respects the fixed suggestion-option data model
- handles the two exercise modes correctly
- preserves i18n and data-contract requirements
- preserves the required benchmark and model attribution in the UI

High score:
- benchmark behavior is implemented accurately and consistently

Low score:
- core behaviors are missing, contradictory, or only partially implemented

### 2. Product Judgment And UX Clarity

Review whether the implementation:
- makes the main flow understandable
- communicates setup choices clearly
- makes wrong-answer handling understandable
- makes hint behavior understandable
- presents the final result clearly
- closes open UX questions coherently

High score:
- the UX is clear, coherent, and supports the benchmark task well

Low score:
- the flow is confusing, ambiguous, or internally inconsistent

Important:
- do not reward or penalize a submission mainly for visual taste
- prefer judging usability clarity, hierarchy, readability, and coherence

### 3. Technical Judgment And Codebase Coherence

Review whether the implementation:
- uses an appropriate web-stack structure for a benchmark-sized task
- keeps the codebase understandable and reviewable
- separates data, logic, and UI in a sensible way
- avoids unnecessary complexity
- makes implementation decisions that are coherent with the benchmark scope

High score:
- the codebase is compact, coherent, and technically well-judged

Low score:
- the codebase is brittle, overbuilt, under-structured, or hard to review

### 4. Testing Discipline And Regression Resistance

Review whether the implementation:
- includes meaningful automated tests
- covers the critical product paths
- covers scoring and validation behavior
- covers both exercise modes
- makes randomness testable rather than opaque
- avoids flaky assertions that depend on one exact random order
- reduces obvious regression risk

High score:
- tests are relevant, targeted, and aligned with the benchmark contract

Low score:
- tests are shallow, generic, missing, or unrelated to the real product contract

### 5. Documentation And Process Quality

Review whether the implementation:
- explains how to install, run, and verify the project
- makes important self-closed decisions visible
- records visible model, provider, and runtime settings, or explicitly marks them as unavailable
- records whether a final git commit was created
- leaves enough review trace to understand tradeoffs
- keeps the submission inspectable without depending on hidden context
- does not depend on inaccessible private prompt context to explain key decisions

High score:
- repository artifacts make the implementation easy to review and reproduce

Low score:
- the reviewer has to infer too much from missing or unclear documentation

## Recommended Review Flow

1. Read the submission's run instructions.
2. Read `docs/ai/IMPLEMENTATION_REPORT.md`.
3. Install and run the project using the documented Node.js workflow.
4. Verify the contract behaviors from `docs/TEST_CASES.md`.
5. Assign a contract-verification status.
6. If the submission is comparable, assign `0-5` scores for each weighted dimension.
7. Calculate the weighted total.
8. Record notable strengths, weaknesses, and important self-closed decisions.

## Handling Open Questions

Open benchmark questions are not automatic failures.
They become evaluation points.

Review should ask:
- did the implementation notice the gap
- did it close the gap coherently
- did it avoid contradicting the fixed benchmark contract
- did it make the decision visible enough for reviewers to inspect

## Output Format

A reviewer should produce at minimum:
- contract-verification status
- weighted score if comparable
- must-fix findings or contract failures
- notable strengths
- notable process or documentation observations

If a numeric rubric is used, the reviewer should also record the per-dimension scores from
`docs/REVIEW_RUBRIC.md`.

## Anti-Patterns For Review

Review should avoid:
- rewarding extra features more than benchmark correctness
- penalizing framework choice when the benchmark leaves it open
- relying on undocumented reviewer preferences as if they were contract requirements
- using chat history as the primary evidence source
