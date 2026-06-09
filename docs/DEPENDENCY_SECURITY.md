# Dependency Security

Status: accepted temporary MVP policy.

This document defines the temporary dependency and runner security policy for Phonetic Benchmark
implementation runs.

It is the policy reference linked from `AGENTS.md`, `docs/WORKFLOW.md`, `docs/EVALUATION.md`, and
`docs/REVIEW_RUBRIC.md` for the dependency security MVP.

## Normative Language

In this document:

- `must` and `must not` define benchmark requirements
- `may` defines allowed behavior
- `should` defines implementation guidance that does not by itself decide pass or fail status

## Purpose

The benchmark asks implementation models to create a runnable Node.js-based web application from a
docs-only baseline.

That means a model may add dependencies, edit `package.json`, generate lockfiles, and run project
commands. Dependency installation and project scripts must therefore be treated as untrusted
execution.

This policy has three goals:

- keep implementation freedom where it matters for product and engineering judgment
- block dependency sources and install behavior that are hard to review
- prevent submitted projects from reading host user credentials during install, build, test, or
  preview

## Threat Model

The main risk for this benchmark is credential discovery from the host user's environment.

Sensitive host material includes:

- SSH keys
- npm tokens
- git credentials
- cloud credentials
- browser profiles
- shell history
- local config files
- private project directories
- secret environment variables

The benchmark runner must not expose the host user's home directory, credentials, tokens, or secret
environment variables to submitted project commands.

The security boundary is credential and host filesystem isolation. It is not offline execution.

## Enforcement Principle

Model instructions are not the security boundary.

The implementation model should be told about this policy, but final evaluation must enforce it
through a controlled runner.

If the model ignores the policy:

- rejected dependencies may be corrected during the implementation run
- final dependency policy violations are submission failures
- reviewer intent analysis is not required
- the final repository state and runner result are authoritative

## Package Manager Scope

Temporary policy:

- npm is the first supported package manager
- official evaluation uses npm and `package-lock.json`
- other package managers are out of scope unless equivalent validation rules are defined

This keeps the first runner small and reviewable. The policy may later add pnpm, Yarn, or Bun under
equivalent source, lockfile, and script restrictions.

A project that requires another package manager and cannot install or start through the official npm
runner is `Unrunnable`.

## Package Manifest Rules

The submitted project must not include `.npmrc` in the repository.

The model may edit `package.json`, but the final `package.json` must not modify npm trust,
resolution, publishing, package-manager, or workspace behavior in ways that bypass the benchmark
policy.

Hard rejection rules:

- reject dependency specs using `git`, `github`, `gist`, `file`, local paths, tarball URLs, `http`,
  or `https`
- reject `npm:` alias dependencies
- reject moving tags such as `latest`, `next`, `beta`, and `canary`
- require exact semver versions for direct dependencies
- reject root `optionalDependencies`
- reject `bundledDependencies` and `bundleDependencies`
- reject `workspaces`
- reject `overrides`
- reject `resolutions`
- reject `publishConfig`
- reject package-manager configuration that requires a non-npm installer for official evaluation
- when `packageManager` is present, require an exact stable npm version such as `npm@11.11.0`

Allowed dependency sections:

- `dependencies`
- `devDependencies`

All other dependency-bearing sections are rejected unless a later benchmark version explicitly adds
validation rules for them. Allowed dependency sections are valid only when every direct dependency
follows the approved version and source rules.

## Script Rules

Install-related lifecycle scripts are rejected.

Hard rejection script names:

- `preinstall`
- `install`
- `postinstall`
- `prepare`
- `prepack`
- `postpack`
- `prepublish`
- `prepublishOnly`

Normal project scripts may be allowed:

- `dev`
- `build`
- `test`
- `start`
- `preview`

These scripts remain untrusted. Official evaluation must run them through the controlled runner
environment, not directly on a credentialed host.

Project scripts must not bypass dependency validation by installing undeclared packages, invoking
undeclared packages from the network, or executing remote install scripts. Examples include runtime
`npm install`, `npm add`, `npm exec`, `npx`, or `curl`/`wget` piped to a shell for project setup.
Required project dependencies must be declared in `dependencies` or `devDependencies` and resolved
through the validated npm lockfile.

