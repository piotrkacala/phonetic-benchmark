# Benchmark Workflow

Local workflow for maintaining and running this stable benchmark package.

The baseline repo is not a normal product build.
It contains the benchmark definition before any implementation.
In an implementation run, the expected work is to add a runnable web application submission.

## Core Assumptions

- `AGENTS.md` is the repository contract
- `benchmark-data/alphabets.json` is the canonical benchmark data
- benchmark docs come before code
- `docs/` holds durable benchmark docs
- `docs/ai/` holds agent-operational artifacts

## Recommended Docs Layout

```text
docs/
  OVERVIEW.md
  REQUIREMENTS.md
  ALLOWED_FREEDOMS.md
  TEST_CASES.md
  EVALUATION.md
  DECISIONS.md
  ai/
    workflow/
    tasks/
      next/
      done/
    reports/
```

## Source Of Truth Order

When documents disagree, use this order:

1. `AGENTS.md`
2. `REQUIREMENTS.md`
3. `DECISIONS.md`
4. `TEST_CASES.md`
5. `ALLOWED_FREEDOMS.md`
6. `EVALUATION.md`
7. code
8. chat history

## Phase 1: Clarify The Benchmark

Goal:
arrive at an unambiguous benchmark target before writing implementation guidance.

What to define:
- product goals
- mandatory user-facing behavior
- domain rules
- scoring and validation behavior
- what implementation freedom is intentionally left open
- what is out of scope

You are done when:
- each main workflow has a clear trigger and response
- the benchmark constraints are testable
- non-goals are explicit

## Phase 2: Write The Core Docs

Create the core docs in this order:

1. `OVERVIEW.md`
2. `REQUIREMENTS.md`
3. `TEST_CASES.md`
4. `ALLOWED_FREEDOMS.md`
5. `EVALUATION.md`
6. `DECISIONS.md`

Rules:
- `REQUIREMENTS.md` defines the benchmark contract
- `TEST_CASES.md` defines the concrete scenarios used for evaluation
- `ALLOWED_FREEDOMS.md` states where implementations may vary
- `EVALUATION.md` explains how outputs are compared
- `DECISIONS.md` records why benchmark choices were made

## Phase 3: Add Or Update Any Shell

Goal:
keep any starter implementation clearly secondary to the benchmark docs if the benchmark scope ever
changes to include one.

Rules:
- do not let a starter shell redefine the benchmark contract
- do not make one framework look mandatory unless the benchmark version says so
- if a shell is kept, label it as a baseline artifact only

Current repository decision:
- do not keep a canonical starter shell in the baseline repository

## Phase 4: Run The Benchmark

When the package is stable, make sure the workflow documents:
- what files are given to the model
- what starting code is provided
- what the model is allowed to change
- what commands are used to validate outputs
- how results are recorded

Implementation-run rule:
- the model is allowed and expected to add application code, package metadata, tests, and run
  instructions needed for a complete submission
- this does not turn the baseline repository into a canonical starter app

## Operational Rule

Keep this repository self-contained.
Do not leave durable references to files outside this repository.
