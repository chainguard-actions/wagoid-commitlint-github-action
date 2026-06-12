<!-- markdownlint-disable -->

# Hardening Report: wagoid--commitlint-github-action/v6.2.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **wagoid--commitlint-github-action/v6.2.0** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The action.yml uses a Docker image reference with a mutable tag instead of a SHA digest. The image `docker://wagoid/commitlint-github-action:6.2.0` uses the tag `6.2.0`, which can be silently overwritten on the registry, enabling a supply-chain attack. It should be pinned to an immutable SHA digest, e.g. `docker://wagoid/commitlint-github-action@sha256:<64-hex-char-digest> # 6.2.0`.

Locations:

- `action.yml:37`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses

**Notes:**

Pinned the Docker image `wagoid/commitlint-github-action:6.2.0` to its immutable SHA256 digest `sha256:e8c10d30bdd216f48bbc2a8479a000a248b072cd71cdb9900b940fee5e70c775` in action.yml line 37. The tag `6.2.0` is preserved as a comment for readability.

