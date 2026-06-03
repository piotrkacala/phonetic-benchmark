# Review Rubric

## Purpose

This document gives reviewers a lightweight scoring aid for comparing benchmark submissions.

It is not the benchmark contract.
The contract lives in:
- `docs/REQUIREMENTS.md`
- `docs/TEST_CASES.md`

This rubric is used after contract verification, not instead of it.

## How To Use It

1. First decide whether the submission is `Unrunnable`, `Contract-Failing`, or `Comparable` using
   `docs/EVALUATION.md`.
2. Only score submissions that are `Comparable`.
3. Assign a `0-5` score in each review dimension.
4. Use the anchor descriptions below as guidance, not as a fake precision system.
5. Prefer repository evidence over reviewer taste.

## Scale

Use this common meaning across all dimensions:
- `0` = unusable or absent
- `1` = very weak
- `2` = partial
- `3` = solid
- `4` = strong
- `5` = excellent

## Dimensions

### 1. Contract Fidelity And Behavioral Correctness

What to look for:
- required behaviors are present
- fixed data is respected
- scoring matches the benchmark
- i18n and mode separation are preserved
- required benchmark and model attribution is visible in the UI

Anchors:
- `0`: major benchmark requirements are missing or contradicted
- `3`: required behaviors are implemented correctly with only minor issues
- `5`: the benchmark contract is implemented accurately and completely

### 2. Product Judgment And UX Clarity

What to look for:
- setup flow is understandable
- active run state is clear
- wrong-answer handling is understandable
- hint behavior is understandable
- result screen communicates outcome clearly
- open UX questions were closed coherently

Anchors:
- `0`: the interface makes the task confusing or error-prone
- `3`: the interface is clear enough to use and review without major friction
- `5`: the interface is especially clear, coherent, and supportive of the task

Important:
- do not score based mainly on whether the reviewer personally likes the visual style
- prefer clarity, consistency, hierarchy, and readability over taste

### 3. Technical Judgment And Codebase Coherence

What to look for:
- the app runs coherently
- the codebase is appropriately structured
- the chosen stack fits the benchmark size
- data, logic, and UI are separated sensibly
- randomness and state are handled in a reviewable way

Anchors:
- `0`: the technical delivery is brittle, unclear, or barely functional
- `3`: the implementation is technically competent and reasonably coherent
- `5`: the implementation is compact, reliable, and well-judged technically

### 4. Testing Discipline And Regression Resistance

What to look for:
- automated tests exist where appropriate
- tests reflect benchmark requirements
- scoring, input handling, and both modes are covered
- randomized behavior is tested without flaky assumptions
- obvious regression risk is reduced

Anchors:
- `0`: tests are absent, generic, or unrelated to the real benchmark
- `3`: tests cover the important flows and reduce practical regression risk
- `5`: tests are focused, requirement-aligned, and give strong confidence

### 5. Documentation And Process Quality

What to look for:
- the repository shows how to install, run, and verify the project
- important self-closed decisions are visible
- `docs/ai/IMPLEMENTATION_REPORT.md` records process evidence
- visible model/provider/runtime settings are recorded without guessing unavailable settings
- final git commit status is recorded
- docs or notes make key decisions inspectable
- any available commit history or handoff artifacts are helpful, but not required
- the reviewer can reconstruct what was done without hidden context

Anchors:
- `0`: the reviewer has to guess how the project works or how to assess it
- `3`: the repo provides enough trace to review the work with moderate effort
- `5`: the repo is easy to inspect, reproduce, and understand as a submission

## Suggested Weights

Use the canonical weighted dimensions from `docs/EVALUATION.md`:
- Contract fidelity and behavioral correctness: `35`
- Product judgment and UX clarity: `20`
- Technical judgment and codebase coherence: `20`
- Testing discipline and regression resistance: `15`
- Documentation and process quality: `10`

Suggested total:
- `sum((dimension_score / 5) * dimension_weight)`

This produces a `0-100` score.

## Short Reviewer Output

At minimum, a completed review should record:
- contract-verification status
- per-dimension scores
- weighted total if used
- main strengths
- main weaknesses
- notable self-closed decisions

## Anti-Patterns

Avoid:
- scoring visual polish as if it were the main benchmark goal
- rewarding extra features more than benchmark correctness
- penalizing framework choice when the benchmark leaves it open
- inferring hidden quality from confident wording alone
- using chat history as the main evidence source
