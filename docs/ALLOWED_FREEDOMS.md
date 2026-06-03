# Allowed Freedoms

## Purpose

This document defines where implementations may differ without violating the benchmark.

The benchmark is intentionally specific about required behavior and intentionally open about many
implementation details.
The point is to preserve a stable product contract while still allowing meaningful differences in
engineering judgment.

## Rule Of Interpretation

When a detail is not fixed by the benchmark docs:
- the implementing model should make a reasonable decision
- the decision should stay internally consistent
- the decision should not contradict the benchmark contract
- the decision should be explainable in repository artifacts, not only in chat

## Not Flexible

The following items are fixed by the benchmark and are not implementation freedoms:
- the product is a web application
- the project uses a Node.js-based workflow
- the canonical benchmark data lives in `benchmark-data/`
- supported phonetic alphabets are Polish and NATO only
- supported interface languages are Polish and English only
- the application has exactly two required exercise modes: `keyboard` and `suggestion`
- input modes may not be mixed within a single run
- symbols are shown in randomized order
- suggestion mode uses fixed option data from the repository
- hint reveals the answer but does not auto-complete the question
- final scoring follows the documented hint-based formula

## Implementation Freedoms

The following choices are intentionally left open:
- frontend framework or no framework
- client rendering versus server rendering
- SPA versus multi-page architecture
- routing approach
- component structure
- state-management approach
- styling approach
- CSS architecture
- animation and motion choices
- visual design direction
- exact page layout
- exact copy tone, as long as required meaning is preserved in both languages
- i18n library or translation-file structure
- how randomized order is implemented internally
- how shuffled suggestion-button order is implemented internally
- test framework and test structure

## UX Freedoms

The benchmark does not prescribe:
- a specific onboarding flow
- a specific visual hierarchy on the setup screen
- a specific look for the result screen
- whether progress is shown as text, bar, fraction, or multiple indicators
- whether wrong answers show inline feedback, passive non-advancement, or another clear response
- whether the app includes optional convenience features such as replay, restart, or navigation
  shortcuts

Any extra UX behavior is allowed if it does not:
- obscure the required benchmark flows
- change the scoring contract
- break mode separation
- conflict with fixed data or fixed domain rules

## Technical Freedoms

Implementations may differ in:
- project structure
- naming conventions
- data-loading strategy
- use of TypeScript or plain JavaScript
- bundler choice
- test-runner choice
- package manager choice
- deployment assumptions

The benchmark does not require:
- a database
- authentication
- server-side persistence
- analytics
- user accounts
- backend APIs, unless an implementation chooses them voluntarily

## Open Questions The Model Must Close

The current benchmark version intentionally leaves some product details open.
Implementations must close them coherently.

Open items currently include:
- reset behavior during or after a run
- whether interface-language switching is allowed during an active run

Those decisions are implementation freedoms, but they are reviewable freedoms.
Review should check whether they were closed clearly and coherently.

## Extra Features

Extra features are allowed, but they are secondary.

Examples of acceptable extras:
- restart button
- progress history within the current session
- optional transition effects
- richer result breakdown
- accessibility enhancements beyond the benchmark minimum

Examples of risky extras:
- features that change how score is computed
- features that bypass answering the current question
- features that blur the boundary between keyboard mode and suggestion mode
- features that replace fixed project data with generated runtime content

## Documentation Expectation

If an implementation makes a non-trivial choice in an open area, it should document that choice in
repository artifacts.
The benchmark should remain reviewable without depending on chat history alone.