The MVP runner statically rejects obvious script forms that match these examples. This static check
is not a complete sandbox for all possible script behavior; final evaluation still depends on the
controlled runner environment and reviewer evidence for anything the static script check cannot prove.

## Lockfile Rules

Official evaluation must generate a fresh `package-lock.json` from the submitted `package.json`
with lifecycle scripts disabled:

```bash
npm install --package-lock-only --ignore-scripts
```

The generated lockfile is authoritative for official installation and must be validated before
install. A submitted lockfile is not trusted as evidence that dependency resolution is safe.

The submitted project must not include `npm-shrinkwrap.json`. Official evaluation is defined around
freshly generated `package-lock.json`, and npm shrinkwrap files are rejected instead of treated as an
authoritative dependency artifact.

Hard rejection rules:

- every `resolved` URL must point to the approved npm registry or approved registry proxy
- reject link dependencies
- reject local file dependencies
- reject git dependencies
- reject remote tarballs outside the approved registry or proxy
- require `integrity` metadata for registry packages
- reject bundled package markers when present

Review signal:

- report transitive packages with install scripts

Temporary policy: transitive install scripts are reported, not rejected, when the official install
uses `--ignore-scripts`.

## Install Rules

Official dependency installation must use:

```bash
npm ci --ignore-scripts
```

The runner must pass explicit npm flags instead of relying only on global npm configuration.

Useful npm defaults for the runner environment:

```text
ignore-scripts=true
min-release-age=7
registry=https://registry.npmjs.org/
fund=false
audit=false
progress=false
foreground-scripts=false
save-exact=true
```

The official runner must use `min-release-age=7` or a stricter release-age policy for dependency
resolution. The public npm registry is the default approved registry. An operator-controlled
proxy/cache may be used when it enforces equivalent or stricter source and release-age rules. The
choice must be recorded before official evaluation.

## Build, Test, And Preview Rules

Build, test, and preview commands may use network access.

The security requirement is not offline execution. The security requirement is that project commands
do not receive host user credentials or broad host filesystem access.

Build, test, and preview must run in the controlled runner environment with:

- no host home directory mount
- no SSH keys
- no npm tokens
- no git credentials
- no browser profiles
- no cloud credentials
- no secret environment variables
- no Docker socket
- a non-root container user when containers are used
- only the submitted repository mounted

If a command needs network access, that is allowed unless a later benchmark version narrows the
policy.

## Container Rules

Containers are recommended for install, build, test, and preview isolation, but the requirement is
the isolation boundary, not Docker specifically.

The official runner must provide equivalent isolation with a container, VM, disposable host, or other
operator-controlled mechanism. When containers are used, the container configuration must:

- use a pinned Node.js/npm environment
- run as a non-root user
- mount only the benchmark repository or submission directory
- use a temporary npm cache
- do not mount the host home directory
- do not mount `/var/run/docker.sock`
- do not pass secret environment variables

Docker is not a sufficient boundary if the implementation model has unrestricted Docker access.

If the model can run arbitrary Docker commands, it may be able to mount host paths or access host
resources. For stronger isolation, the model or agent user should not be in the `docker` group, and
official evaluation should be run by an operator-controlled wrapper, VM, or disposable host.
If a model or agent user was in the `docker` group during implementation, record that as
development isolation risk. It does not by itself decide dependency-policy status for the final
submission, because the official runner evaluates the final repository state separately.

Known MVP residual risk: the first container wrapper may not enforce CPU, memory, process, or disk
limits, and a locally built runner image may record only a local image ID rather than a published
digest. These are hardening gaps to record as operator evidence, not dependency-policy failures by
themselves.

## Runner Flow

Official evaluation must run from a clean dependency state and use this sequence:

1. validate `package.json`
2. generate a fresh lockfile without scripts
3. validate the generated lockfile
4. remove any existing `node_modules` and generated dependency artifacts
5. install dependencies with `npm ci --ignore-scripts`
6. run the documented test command when present
7. run the documented build command when present
8. run a bounded preview readiness check
9. when manual review is needed, start the documented preview or dev server in serve mode

