# Decisions

## Purpose

This document records durable benchmark decisions that have already been made.

It is not a backlog.
It is a record of choices that should prevent the benchmark from drifting over time.

Current benchmark version:
- `v2`

## Decision 1: Baseline Repository Scope Is Docs And Data Only

Status:
- accepted

Decision:
- the baseline repository contains benchmark docs and benchmark data only
- it does not ship a canonical starter implementation shell

Why:
- a starter app would bias the benchmark toward one structure and one implementation path
- the benchmark should compare how systems build from the same brief, not how they edit one
  preselected shell

Implications:
- submissions should bring their own implementation structure during benchmark runs
- the benchmark repo remains focused on product contract, test cases, and evaluation

## Decision 2: The Product Target Is A Web App With A Node.js-Based Workflow

Status:
- accepted

Decision:
- the finished deliverable must be a web application
- the project workflow must be based on Node.js

Why:
- this keeps the benchmark inside a consistent implementation category
- it avoids submissions that solve the task in Python or another unrelated runtime shape
- it still leaves wide freedom in frontend and architecture choices

Implications:
- framework choice remains open
- runtime and tooling category is constrained loosely, not narrowly

## Decision 3: React Is Not A Benchmark Requirement

Status:
- accepted

Decision:
- the benchmark must not require React or any other specific frontend framework

Why:
- the benchmark should measure implementation judgment, not compliance with an arbitrary framework
  preference
- forcing React would reduce the value of cross-system comparison

Implications:
- implementations may use React, Vue, Svelte, plain JavaScript, or another suitable web-stack
  approach
- review should not reward or penalize framework choice by itself

## Decision 4: Canonical Data Lives In Framework-Neutral Files

Status:
- accepted

Decision:
- the benchmark's canonical data lives in `benchmark-data/`
- the phonetic alphabets are stored in `benchmark-data/alphabets.json`
- fixed suggestion-mode options are stored in `benchmark-data/multiple-choice-options.json`

Why:
- the benchmark needs a durable, implementation-agnostic source of truth
- framework-neutral files reduce accidental coupling to one codebase layout
- suggestion-mode determinism requires fixed repository data

Implications:
- implementations must read or mirror the benchmark data faithfully
- review can check behavioral correctness against stable source files

## Decision 5: Alphabet Data Uses `symbol` And `codeword`

Status:
- accepted

Decision:
- alphabet entries use the field names `symbol` and `codeword`

Why:
- the old field names were implementation-shaped rather than domain-shaped
- `symbol` and `codeword` express the domain more clearly and travel better across stacks

Implications:
- tests and implementations should align with this schema
- future benchmark docs should use the same terminology

## Decision 6: Supported Alphabets Are Polish And NATO Only

Status:
- accepted

Decision:
- benchmark `v2` supports exactly two phonetic alphabets:
  - Polish
  - NATO

Why:
- these are the stable benchmark datasets currently defined in the repository
- limiting scope keeps the benchmark compact and repeatable

Implications:
- adding more alphabets would require a benchmark-version change
- evaluation should not expect broader dataset support in `v2`

## Decision 7: The Interface Must Support Polish And English

Status:
- accepted

Decision:
- the application must provide full interface support in Polish and English

Why:
- this introduces a meaningful but bounded i18n requirement
- it increases benchmark value beyond a single-language toy task

Implications:
- user-visible interface strings are part of the implementation burden
- review should treat missing i18n as a contract failure, not a cosmetic issue

## Decision 8: The Benchmark Requires Two Separate Exercise Modes

Status:
- accepted

Decision:
- the required exercise modes are:
  - keyboard mode
  - suggestion mode
- those input methods must remain separate within a single run

Why:
- the product should test more than one interaction pattern
- allowing mixed-input runs would weaken the benchmark contract and complicate scoring review

Implications:
- implementations must model mode selection explicitly
- review should verify that runs cannot drift between the two input methods

## Decision 9: Suggestion Mode Uses Fixed Repository Data

Status:
- accepted

Decision:
- suggestion mode uses four options per question
- the options come from `benchmark-data/multiple-choice-options.json`
- they are not generated ad hoc at runtime

Why:
- runtime generation would make cross-submission comparison less stable
- fixed option sets improve repeatability and reviewability

