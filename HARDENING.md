<!-- markdownlint-disable -->

# Hardening Report: wagoid--commitlint-github-action/v6.1.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **wagoid--commitlint-github-action/v6.1.0** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The action.yml uses a Docker image reference with a mutable tag instead of a SHA digest. `image: docker://wagoid/commitlint-github-action:6.1.0` uses the tag `6.1.0`, which can be silently overwritten on the registry, enabling supply-chain attacks. It should be pinned to an immutable SHA digest, e.g. `image: docker://wagoid/commitlint-github-action@sha256:<64-hex-char-digest>`.

Locations:

- `action.yml:38`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses

**Notes:**

Replaced the mutable Docker image tag `wagoid/commitlint-github-action:6.1.0` with the immutable SHA256 digest `wagoid/commitlint-github-action@sha256:aafb6f5cb6a1e8cc58eb3be688cc501d972d50279c9fc7e8277e2214fef2495d` in action.yml line 38. The original tag is preserved as a comment for readability.

