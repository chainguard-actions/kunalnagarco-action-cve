<!-- markdownlint-disable -->

# Hardening Report: kunalnagarco--action-cve/v1.17.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **kunalnagarco--action-cve/v1.17.1** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Both workflow files reference GitHub Actions using mutable version tags (@v4) instead of pinned full 40-character commit SHA hashes. This exposes the workflow to supply-chain attacks if the tag is moved to a malicious commit. Affected references in cd.yml: actions/checkout@v4, actions/setup-node@v4, actions/cache@v4. Affected references in ci.yml: actions/checkout@v4, actions/setup-node@v4, actions/cache@v4.

Locations:

- `.github/workflows/cd.yml:10`
- `.github/workflows/cd.yml:11`
- `.github/workflows/cd.yml:18`
- `.github/workflows/ci.yml:8`
- `.github/workflows/ci.yml:9`
- `.github/workflows/ci.yml:16`

### missing-permissions (severity: medium)

Neither .github/workflows/cd.yml nor .github/workflows/ci.yml defines a top-level `permissions:` block, and no job-level `permissions:` block is present on any job. Without explicit permissions, the GITHUB_TOKEN is granted its default (broad) permissions, violating the principle of least privilege.

Locations:

- `.github/workflows/cd.yml:1`
- `.github/workflows/ci.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed both workflow files (.github/workflows/cd.yml and .github/workflows/ci.yml):

1. unpinned-uses: Pinned all three action references to full 40-character commit SHAs:
   - actions/checkout@v4 → @34e114876b0b11c390a56381ad16ebd13914f8d5 # v4
   - actions/setup-node@v4 → @49933ea5288caeca8642d1e84afbd3f7d6820020 # v4
   - actions/cache@v4 → @0057852bfaa89a56745cba8c7296529d2fc39830 # v4

2. missing-permissions: Added top-level permissions blocks to both files:
   - cd.yml: `contents: write` (required for the Release step that creates GitHub releases/tags)
   - ci.yml: `contents: read` (minimum needed for checkout and CI validation)

