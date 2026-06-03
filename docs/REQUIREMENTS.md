# Requirements

## Purpose

This benchmark defines the required behavior for a small phonetic training web application.
It is intended to be implemented by different AI systems from the same data and the same product
brief.

## Product Definition

The finished deliverable is a browser-based learning application for practicing phonetic alphabets.

Benchmark author:
- Piotr Kacała
- piotrkacala.pl

Its core job is to help a user train the mapping:
- from a displayed `symbol`
- to the correct `codeword`

The application must support:
- the Polish phonetic alphabet
- the NATO phonetic alphabet
- Polish and English interface languages
- a keyboard-answering mode
- a four-option suggestion mode

Each completed run should let the user practice one full selected alphabet and then see a final
score.

## Attribution Requirement

The finished application must display a visible attribution line in the UI.

Preferred placement:
- footer

Equivalent placement is allowed if it is clearly visible in normal use and does not require hidden
debug UI or source inspection.

Required content:
- benchmark attribution to Piotr Kacała
- the website `piotrkacala.pl`
- the implementing model name and version
- the implementation date

Recommended wording:
- `Phonetic Benchmark by Piotr Kacała (piotrkacala.pl). Developed by {model_name_and_version} on {implementation_date}.`

Formatting may vary slightly, but the meaning must be preserved.

## Delivery Constraints

- The deliverable must be a web application.
- The implementation must use a Node.js-based project workflow.
- The benchmark must not require React or any other specific frontend framework.
- The baseline repository contains docs and benchmark data only. It does not provide a canonical
  starter implementation.
- During a benchmark implementation run, the submission must add the application code and project
  workflow needed to satisfy this contract.
- If the benchmark leaves a product or UX detail unspecified, the implementing model must close
  that gap with a reasonable, coherent decision instead of blocking on clarification.

## Submission Contract

At minimum, a submission must include:
- a `package.json`
- documented install commands
- documented run commands
- documented test commands if automated tests are included

The reviewer should not have to guess how to install, run, or verify the project.

## Canonical Data

The benchmark data in `benchmark-data/` is part of the contract.

Required files:
- `benchmark-data/alphabets.json`
- `benchmark-data/multiple-choice-options.json`

`alphabets.json` defines the supported phonetic alphabets using the fields:
- `symbol`
- `codeword`

`multiple-choice-options.json` defines the fixed option sets used in suggestion mode.
Those options must be read from project data and must not be generated ad hoc at runtime.

## Supported Content

The application must support exactly these phonetic alphabets:
- Polish phonetic alphabet
- NATO phonetic alphabet

The application must support exactly these interface languages:
- Polish
- English

All user-visible interface strings must be available in both interface languages.

## Main User Flow

The application must allow the user to:
1. choose the interface language
2. choose the phonetic alphabet
3. choose an exercise mode
4. complete one full exercise run
5. see a final result screen

The benchmark requires two separate exercise modes:
- keyboard mode
- suggestion mode

The user must not mix those input methods within a single run.
If the user wants to switch modes, the application should require leaving or restarting the run.

## Exercise Run Behavior

For each run:
- the full chosen alphabet must be used
- symbols must be presented in randomized order
- each symbol appears once per run
- the user progresses only after selecting or entering the correct codeword
- the run ends only after all symbols in the chosen alphabet have been completed

## Keyboard Mode

In keyboard mode:
- the application presents one symbol at a time
- the user answers by typing the matching codeword
- leading and trailing whitespace must be ignored
- answer matching must be case-insensitive
- diacritics must remain significant and must not be stripped during comparison
- the correct answer must be accepted when entered with Polish characters intact

Examples:
- ` adam ` must match `ADAM`
- `żaba` must match `ŻABA`
- `zaba` must not match `ŻABA`

## Suggestion Mode

In suggestion mode:
- the application presents one symbol at a time
- the user answers by clicking one of exactly four buttons
- each question must include one correct option and three incorrect options
- the option sets must come from `benchmark-data/multiple-choice-options.json`
- the option sets must not be generated dynamically at runtime

For symbols that normally have a first-letter match:
- the correct option and all three incorrect options must begin with the same letter as the target
  symbol

For a small set of Polish exceptions where that rule is not practical:
- the exception must be handled by the fixed project data
- the implementation must use the exception options exactly as defined in the project data

The benchmark data currently treats these Polish symbols as exceptions:
- `Ą`
- `Ę`
- `Ń`

The display order of the four options must be randomized.
The option content itself is fixed by the benchmark data.

## Hint Behavior

The application must provide a hint action during a run.

When the user triggers a hint:
- the correct answer for the current symbol is revealed
- the revealed hint remains visible until the current question is completed
- the current question is not auto-completed
- the user must still submit the correct answer manually
- in keyboard mode, the user must still type the correct answer
- in suggestion mode, the user must still click the correct option
- in suggestion mode, the hint must reveal the correct `codeword` as visible text
- in suggestion mode, the hint must not mark, auto-select, or auto-submit the correct option

## Final Scoring

The benchmark uses a deliberately simple deterministic scoring rule.

Definitions:
- `total_questions` = number of symbols in the selected alphabet
- `hinted_questions` = number of questions for which the user used the hint at least once
- `clean_questions` = `total_questions - hinted_questions`

Final score:
- `score_percent = round((clean_questions / total_questions) * 100)`

Scoring rules:
- using a hint marks that question as not clean
- repeated wrong attempts without using a hint do not reduce the score directly
- a hinted question still has to be answered correctly to finish the run

Examples:
- 26 total questions, 0 hinted => `100%`
- 26 total questions, 1 hinted => `96%`
- 26 total questions, 5 hinted => `81%`
- 34 total questions, 2 hinted => `94%`

## Result Screen

After the run is complete, the application must show a final result screen.

At minimum it must show:
- the final percentage score
- enough context to make clear that the run is finished
- which alphabet and which mode were used for the completed run

## Non-Requirements

The benchmark does not prescribe:
- a specific frontend framework
- a specific visual design direction
- a specific state-management approach
- a specific routing approach
- a specific styling solution

## Review Expectations

Evaluation should consider:
- correctness against these requirements
- completeness of the implemented behavior
- quality of the final output
- quality of the implementation process and supporting reasoning
