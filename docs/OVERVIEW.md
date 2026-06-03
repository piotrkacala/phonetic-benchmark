# Overview

## What This Repository Is

This repository defines a benchmark for AI-assisted implementation of a small web application.

Benchmark author:
- Piotr Kacała
- piotrkacala.pl

The benchmark is not a finished product implementation.
It is a stable benchmark package that gives different models and agent workflows the same product
target, the same benchmark data, and the same evaluation structure.

## What The Target Application Is

The target application is a browser-based learning tool for practicing phonetic alphabets.

Its core task is simple:
- show a `symbol`
- require the user to produce the matching `codeword`

The benchmark requires support for:
- the Polish phonetic alphabet
- the NATO phonetic alphabet
- Polish and English interface languages
- a keyboard-answering mode
- a four-option suggestion mode

The finished application should also expose a visible attribution line for the benchmark and the
implementing model.

## Why This Benchmark Exists

This benchmark is intended to expose meaningful differences in how AI systems:
- read and preserve requirements
- implement a bounded product from stable inputs
- make reasonable decisions where details are intentionally left open
- handle tests, regressions, and reviewability
- produce complete, inspectable repository outputs

It is meant to be small enough to rerun often, but rich enough to reveal real differences in
judgment and execution.

## What Is Fixed

The benchmark fixes:
- the product domain
- the canonical alphabet data
- the fixed multiple-choice option data
- the required exercise modes
- the scoring rule
- the evaluation structure

These fixed parts live in versioned repository files so the benchmark stays stable and reviewable.

## What Is Intentionally Open

The benchmark does not prescribe:
- a specific frontend framework
- a specific styling direction
- a specific app architecture
- a specific test framework
- every UX detail

When something is left open, the implementing model is expected to close the gap coherently rather
than block on clarification.

## Repository Contents

This repository contains:
- benchmark documentation
- benchmark data
- workflow and evaluation guidance

The baseline repository does not contain:
- a canonical starter application
- a framework-preferred baseline shell

During an implementation run, a submission is expected to add a runnable application and supporting
project files.

## Source Documents

The main benchmark documents are:
- `docs/VERSION.md` — current benchmark version
- `docs/WORKFLOW.md` — how benchmark runs should be executed
- `docs/REQUIREMENTS.md` — required product behavior
- `docs/TEST_CASES.md` — contract tests and review checks
- `docs/ALLOWED_FREEDOMS.md` — where implementations may differ
- `docs/EVALUATION.md` — how completed submissions are evaluated
- `docs/DECISIONS.md` — durable benchmark decisions

The canonical benchmark data lives in:
- `benchmark-data/alphabets.json`
- `benchmark-data/multiple-choice-options.json`

The current benchmark version is defined in:
- `docs/VERSION.md`

## What A Submission Should Produce

A benchmark submission should produce:
- a runnable web application
- a Node.js-based project workflow
- behavior consistent with the benchmark contract
- enough repository evidence to review implementation quality

Benchmark runs may use one chat window or multiple human-created continuation windows.
The benchmark workflow explicitly allows that.

The goal is not only to make something that runs.
The goal is to make something that is comparable, reviewable, and aligned with the benchmark.

## Review Standard

Submissions are reviewed in two stages:
1. verify whether the benchmark contract is satisfied
2. compare quality across submissions that clear the contract baseline

This prevents flashy but non-compliant solutions from being treated as benchmark successes.
