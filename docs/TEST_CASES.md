# Test Cases

## Purpose

This document defines how benchmark outputs should be checked.

The benchmark uses two layers of evaluation:
- contract tests
- process and quality review

Contract tests verify whether the implementation satisfies the required product behavior.
Process and quality review compares how well the implementation work was carried out.

## Layer 1: Contract Tests

These tests target observable behavior.
An implementation should be considered incomplete if it fails any must-pass contract test.

### Data Contract

1. The repository contains `benchmark-data/alphabets.json`.
2. The repository contains `benchmark-data/multiple-choice-options.json`.
3. `alphabets.json` uses the fields `symbol` and `codeword`.
4. `multiple-choice-options.json` contains fixed option sets for both supported alphabets.
5. Suggestion-mode answer sets are read from project data and are not generated ad hoc.

### Platform Contract

1. The deliverable is a web application.
2. The project uses a Node.js-based workflow.
3. The implementation does not depend on React specifically unless chosen voluntarily.
4. The implementation can be installed and run with documented Node.js commands.
5. The submission includes a `package.json`.
6. The submission includes a `README.md`.
7. The reviewer does not need to guess the basic install and run steps.

### Submission Artifact Contract

1. `README.md` documents install commands.
2. `README.md` documents run commands.
3. `README.md` documents verification or test commands when automated tests are included.
4. `README.md` identifies the chosen stack and package manager.
5. `README.md` identifies the benchmark version implemented.
6. `README.md` identifies the implementing model name and version.
7. `README.md` records the implementation's decisions for the benchmark's open product questions.
8. The documented open-question decisions match the implemented UI behavior.
9. If a test command is documented, it completes successfully in the submitted repository.
10. `docs/ai/IMPLEMENTATION_REPORT.md` exists.
11. `docs/ai/IMPLEMENTATION_REPORT.md` records visible model/provider/runtime settings or marks
   unavailable settings as `not exposed by interface`.
12. `docs/ai/IMPLEMENTATION_REPORT.md` records commands run and verification results.
13. `docs/ai/IMPLEMENTATION_REPORT.md` records whether the model created a final git commit.

### Attribution Contract

1. The UI includes a visible attribution line.
2. The attribution credits Piotr Kacała.
3. The attribution includes `piotrkacala.pl`.
4. The attribution includes the implementing model name and version.
5. The attribution includes an implementation date.
6. The implementation date is fixed for the benchmark run and is not generated dynamically at
   runtime, application startup, or build time.
7. The attribution is visible in normal use and does not require hidden or debug-only UI.

### Language And Localization

1. The interface supports Polish.
2. The interface supports English.
3. All user-visible UI strings are translated in both languages.
4. Switching interface language updates visible UI copy consistently outside any explicitly open
   product question.

### Setup Flow

1. The user can choose an interface language.
2. The user can choose the phonetic alphabet.
3. The user can choose an exercise mode.
4. The available exercise modes are exactly `keyboard` and `suggestion`.
5. Starting a run produces one active exercise session with the chosen alphabet and mode.

### Run Progression

1. A run uses the full selected alphabet exactly once per symbol.
2. Symbols are presented in randomized order.
3. The user cannot finish the run early without completing every symbol.
4. A correct answer advances to the next symbol.
5. A wrong answer does not advance to the next symbol.
6. The implementation's tests or verification approach do not rely on one fixed symbol order.

### Keyboard Mode

1. Keyboard mode accepts manual text input.
2. Leading whitespace is ignored.
3. Trailing whitespace is ignored.
4. Matching is case-insensitive.
5. Diacritics remain significant.
6. `żaba` is accepted for `ŻABA`.
7. `zaba` is rejected for `ŻABA`.
8. After a correct answer, the current question ends and the next symbol is shown.

### Suggestion Mode