The policy requirement is that official evaluation uses a controlled runner rather than trusting the
model's previous install state or runner scripts copied from a mutable submission.

The runner should discover project commands from `package.json` scripts. README instructions should
match those scripts. If README instructions and `package.json` scripts disagree, the runner result is
authoritative and the mismatch is review evidence.

When a container wrapper is used, official evaluation should run against a disposable writable copy
of the submitted project rather than bind-mounting the original submission read-write. The original
submission remains evidence; generated lockfiles, installed dependencies, and preview artifacts belong
in the runner workspace.

## Failure Policy

During an implementation run:

- a rejected dependency is feedback
- the model may choose a different dependency and retry
- repeated failure to produce a runnable project remains the model's responsibility

For final evaluation:

- dependency policy violation in final metadata: `Contract-Failing`
- forbidden package source in final lockfile: `Contract-Failing`
- install failure through the official runner: `Unrunnable`
- app cannot be started through the official runner: `Unrunnable`
- documented test command fails: `Contract-Failing`
- runner passes and product contract passes: eligible for `Comparable`

## Operator Readiness Checks

Before official evaluation, the operator must confirm:

- no private credentials are stored in the benchmark workspace
- the runner does not mount the host home directory
- the runner does not pass secret environment variables
- the runner does not mount the Docker socket
- the runner uses a non-root user when containerized
- any uncontrolled Docker access or broad host shell access during implementation is recorded as
  development isolation risk
- the approved registry or proxy is known
- the dependency policy status is recorded

The common credential-file check belongs especially at the start of a run, before the workspace is
given to an implementation model. Operators should scan for filenames such as `.env`, `.env.local`,
`.netrc`, `id_rsa`, `id_ed25519`, and generic `credentials` files so they can clean the workspace
early. The official npm runner hard-rejects `.npmrc` because it changes npm trust and credential
behavior; broader credential-file findings are operator evidence and must be resolved by workspace
hygiene and runner isolation rather than by trusting model behavior.

## MVP Decisions

These decisions define the accepted temporary MVP policy.

- npm remains the only supported package manager for the first runner. pnpm, Yarn, and Bun remain out
  of scope until equivalent validators exist.
- exact prerelease dependency versions are rejected by default. A later benchmark version may add a
  narrow allowlist if a prerelease dependency becomes necessary.
- transitive install scripts are reported as review evidence, not rejected, when official install
  uses `--ignore-scripts`.
- transitive `optionalDependencies` are reported as review evidence, not rejected, in the first
  policy version.
- `min-release-age=7` is mandatory for official dependency resolution unless the approved registry
  proxy enforces a stricter freshness policy.
- the public npm registry is the default approved registry. An operator-controlled proxy/cache is an
  allowed stronger mode when it preserves the same source restrictions.
- the submitted lockfile is not trusted for official installation. Official evaluation generates a
  fresh lockfile from the submitted `package.json`; comparing it to the submitted lockfile is only a
  review signal.
- `npm-shrinkwrap.json` is rejected in submitted projects because official evaluation uses a freshly
  generated `package-lock.json`.
- official evaluation removes existing `node_modules` and generated dependency artifacts before
  install.
- the runner discovers commands from `package.json` scripts. README instructions must agree with
  those scripts; disagreements are review evidence.
- the runner passes only an explicit allowlist of non-secret environment variables, such as `HOME`,
  `PATH`, npm configuration needed by the runner, `CI`, `PORT`, and `HOST`.
- the runner pins specific Node.js and npm versions. It must not use floating `latest` runtime
  images, moving `packageManager` values, or unrecorded runtime versions for official evaluation.
- a VM or disposable host is the stronger official-evaluation choice when the implementation model
  had uncontrolled Docker access or broad host shell access. Otherwise, an operator-controlled
  container runner is acceptable.
- if this policy affects comparability for official submissions, promotion should be recorded as a
  benchmark-version change before those submissions are compared.
