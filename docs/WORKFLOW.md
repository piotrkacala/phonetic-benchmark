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

When the current task is an implementation run, the model should:
- add a runnable browser-based application
- add the Node.js project workflow needed to install and run it
- add tests or verification artifacts when appropriate
- preserve the benchmark docs and canonical data as the source of truth

The rule against a canonical starter shell applies to the shared baseline package.
It does not prohibit implementation code in benchmark submissions.

## Git Policy

The repository must be under git.

Hard requirement:
- the run starts from a known baseline commit

Recommended:
- keep the implementation history if it is convenient in the chosen tool
- use logical commits when the workflow supports them cleanly

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
- notes in docs
- task or summary files if the workflow chooses to create them

This reduces dependence on one model's hidden chat memory.

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
