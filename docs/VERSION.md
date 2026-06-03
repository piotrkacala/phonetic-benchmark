# Benchmark Version

Current benchmark version:
- `v2`

## Meaning

`v2` is the current repository-defined benchmark version for this package.

It refers to the benchmark contract defined by the current repository docs and benchmark data,
including:
- product requirements
- benchmark data files
- test cases
- allowed freedoms
- evaluation rules
- workflow expectations
- required submission artifacts

## Version History

### v2

`v2` keeps the same product behavior as `v1` and adds clearer submission-artifact requirements:
- a submission README
- an implementation report in `docs/ai/`
- visible recording of model/provider settings when available
- explicit recording of the implementation's decisions for open product questions
- explicit permission and guidance for using git commits during implementation
- a fixed implementation date instead of runtime-generated attribution dates
- a requirement that documented open-question decisions match implemented behavior
- a requirement that documented test commands complete successfully

### v1

`v1` is the first repository-defined benchmark version for this package.

## Versioning Rule

Changes that materially affect comparison should create a new benchmark version instead of silently
rewriting a released benchmark version.

Examples of version-changing modifications:
- changing required product behavior
- changing scoring rules
- changing benchmark datasets
- changing suggestion-option data
- changing evaluation criteria in a way that affects comparability

Minor clarifications that do not materially change the benchmark may remain within the same
version.
