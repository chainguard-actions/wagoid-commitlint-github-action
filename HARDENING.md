<!-- markdownlint-disable -->

# Hardening Report: wagoid--commitlint-github-action/v6.2.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **wagoid--commitlint-github-action/v6.2.0** was hardened automatically. 1 finding(s) were identified and resolved across 2 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The `runs.image:` field in action.yml references a Docker image using a mutable version tag (`6.2.0`) instead of an immutable SHA digest. This means the image content could change without notice, enabling a supply-chain attack. The failing reference is: `image: docker://wagoid/commitlint-github-action:6.2.0`. It should be replaced with a pinned digest such as `image: docker://wagoid/commitlint-github-action@sha256:<64-hex-char-digest> # 6.2.0`.

Locations:

- `action.yml:36`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses

**Notes:**

Pinned the Docker image reference in action.yml from `docker://wagoid/commitlint-github-action:6.2.0` to `docker://wagoid/commitlint-github-action:6.2.0@sha256:e8c10d30bdd216f48bbc2a8479a000a248b072cd71cdb9900b940fee5e70c775`. The docker:// scheme and the :6.2.0 tag are preserved inline alongside the immutable digest.

### Iteration 2

**Fixes applied:** script-injection, unpinned-uses, missing-permissions

**Notes:**

Fixed all three findings:
1. script-injection (commitlint.yml line 26): Moved `${{ toJSON(steps.run_commitlint.outputs.results) }}` out of the `run:` shell string into an `env:` block as `COMMITLINT_RESULTS`, then referenced it as `echo "$COMMITLINT_RESULTS"` in the shell.
2. unpinned-uses: Pinned all mutable tag references to full SHA digests in both workflow files:
   - actions/checkout@v3 → @f43a0e5ff2bd294095638e18286ca9a3d1956744 # v3
   - actions/setup-node@v4 → @49933ea5288caeca8642d1e84afbd3f7d6820020 # v4
   - actions/cache@v3 → @6f8efc29b200d32929f49075959781ed54ec270c # v3
   Applied to both jobs in ci.yml (sanity-checks, release) and both jobs in commitlint.yml (commitlint, commitlint-pulling-from-docker-hub).
3. missing-permissions: Added top-level `permissions: contents: read` to commitlint.yml. The ci.yml already had job-level permissions blocks (contents: read for sanity-checks, contents: write for release).

