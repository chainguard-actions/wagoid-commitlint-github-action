<!-- markdownlint-disable -->

# Hardening Report: wagoid--commitlint-github-action/v6.2.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **wagoid--commitlint-github-action/v6.2.1** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The action.yml `runs.image` field references a Docker image using a mutable tag (`docker://wagoid/commitlint-github-action:6.2.1`) instead of an immutable SHA digest. This means the image could be replaced with a different (potentially malicious) version without changing the action reference. It should be pinned to a SHA digest, e.g. `docker://wagoid/commitlint-github-action@sha256:<64-hex-char-digest>`.

Locations:

- `action.yml:43`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses

**Notes:**

Pinned the Docker image in action.yml from the mutable tag `docker://wagoid/commitlint-github-action:6.2.1` to the immutable digest `docker://wagoid/commitlint-github-action@sha256:86a04e0a99128551a7555c269d2b675c3c85f61358cf7dd558f6b873b66f561a # 6.2.1`. The original tag is preserved as a comment for readability.

