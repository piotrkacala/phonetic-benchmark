# AGENTS.md

## Project Overview

This repository is a benchmark package for AI-assisted implementation of a phonetic training web
application.
Its purpose is to let different models and agent workflows implement the same product brief from
the same starting point.

## Key Invariants

- The canonical benchmark data is stored in `benchmark-data/`.
- The Polish and NATO phonetic alphabets must remain in the repository.
- The fixed multiple-choice suggestion sets must remain in the repository.
- The baseline repository should contain docs and benchmark data only, not a canonical starter
  shell.
- The benchmark must not require React or any other specific frontend framework.
- The implementation target may be constrained to web technology built and run through Node.js.
- Durable benchmark rules belong in versioned markdown files, not only in chat history.

## Start Here

Read these first before making significant changes:
- `agent-collaboration-defaults.md` — shared collaboration defaults for this repo
- `BENCHMARK.md` — benchmark intent and framing
- `workflow.md` — local workflow for maintaining and running the benchmark package
- `docs/DEPENDENCY_SECURITY.md` — dependency and runner security policy
- `benchmark-data/alphabets.json` — canonical alphabet data
- `benchmark-data/multiple-choice-options.json` — canonical suggestion-mode option sets

## Commands

### Development

No canonical development command exists yet.
If a runnable stack is introduced, document it here in the same change.

### Quality

No canonical quality command exists yet.
If linting or validation is introduced, document it here in the same change.

### Testing

No canonical test command exists yet.
If automated tests are introduced, document them here in the same change.

### Production

No canonical production build command exists yet.
If a benchmark runner or shell is introduced, document it here in the same change.

## Repository Structure

- `benchmark-data/` — framework-neutral benchmark data that must stay stable
- `docs/` — durable benchmark docs and source-of-truth specs
- `docs/ai/` — operational artifacts for agent workflow
- `BENCHMARK.md` — current benchmark framing note
- `workflow.md` — local workflow bootstrap for this repository

## Architecture Notes

- The baseline repository is intentionally decoupled from any React shell.
- The benchmark package should stay docs-first and data-first, without a canonical implementation
  in the baseline.
- The benchmark package should define constraints and data without biasing the implementation
  toward one framework.
- During an implementation run, adding a runnable application is expected. The "no canonical
  implementation" rule applies to the shared baseline, not to benchmark submissions.

## Workflow Rules

- Read the relevant benchmark doc before changing a corresponding contract.
- Preserve benchmark stability by recording durable decisions in repo files.
- Do not reintroduce framework-specific assumptions unless the benchmark explicitly requires them.
- Do not add a canonical starter app to the baseline unless the benchmark version explicitly
  changes scope.
- Keep the repo self-contained. Do not leave durable references to files outside this repository.
- Treat dependency installation and project scripts as untrusted execution. Follow
  `docs/DEPENDENCY_SECURITY.md` instead of adding ad hoc package-manager rules.
- For implementation submissions, do not add `.npmrc`, non-registry dependency specs, or install
  lifecycle scripts such as `preinstall`, `install`, `postinstall`, or `prepare`.
- Use documented benchmark runner commands when available. Final evaluation reruns install, build,
  test, and preview in a controlled environment.

## Quality Gates

Before considering a benchmark-structure change done:
- confirm the canonical data still exists in `benchmark-data/alphabets.json`
- confirm repo instructions do not reference external local paths
- confirm durable decisions are reflected in markdown, not only in chat