Implications:
- implementations should not invent option lists dynamically
- reviewers can inspect both data and behavior deterministically

## Decision 10: Suggestion Options Follow Same-Letter Rules With Explicit Exceptions

Status:
- accepted

Decision:
- for non-exception symbols, the correct option and all three incorrect options must start with the
  same letter as the target symbol
- Polish exceptions are explicitly handled in benchmark data
- current exception symbols are:
  - `Ą`
  - `Ę`
  - `Ń`

Why:
- same-letter distractors make the multiple-choice mode more meaningful
- a few Polish letters do not work well under that rule without artificial data
- explicit exceptions are better than hidden logic

Implications:
- implementations must trust the benchmark data for exception handling
- reviewers should judge behavior against the fixed data, not against inferred rules alone

## Decision 11: Symbol Order And Suggestion Order Are Randomized

Status:
- accepted

Decision:
- symbols are presented in randomized order during a run
- the display order of the four suggestion options is also randomized

Why:
- fixed order would reduce exercise value and make the benchmark less realistic
- randomization still allows freedom in implementation while preserving the required behavior

Implications:
- implementations must include randomness in both places
- tests should verify the contract without depending on one exact static order

## Decision 12: Hint Reveals The Answer But Does Not Complete The Question

Status:
- accepted

Decision:
- hint reveals the correct answer for the current question
- the hint remains visible until that question is completed
- the question is not auto-completed
- the user must still type or click the correct answer manually

Why:
- this preserves learning intent and prevents hint from becoming a skip mechanic
- it also supports deterministic scoring based on hint usage

Implications:
- implementations must separate `hint shown` from `question completed`
- tests should verify both reveal behavior and required manual completion

## Decision 13: Final Score Depends Only On Hint Usage

Status:
- accepted

Decision:
- final score is computed from the number of clean versus hinted questions
- wrong attempts without using a hint do not directly lower the score

Why:
- the original scoring logic was not clearly remembered
- the benchmark needs a simple, deterministic scoring rule that every implementation can share
- hint-based scoring is easy to specify and easy to test

Implications:
- implementations must follow the documented formula exactly
- evaluation can compare outputs without reconstructing hidden scoring heuristics

## Decision 14: Open Product Gaps Must Be Closed By The Implementing Model

Status:
- accepted

Decision:
- if the benchmark leaves a product or UX detail unspecified, the implementing model must close the
  gap with a reasonable, coherent decision

Why:
- the benchmark is supposed to reveal judgment, not only rote execution
- forcing clarification for every small gap would reduce benchmark usefulness

Implications:
- open questions remain reviewable rather than ignored
- reviewers should inspect whether self-closed decisions are coherent and visible in repository
  artifacts

## Decision 15: Evaluation Has Two Layers

Status:
- accepted

Decision:
- benchmark evaluation has two layers:
  - contract verification
  - comparative scoring

Why:
- some submissions may be runnable but still fail the benchmark contract
- stronger submissions should be distinguishable after baseline correctness is established

Implications:
- only contract-clearing submissions should receive a full comparative score
- evaluation should balance correctness, product judgment, technical quality, testing, and
  documentation

## Decision 16: The Benchmark Requires A Git Baseline But Not Mandatory Model Commits

Status:
- accepted

Decision:
- benchmark runs must start from a repository under git with a known baseline commit
- the model is not required to create commits during the run
- the model is allowed and encouraged to create a final local commit when its environment supports
  that workflow

Why:
- reviewers need a stable baseline and inspectable final diff
- mandatory commit behavior would bias the benchmark toward tools with stronger git ergonomics

Implications:
- final repository state is required evidence
- commit history is optional evidence
- if no final commit is created, the submission should record that fact in
  `docs/ai/IMPLEMENTATION_REPORT.md`
- implementation models should not push commits to remotes unless the operator explicitly asks them
  to do so

## Decision 17: Manual Fresh-Window Handoffs Are Allowed

Status:
- accepted

Decision:
- a human operator may continue the same benchmark run in a fresh chat window
- this is allowed even when a model has no subagent support

Why:
- models and interfaces differ in context-window and orchestration capabilities
- the benchmark should compare implementation quality, not only runtime tooling features

Implications:
- benchmark workflow must support short continuation prompts
- repository artifacts should carry important state where practical
- reviewers should not treat fresh-window continuation as a process failure

