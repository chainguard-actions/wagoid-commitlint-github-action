<!-- markdownlint-disable -->

# Hardening Report: wagoid--commitlint-github-action/v6.1.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **wagoid--commitlint-github-action/v6.1.1** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The action.yml uses a Docker image reference with a mutable version tag instead of an immutable SHA digest. `image: docker://wagoid/commitlint-github-action:6.1.1` uses the tag `6.1.1`, which can be silently overwritten on Docker Hub, enabling a supply-chain attack. It should be pinned to a SHA digest, e.g. `image: docker://wagoid/commitlint-github-action@sha256:<64-hex-char-digest> # 6.1.1`.

Locations:

- `action.yml:40`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses

**Notes:**

Replaced the mutable Docker image tag `docker://wagoid/commitlint-github-action:6.1.1` with the immutable SHA256 digest `docker://wagoid/commitlint-github-action@sha256:0c45396ecf19521155d2a772e86b7a49d88f5748a52a07d6b7263adb61bd9fc8 # 6.1.1` in action.yml line 40. The original tag is preserved as a comment for readability.

