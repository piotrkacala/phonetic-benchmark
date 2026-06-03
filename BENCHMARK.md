# Phonetic Benchmark

## Purpose

This repository defines a stable benchmark for AI-assisted implementation.

Benchmark author:
- Piotr Kacała
- piotrkacala.pl

The goal is not to preserve one preferred product implementation. The goal is to create a repeatable test case that different AI models, agents, and workflows can implement from the same product brief and supporting documents.

The benchmark should make it possible to compare how different systems:

- read and preserve requirements
- fill in missing implementation details
- make product and UX decisions within bounded freedom
- handle testing discipline and regression risk
- differ in completeness, structure, and judgment

## Scientific Approach

This benchmark is meant to be treated more like a controlled experiment than a normal side project.

Core principles:

- The benchmark brief should be stable and versioned.
- Different models should receive the same benchmark inputs.
- Evaluation criteria should stay fixed within a benchmark version.
- Changes to the benchmark should create a new benchmark version, not silently modify the old one.
- Stack, styling, and implementation choices may vary unless explicitly constrained by the benchmark documents.

The purpose is not to declare a universal winner. The purpose is to observe meaningful differences in how systems interpret the same problem.

## Repository Role

This repo contains:

- benchmark documents describing the product and expected behavior
- benchmark data describing the phonetic alphabets and other stable fixtures
- workflow instructions for running the benchmark
- evaluation criteria for comparing outputs across models
- submission-artifact requirements for completed implementation runs

The baseline repository does not ship a canonical starter implementation shell.
During a benchmark run, an implementation model is expected to add its own runnable application
structure as part of the submission.

## Benchmark Design Intent

The benchmark should specify:

- product goals
- required user-facing behavior
- content and domain rules
- testable functional constraints
- expected workflows for running the exercise
- evaluation dimensions

The benchmark should avoid prescribing:

- frontend stack
- styling direction
- color choices
- visual polish decisions
- unnecessary architectural preferences

The benchmark may constrain the implementation to a web technology stack that is built and run from
Node.js, but it should not require React or any other specific frontend framework.

The point is to keep the product definition stable while leaving room for models to reveal their own implementation judgment.

## Why Phonetic

This project is a good benchmark candidate because it is small enough to rerun often, but rich enough to expose real differences.

It includes:

- domain rules
- multiple exercise modes
- validation and scoring logic
- state transitions
- UX tradeoffs
- testability concerns

That makes it more useful than a toy CRUD task and less noisy than a large production system.

## Current Benchmark Package

The benchmark package is defined by the versioned repository documents and benchmark data.

Current document set:

- `docs/VERSION.md`
- `docs/OVERVIEW.md`
- `docs/REQUIREMENTS.md`
- `docs/WORKFLOW.md`
- `docs/ALLOWED_FREEDOMS.md`
- `docs/EVALUATION.md`
- `docs/REVIEW_RUBRIC.md`
- `docs/TEST_CASES.md`
- `docs/DECISIONS.md`

The current benchmark version is `v2`.

## Benchmark Data

The canonical data that must remain in the repository is:

- the Polish phonetic alphabet
- the NATO phonetic alphabet
- the fixed multiple-choice suggestion sets

That data should live in a framework-neutral format using the field names `symbol` and `codeword`.