## Decision 18: Prompts Must Not Add Hidden Requirements

Status:
- accepted

Decision:
- prompts may frame the current task and point to repository docs
- prompts must not add hidden product requirements or private scoring rules outside the repo

Why:
- otherwise different runs would receive materially different benchmark definitions
- the benchmark should be fair, reviewable, and reproducible

Implications:
- repository docs remain the benchmark source of truth
- continuation prompts should be concise and operational rather than secretly product-defining

## Decision 19: The Finished Application Must Include Visible Benchmark And Model Attribution

Status:
- accepted

Decision:
- the finished application must include a visible attribution line in the UI
- it must credit Piotr Kacała and `piotrkacala.pl`
- it must also identify the implementing model and implementation date

Why:
- benchmark provenance should remain visible in the implementation itself
- review should be able to identify which model produced a given output
- this helps when screenshots, demos, or deployed outputs are compared outside raw repo context

Implications:
- implementations should include a footer or similarly visible attribution area
- review should treat missing attribution as a contract issue rather than a cosmetic omission

## Decision 20: Benchmark Operator-Model Communication Defaults To English

Status:
- accepted

Decision:
- benchmark prompts and continuation handoffs should be written in English by default
- the benchmark must not require Polish-language conversation between the operator and the
  implementation model
- this does not change the product requirement that the web application itself supports both Polish
  and English in the UI

Why:
- some models are materially weaker at Polish-language chat than at English-language chat
- forcing Polish conversation would measure language-specific prompt handling rather than product
  implementation quality
- English is the more neutral default for an open benchmark intended to compare heterogeneous models

Implications:
- operator workflow docs should assume English-language prompts unless the benchmark version says
  otherwise
- review should not penalize a run for using English-language operator prompts
- application-localization requirements remain governed by `docs/REQUIREMENTS.md`

## Decision 21: Submissions Must Include Durable Review Artifacts

Status:
- accepted

Decision:
- every completed submission must include a `README.md`
- every completed submission must include `docs/ai/IMPLEMENTATION_REPORT.md`
- those artifacts must make install, run, verification, model identity, open decisions, and process
  evidence inspectable from the repository

Why:
- chat history is often inaccessible or incomplete during review
- previous implementation runs showed that models often omit durable process notes unless the
  repository contract asks for them explicitly
- the benchmark should compare implementation output from repository evidence, not hidden context

Implications:
- missing or materially incomplete submission artifacts are contract failures
- documentation and process quality can still distinguish stronger and weaker artifacts after the
  baseline artifact contract is met

## Decision 22: Runtime Settings Should Be Recorded When Visible

Status:
- accepted

Decision:
- submissions must record the implementing model name and version
- submissions must record provider or interface information when visible
- submissions must record effort, reasoning, temperature, sampling, or similar runtime settings when
  visible to the implementation model
- unavailable settings must be recorded as `not exposed by interface` rather than guessed

Why:
- provider and runtime settings may materially affect behavior
- some model interfaces expose those settings and others do not
- guessing settings would make the artifact less reliable than explicitly marking them unavailable

Implications:
- reviewers can distinguish known run configuration from unavailable configuration
- models are not penalized for settings they cannot inspect, but they must not invent them

## Decision 23: Submission Evidence Must Match The Delivered Application

Status:
- accepted

Decision:
- the implementation date shown in attribution must be a fixed date for the benchmark run
- the implementation date must not be generated dynamically at runtime, application startup, or
  build time
- documented decisions for open product questions must match the implemented UI behavior
- if a test command is documented, it must complete successfully in the submitted repository

Why:
- previous implementation runs repeatedly produced runtime-generated attribution dates instead of
  preserved implementation dates
- some submissions documented behavior that differed from the delivered UI
- optional tests should remain optional, but a declared verification command must be truthful and
  runnable

Implications:
- models still choose how to close open product questions
- tests remain a quality signal rather than a universal hard requirement
- incorrect attribution dates, mismatched decision notes, and broken declared test workflows are
  contract failures

## Currently Open

The following topics remain intentionally open in the current benchmark version:
- reset behavior during or after a run
- whether interface-language switching is allowed during an active run

These are not accepted decisions yet.
Implementations may close them, but the benchmark package has not frozen them globally.
Submissions must record how they closed these questions in repository artifacts.
