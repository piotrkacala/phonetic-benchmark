# Benchmark Version

Current benchmark version:
- `v1`

## Meaning

`v1` is the first repository-defined benchmark version for this package.

It refers to the benchmark contract defined by the current repository docs and benchmark data,
including:
- product requirements
- benchmark data files
- test cases
- allowed freedoms
- evaluation rules
- workflow expectations

## Versioning Rule

Changes that materially affect comparison should create a new benchmark version instead of silently
rewriting `v1`.

Examples of version-changing modifications:
- changing required product behavior
- changing scoring rules
- changing benchmark datasets
- changing suggestion-option data
- changing evaluation criteria in a way that affects comparability

Minor clarifications that do not materially change the benchmark may remain within the same
version.
