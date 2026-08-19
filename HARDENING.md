<!-- markdownlint-disable -->

# Hardening Report: wagoid--commitlint-github-action/v6.1.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **wagoid--commitlint-github-action/v6.1.0** was hardened automatically. 1 finding(s) were identified and resolved across 2 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The action.yml uses a Docker image reference with a mutable tag instead of an immutable SHA digest. `image: docker://wagoid/commitlint-github-action:6.1.0` uses the tag `6.1.0`, which can be silently replaced by a different (potentially malicious) image. It should be pinned to a specific SHA256 digest, e.g. `image: docker://wagoid/commitlint-github-action@sha256:<64-hex-char-digest> # 6.1.0`.

Locations:

- `action.yml:34`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses

**Notes:**

Pinned the Docker image reference in action.yml from the mutable tag `docker://wagoid/commitlint-github-action:6.1.0` to the immutable digest `docker://wagoid/commitlint-github-action:6.1.0@sha256:aafb6f5cb6a1e8cc58eb3be688cc501d972d50279c9fc7e8277e2214fef2495d`. The `docker://` scheme and `:6.1.0` tag are preserved inline as required.

### Iteration 2

**Fixes applied:** unpinned-uses, script-injection, missing-permissions

**Notes:**

Fixed all three findings across .github/workflows/ci.yml and .github/workflows/commitlint.yml:

1. unpinned-uses: Pinned all action references to full 40-char SHAs with tag comments preserved: actions/checkout@v3→a37ce9120846195fa4ece8f58b268e6043cb2f26, actions/setup-node@v4→49933ea5288caeca8642d1e84afbd3f7d6820020, actions/cache@v3→6f8efc29b200d32929f49075959781ed54ec270c. Applied to all 6 occurrences across both files.

2. script-injection: In commitlint.yml line 22, moved `${{ toJSON(steps.run_commitlint.outputs.results) }}` out of the run: shell string into a step-level env: block as COMMITLINT_RESULTS, then referenced it as `echo "$COMMITLINT_RESULTS"` in the shell script.

3. missing-permissions: Added `permissions: {}` at the top level of commitlint.yml and `permissions: { contents: read }` to both jobs (commitlint and commitlint-pulling-from-docker-hub), which only need read access to checkout repository contents.

