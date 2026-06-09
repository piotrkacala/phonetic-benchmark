# Workflow

## Purpose

This document defines how the benchmark should be run with an implementation model.

The goal is to keep runs comparable without assuming that every model has the same tooling,
context-window size, agent orchestration features, or git integration.

## Baseline Setup

Before starting an implementation run:
- place the benchmark package in its own git repository
- make a clean baseline commit containing the benchmark docs and benchmark data
- use that committed state as the shared starting point for every model run
- record which benchmark version is being run from `docs/VERSION.md`

Why this matters:
- the reviewer needs a stable starting point
- the final implementation needs to be inspectable as a change from a known baseline
- different models should not receive different hidden repository states

## Implementation Task Interpretation

The docs-only baseline is not a refusal condition.

When the current task is an implementation run, the model must:
- add a runnable browser-based application
- add the Node.js project workflow needed to install and run it
- add tests or verification artifacts when appropriate
- preserve the benchmark docs and canonical data as the source of truth
- add submission documentation in `README.md`
- add an implementation report at `docs/ai/IMPLEMENTATION_REPORT.md`

The rule against a canonical starter shell applies to the shared baseline package.
It does not prohibit implementation code in benchmark submissions.

## Dependency Security And Runner Policy

Dependency installation, project scripts, build, test, and preview commands are untrusted execution
for benchmark purposes.

The detailed policy lives in `docs/DEPENDENCY_SECURITY.md`. This workflow document only describes
how operators and implementation models should use that policy during a run.

Current operating assumptions:
- npm is the first supported package manager for official evaluation
- build, test, and preview may use network access
- the main security boundary is credential and host filesystem isolation, not offline execution
- model instructions help compliance but are not the enforcement boundary
- official final evaluation must use the controlled runner

Implementation models may add ordinary npm dependencies within policy. If a dependency or script is
rejected during a run, the model may correct the submitted state and retry. The final repository
state and official runner result are authoritative.

## Operator Readiness Checks

Readiness checks are operator evidence, not the official controlled runner. The public benchmark
package does not require implementation models to run evaluator tooling before or during a
submission.

Useful readiness checks include repository cleanliness, expected benchmark files, common
credential-file names, `.npmrc`, runtime availability, registry reachability, isolation posture, and
final-mode `package.json` policy signals. Readiness checks do not replace the official flow that
generates a fresh lockfile, validates dependency metadata, installs dependencies, runs tests, builds
the app, and checks preview readiness.

The isolation checks are operator evidence, not a complete development sandbox. In particular, a
model or agent user that belongs to the `docker` group is reported as a development isolation risk
because that user may be able to mount host paths or reach host resources. This warning does not
replace the official final runner result, and it is not by itself a dependency-policy failure.

The common credential-file scan is most useful before a model run starts, because files such as
`.env`, `.env.local`, `.netrc`, `id_rsa`, `id_ed25519`, or generic `credentials` files should not be
present in the benchmark workspace given to the model. Operators should treat those findings as
workspace hygiene failures to fix before implementation begins. The official runner still
hard-rejects submitted `.npmrc` files as package policy, while the broader credential-file scan is
readiness/operator evidence rather than a substitute for runner isolation.

Operators may run private readiness tooling before a model run starts and again before final
evaluation. Passing readiness checks are useful evidence, but they do not replace official runner
evidence.

Simple operator flow:
- prepare a clean baseline checkout before giving the workspace to a model
- remove any reported credential-like files before the run begins
- record any reported development isolation risks, such as uncontrolled Docker or broad host
  filesystem access
- after the submission is complete, run evaluator-controlled readiness checks when available
- run the evaluator-controlled final runner outside the mutable submission workspace
- record readiness and runner results as evaluation evidence

## Git Policy

The repository must be under git.

Hard requirement:
- the run starts from a known baseline commit

Recommended:
- keep the implementation history if it is convenient in the chosen tool
- use logical commits when the workflow supports them cleanly
- create a final implementation commit when the environment allows it

Not required:
- the model does not have to create commits during the run
- the benchmark should not fail a submission only because the model did not commit its work

Reasoning:
- some model interfaces support git-heavy workflows well, others do not
- making commits mandatory would bias the benchmark toward certain tools rather than better product
  implementation

Review implication:
- final repository state is mandatory evidence
- commit history is optional evidence
- if the model creates a commit, that commit should not be pushed to a remote unless the operator
  explicitly asks for it
- if the model does not create a commit, the implementation report should say so

Allowed git actions:
- inspect status and diffs
- stage implementation changes
- create local commits

The model should avoid destructive git actions unless explicitly instructed by the operator.

## Prompt Policy

Benchmark runs should rely on repository docs as the source of truth.

Prompts may:
- tell the model which task to perform now
- tell the model which benchmark docs to read
- point the model at relevant repository files
- summarize already-completed work from the same run