1. Suggestion mode renders exactly four answer buttons for each question.
2. Exactly one option is correct.
3. Exactly three options are incorrect.
4. The option set comes from `benchmark-data/multiple-choice-options.json`.
5. For non-exception symbols, all four options start with the same letter as the target symbol.
6. Polish exceptions are handled according to fixed benchmark data.
7. The visual order of the four options is randomized.
8. Clicking a wrong option does not advance the run.
9. Clicking the correct option advances the run.
10. The implementation's tests or verification approach do not rely on one fixed option order.

### Mode Separation

1. A run started in keyboard mode cannot be completed through multiple-choice clicks.
2. A run started in suggestion mode cannot be completed through free-text typing.
3. The implementation preserves the separation between the two input modes for the full run.

### Hint Behavior

1. Each active question exposes a hint action.
2. Triggering hint reveals the correct answer for the current question.
3. Revealing the hint does not auto-complete the question.
4. The hint stays visible until the current question is completed.
5. In keyboard mode, the user must still type the correct answer after seeing the hint.
6. In suggestion mode, the user must still click the correct option after seeing the hint.
7. In suggestion mode, the revealed hint is visible as `codeword` text.
8. In suggestion mode, the hint does not identify the correct button by auto-selection or marking.

### Final Scoring

1. The application calculates a final percentage score.
2. The score uses the documented rule based on hinted versus clean questions.
3. A hinted question counts as not clean even if the final answer is correct.
4. Wrong attempts without hint do not directly reduce the score.
5. A run with zero hinted questions results in `100%`.
6. A run with one hinted question out of 26 results in `96%`.
7. A run with five hinted questions out of 26 results in `81%`.
8. A run with two hinted questions out of 34 results in `94%`.

### Result Screen

1. Completing the final question shows a result screen.
2. The result screen clearly indicates that the run is finished.
3. The result screen shows the final percentage score.
4. The result screen shows which alphabet was used.
5. The result screen shows which mode was used.

## Layer 2: Process And Quality Review

These checks are comparative rather than purely binary.
They should be used to evaluate how strong the implementation work is, not only whether the app
basically works.

### Requirement Preservation

Review whether the implementation:
- preserved the benchmark contract without silently dropping behaviors
- respected the fixed benchmark data instead of replacing it with convenient shortcuts
- handled open questions with explicit, coherent decisions
- avoided inventing new hard requirements not present in the benchmark

### Product Judgment

Review whether the implementation:
- makes setup and run flow understandable without extra explanation
- handles wrong answers clearly
- makes mode separation understandable to the user
- makes hint behavior understandable before and after use
- presents result-state information clearly enough to support benchmark review

### Technical Judgment

Review whether the implementation:
- uses a coherent Node.js-based workflow
- chooses a reasonable web stack for the benchmark
- keeps data loading, state updates, and scoring logic understandable
- avoids unnecessary coupling to one framework-specific pattern when not needed
- keeps the codebase small and reviewable for a benchmark-sized task

### Testing Discipline

Review whether the implementation:
- includes meaningful automated tests for the core flows
- covers scoring logic and input-validation edge cases
- covers both exercise modes
- covers localization-sensitive behaviors where practical
- uses test cases that correspond to benchmark requirements instead of generic boilerplate
- handles randomized behavior in a testable, non-flaky way

### Regression Risk

Review whether the implementation:
- leaves obvious edge cases unhandled
- contains hidden assumptions that would likely fail under another alphabet or locale
- relies on unstable randomness without testable structure
- hardcodes UI behavior in ways that contradict the benchmark contract

### Documentation And Process Quality

Review whether the implementation:
- documents how to install, run, and verify the project
- explains any important self-closed decisions for open questions
- records visible model, provider, and runtime settings without guessing unavailable settings
- records whether the model created a final git commit
- keeps reasoning traceable enough that another reviewer can understand the tradeoffs
- produces an output that is inspectable without relying on chat history alone

## Open Questions

The following items remain intentionally open in the current benchmark version:
- reset behavior during or after a run
- whether interface-language switching is allowed during an active run

Implementations must close these gaps coherently.
Review should assess whether those decisions are reasonable and internally consistent.
