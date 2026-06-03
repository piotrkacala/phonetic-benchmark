# Phonetic Benchmark

This repository defines a benchmark for AI-assisted implementation of a small web application.

Benchmark author:
- Piotr Kacała
- piotrkacala.pl

The benchmark is intentionally not tied to React or any other frontend framework.
Implementations may use any web stack as long as it is built and run within a Node.js-based workflow.

## Repository Role

- hold the benchmark definition and workflow
- keep the benchmark data in-repo
- avoid accidental bias toward one frontend stack
- avoid shipping a canonical starter implementation shell in the baseline package

## Working Modes

This repository supports two valid working modes:

- benchmark-maintenance mode: update the benchmark docs, data, workflow, or evaluation rules
- implementation-run mode: add a runnable Node.js-based web application as a benchmark submission

The baseline package intentionally contains docs and benchmark data only.
That does not mean implementation agents should refuse the task.
During an implementation run, the expected work is to add the application code, project workflow,
and verification artifacts needed for a complete submission.

## Submission Artifacts

A completed benchmark submission must leave durable repository evidence, including:
- a runnable Node.js-based application
- a `README.md` with install, run, and verification instructions
- a `docs/ai/IMPLEMENTATION_REPORT.md` file describing the implementation run
- explicit notes for the implementation's decisions on the benchmark's open product questions
- any model/provider/runtime settings visible to the implementation model
- a fixed implementation date in visible attribution

## Canonical Data

The canonical benchmark data currently lives in:
- [benchmark-data/alphabets.json](benchmark-data/alphabets.json)
- [benchmark-data/multiple-choice-options.json](benchmark-data/multiple-choice-options.json)

It contains the two phonetic alphabets and the fixed multiple-choice suggestion sets used by the
benchmark:
- Polish phonetic alphabet
- NATO phonetic alphabet
- fixed 4-option answer sets for suggestion mode

Each entry uses the neutral field names `symbol` and `codeword`.

## Local Workflow Files

This repository keeps its own local workflow bootstrap:
- `AGENTS.md`
- `CODEX.md`
- `agent-collaboration-defaults.md`
- `workflow.md`

These files are copied and adapted locally so the repo stays self-contained.
