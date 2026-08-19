<!-- markdownlint-disable -->

# Hardening Report: kunalnagarco--action-cve/v1.17.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **kunalnagarco--action-cve/v1.17.0** was hardened automatically. 4 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All three `uses:` references in cd.yml use mutable version tags instead of full 40-character SHA commit hashes, making the workflow vulnerable to supply-chain attacks if the upstream action is compromised or the tag is moved. Failing references: `actions/checkout@v4` (line 12), `actions/setup-node@v4` (line 13), `actions/cache@v4` (line 20).

Locations:

- `.github/workflows/cd.yml:12`
- `.github/workflows/cd.yml:13`
- `.github/workflows/cd.yml:20`

### unpinned-uses (severity: high)

All three `uses:` references in ci.yml use mutable version tags instead of full 40-character SHA commit hashes, making the workflow vulnerable to supply-chain attacks if the upstream action is compromised or the tag is moved. Failing references: `actions/checkout@v4` (line 9), `actions/setup-node@v4` (line 10), `actions/cache@v4` (line 17).

Locations:

- `.github/workflows/ci.yml:9`
- `.github/workflows/ci.yml:10`
- `.github/workflows/ci.yml:17`

### missing-permissions (severity: medium)

The workflow file has no top-level `permissions:` key and the single job `cd` has no job-level `permissions:` key either. Without explicit permissions, the workflow inherits the repository's default token permissions, which may be overly broad (e.g., write access to contents and packages). A minimal `permissions:` block should be added.

Locations:

- `.github/workflows/cd.yml:1`

### missing-permissions (severity: medium)

The workflow file has no top-level `permissions:` key and the single job `ci` has no job-level `permissions:` key either. Without explicit permissions, the workflow inherits the repository's default token permissions, which may be overly broad. A minimal `permissions:` block should be added.

Locations:

- `.github/workflows/ci.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all four findings across cd.yml and ci.yml:

1. **unpinned-uses (cd.yml & ci.yml)**: Pinned all three `uses:` references to full 40-character commit SHAs:
   - `actions/checkout@v4` → `actions/checkout@11d5960a326750d5838078e36cf38b85af677262 # v4`
   - `actions/setup-node@v4` → `actions/setup-node@49933ea5288caeca8642d1e84afbd3f7d6820020 # v4`
   - `actions/cache@v4` → `actions/cache@0057852bfaa89a56745cba8c7296529d2fc39830 # v4`

2. **missing-permissions (cd.yml)**: Added top-level `permissions: contents: write` — the CD workflow runs `yarn release` which needs to create GitHub releases/tags.

3. **missing-permissions (ci.yml)**: Added top-level `permissions: contents: read` — the CI workflow only needs to read repository contents for checkout, build, and test operations.