Prompts should not:
- introduce hidden product requirements not present in the repository docs
- add private scoring rules not present in the repository docs
- give one model extra domain knowledge that another run would not receive
- require benchmark operators or models to work through Polish-language chat in order to complete a
  fair run

The benchmark should compare implementation quality, not prompt privilege.

For comparability, benchmark prompts and continuation handoffs should be written in English unless a
benchmark version explicitly states otherwise in repository docs.

## First-Window Prompt

The first prompt for a run should be short and standard in structure.

It should include:
- the instruction to read the benchmark docs in the repository
- the implementation task
- any practical runtime context needed to work in the repo

It should not restate the entire benchmark in chat if the repository already contains it.

Suggested first prompt:

```text
Read AGENTS.md first, then read the benchmark documents in docs/ and the data in benchmark-data/.

Implement the Phonetic Benchmark v2 web application as a complete submission in this repository.
Add the runnable Node.js-based application, package workflow, README.md, and
docs/ai/IMPLEMENTATION_REPORT.md required by the benchmark.

Record your decisions for the open product questions, visible model/provider/runtime settings, and
verification results in repository artifacts. Create a final local git commit if your environment
supports it, but do not push to a remote.
```

## Fresh-Window Handoff Policy

The benchmark must allow manual continuation in a fresh chat window.

This is important because:
- not every model supports subagents
- not every model supports long uninterrupted sessions
- not every tool handles large-context continuation equally well

Therefore the benchmark explicitly allows a human operator to:
- open a new chat window
- paste a continuation prompt
- continue the same implementation run in the same repository

This is a valid benchmark workflow, not a protocol violation.

## What A Continuation Prompt May Contain

A continuation prompt may include:
- the current task
- the relevant benchmark docs to read
- a short summary of what was already completed
- the files that were changed or should be inspected next
- any open decisions already made during the run and written into the repository

A continuation prompt should avoid:
- replaying the full prior chat transcript
- adding new hidden requirements
- drifting away from the benchmark docs as source of truth

## Durable Handoff Principle

When a run spans multiple windows, important state should live in repository artifacts where
possible.

Examples:
- changed code
- run instructions
- notes in `README.md`
- `docs/ai/IMPLEMENTATION_REPORT.md`
- task or summary files if the workflow chooses to create them

This reduces dependence on one model's hidden chat memory.

## Submission Artifact Policy

Every completed implementation run must leave:
- `README.md` with install, run, verification, stack, model, date, benchmark version, and open
  decision notes
- `docs/ai/IMPLEMENTATION_REPORT.md` with process evidence, visible model/provider settings,
  verification results, open decisions, and git-commit status

When the controlled runner exists, the implementation report should include a short summary of the
final runner result or a path to the runner artifact. It should not need to embed full raw logs.

If a model cannot see a provider or runtime setting, it should write `not exposed by interface`
instead of guessing.

## Controlled Final Evaluation Flow

Official evaluation must run from a clean dependency state through an evaluator-controlled runner.
The runner, wrapper, and image must come from the benchmark evaluator environment, not from mutable
submission files.

For container-based MVP evaluation, the operator should run the wrapper from an evaluator-controlled
checkout or published runner image and pass the submitted project path to that wrapper. The wrapper
should copy the submitted project to a disposable writable workspace before mounting it in the
container, so generated lockfiles, `node_modules`, and other runner artifacts do not mutate the
original submission evidence.

The official flow is:

1. validate dependency policy against the final `package.json`
2. generate a fresh `package-lock.json` with lifecycle scripts disabled
3. validate the generated lockfile
4. remove or ignore existing `node_modules` and generated dependency artifacts
5. install with npm using lifecycle scripts disabled
6. run the documented test command when present
7. run the documented build command when present
8. run a bounded preview readiness check
9. when manual review is needed, start the documented preview or dev server in serve mode

The runner may allow network access during build, test, and preview. It must not expose the host
home directory, credentials, broad host filesystem paths, Docker socket, or secret environment
variables to submitted project commands. If the model had uncontrolled Docker or host-level access
during implementation, official evaluation should use a VM, disposable host, or equivalent
operator-controlled environment.

Runner rejection during the implementation run is feedback the model may fix. Final dependency
policy violations are `Contract-Failing`; inability to install or start through the controlled
runner is `Unrunnable`.

## Fairness Principle

The benchmark should not assume:
- subagent support
- automatic task delegation
- infinite context windows
- automatic commit generation
- strong Polish-language conversation ability from every implementation model

A model that works through one main chat plus human-created continuation windows should remain fully
eligible for comparison.

## Reviewer Expectations

Reviewers should understand that:
- commit history may exist, but is optional
- fresh-window continuation is allowed
- the main evaluation target is the final repository output and visible repository evidence
- process evidence should be judged from durable artifacts, not from assumptions about hidden chat
  state
