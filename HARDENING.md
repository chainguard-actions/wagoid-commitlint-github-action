<!-- markdownlint-disable -->

# Hardening Report: wagoid--commitlint-github-action/v6.1.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **wagoid--commitlint-github-action/v6.1.1** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple unpinned action/image references use mutable tags instead of full 40-character SHA commit digests, making the action vulnerable to supply-chain attacks.

In action.yml: `image: docker://wagoid/commitlint-github-action:6.1.1` uses a mutable tag instead of a SHA digest (e.g. `@sha256:<digest>`).

In .github/workflows/ci.yml: `actions/checkout@v3` (line 11), `actions/setup-node@v4` (line 12), `actions/cache@v3` (line 15), `actions/checkout@v3` (line 35), `actions/setup-node@v4` (line 38), `actions/cache@v3` (line 41).

In .github/workflows/commitlint.yml: `actions/checkout@v3` (line 7), `actions/setup-node@v4` (line 14), `actions/cache@v3` (line 17), `actions/checkout@v3` (line 38), `actions/setup-node@v4` (line 41), `actions/cache@v3` (line 44).

Locations:

- `action.yml:36`
- `.github/workflows/ci.yml:11`
- `.github/workflows/ci.yml:12`
- `.github/workflows/ci.yml:15`
- `.github/workflows/ci.yml:35`
- `.github/workflows/ci.yml:38`
- `.github/workflows/ci.yml:41`
- `.github/workflows/commitlint.yml:7`
- `.github/workflows/commitlint.yml:14`
- `.github/workflows/commitlint.yml:17`
- `.github/workflows/commitlint.yml:38`
- `.github/workflows/commitlint.yml:41`
- `.github/workflows/commitlint.yml:44`

### script-injection (severity: high)

Sub-rule (a) violation: A GitHub Actions expression is directly interpolated inside a `run:` shell command string. In .github/workflows/commitlint.yml, the step 'Show results from JSON output' contains: `run: echo ${{ toJSON(steps.run_commitlint.outputs.results) }}`. The `steps.*.outputs.*` context is workflow-controllable and flows through YAML template substitution before the shell processes it, allowing an attacker to inject arbitrary shell commands via crafted commit messages that influence the action's output.

Locations:

- `.github/workflows/commitlint.yml:28`

### missing-permissions (severity: medium)

The workflow file .github/workflows/commitlint.yml has no top-level `permissions:` block and neither of its two jobs (`commitlint` and `commitlint-pulling-from-docker-hub`) defines a job-level `permissions:` block. Without explicit permissions, the workflow inherits the default repository permissions, which may be overly broad (e.g. write access to contents). Explicit minimal permissions should be declared for each job.

Locations:

- `.github/workflows/commitlint.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection, missing-permissions

**Notes:**

Fixed all three findings: (1) Pinned the Docker image in action.yml with SHA digest `@sha256:0c45396ecf19521155d2a772e86b7a49d88f5748a52a07d6b7263adb61bd9fc8`. Pinned all action references in ci.yml and commitlint.yml to full 40-char SHAs: actions/checkout@v3 → f43a0e5ff2bd294095638e18286ca9a3d1956744, actions/setup-node@v4 → 49933ea5288caeca8642d1e84afbd3f7d6820020, actions/cache@v3 → 6f8efc29b200d32929f49075959781ed54ec270c. (2) Fixed script injection in commitlint.yml by moving `${{ toJSON(steps.run_commitlint.outputs.results) }}` into an `env:` block as `COMMITLINT_RESULTS` and referencing it as `"$COMMITLINT_RESULTS"` in the shell. (3) Added `permissions: contents: read` to both jobs in commitlint.yml.

