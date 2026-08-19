<!-- markdownlint-disable -->

# Hardening Report: wagoid--commitlint-github-action/v6.2.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **wagoid--commitlint-github-action/v6.2.1** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple unpinned action/image references found using mutable tags instead of full 40-character SHA commit digests or SHA image digests:

• action.yml: `image: docker://wagoid/commitlint-github-action:6.2.1` — uses a mutable version tag, not a SHA digest (e.g. `@sha256:<64-hex-char-digest>`).
• .github/workflows/ci.yml: `actions/checkout@v3`, `actions/setup-node@v4`, `actions/cache@v3` (used twice each in two jobs).
• .github/workflows/commitlint.yml: `actions/checkout@v3`, `actions/setup-node@v4`, `actions/cache@v3` (used twice each in two jobs).

Mutable tags can be silently updated to point to different, potentially malicious commits, enabling supply-chain attacks.

Locations:

- `action.yml:37`
- `.github/workflows/ci.yml:9`
- `.github/workflows/ci.yml:10`
- `.github/workflows/ci.yml:12`
- `.github/workflows/ci.yml:30`
- `.github/workflows/ci.yml:31`
- `.github/workflows/ci.yml:33`
- `.github/workflows/commitlint.yml:7`
- `.github/workflows/commitlint.yml:9`
- `.github/workflows/commitlint.yml:11`
- `.github/workflows/commitlint.yml:30`
- `.github/workflows/commitlint.yml:31`
- `.github/workflows/commitlint.yml:33`

### script-injection (severity: high)

Sub-rule (a) violation: A `${{ }}` expression is interpolated directly inside a `run:` shell command string. In `.github/workflows/commitlint.yml`, the step 'Show results from JSON output' contains:

  `run: echo ${{ toJSON(steps.run_commitlint.outputs.results) }}`

The `steps.run_commitlint.outputs.results` value is workflow-controllable (it comes from the action's output, which is derived from commit messages that can be attacker-controlled). GitHub Actions performs YAML template substitution before the shell ever sees the string, so any shell metacharacters in the output value will be interpreted by the shell. The value should be passed via an `env:` variable and then referenced as a quoted shell variable: `echo "$RESULTS"`.

Locations:

- `.github/workflows/commitlint.yml:22`

### missing-permissions (severity: medium)

`.github/workflows/commitlint.yml` has no top-level `permissions:` key and neither of its two jobs (`commitlint` and `commitlint-pulling-from-docker-hub`) defines a job-level `permissions:` block. Without explicit permissions, the workflow inherits the repository's default token permissions, which may be overly broad (e.g. `write` access to contents). Explicit minimal permissions should be declared for each job.

Locations:

- `.github/workflows/commitlint.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection, missing-permissions

**Notes:**

Fixed all three findings:
1. **unpinned-uses**: Pinned all action references to full SHAs with tag comments for readability:
   - `actions/checkout@v3` → `@f43a0e5ff2bd294095638e18286ca9a3d1956744 # v3`
   - `actions/setup-node@v4` → `@49933ea5288caeca8642d1e84afbd3f7d6820020 # v4`
   - `actions/cache@v3` → `@6f8efc29b200d32929f49075959781ed54ec270c # v3`
   - Docker image in action.yml: `docker://wagoid/commitlint-github-action:6.2.1@sha256:86a04e0a99128551a7555c269d2b675c3c85f61358cf7dd558f6b873b66f561a` (preserving docker:// scheme and tag)
2. **script-injection**: In commitlint.yml, moved `${{ toJSON(steps.run_commitlint.outputs.results) }}` from the `run:` shell string into an `env:` block as `RESULTS`, then used `echo "$RESULTS"` in the shell script.
3. **missing-permissions**: Added `permissions: {}` at the top level of commitlint.yml and `permissions: contents: read` at the job level for both `commitlint` and `commitlint-pulling-from-docker-hub` jobs.

