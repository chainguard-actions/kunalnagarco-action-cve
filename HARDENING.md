<!-- markdownlint-disable -->

# Hardening Report: kunalnagarco--action-cve/v1.17.3

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **kunalnagarco--action-cve/v1.17.3** was hardened automatically. 4 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Workflow uses action references pinned to mutable version tags instead of immutable full-length commit SHAs. If the tag is moved (e.g. by a supply-chain compromise), the workflow will silently execute different code. Failing references: `actions/checkout@v4` (line 12), `actions/setup-node@v4` (line 13), `actions/cache@v4` (line 20). Each should be pinned to a full 40-character SHA, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4`.

Locations:

- `.github/workflows/cd.yml:12`
- `.github/workflows/cd.yml:13`
- `.github/workflows/cd.yml:20`

### unpinned-uses (severity: high)

Workflow uses action references pinned to mutable version tags instead of immutable full-length commit SHAs. Failing references: `actions/checkout@v4` (line 9), `actions/setup-node@v4` (line 10), `actions/cache@v4` (line 17). Each should be pinned to a full 40-character SHA, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4`.

Locations:

- `.github/workflows/ci.yml:9`
- `.github/workflows/ci.yml:10`
- `.github/workflows/ci.yml:17`

### missing-permissions (severity: medium)

The workflow has no top-level `permissions:` block and no job-level `permissions:` block on any job. Without explicit permissions, the default token permissions (which may be broad, including `write` access to contents and packages) are granted to the GITHUB_TOKEN. A minimal `permissions:` block should be added, e.g. `permissions: read-all` at the top level and then narrowed per-job.

Locations:

- `.github/workflows/cd.yml:1`

### missing-permissions (severity: medium)

The workflow has no top-level `permissions:` block and no job-level `permissions:` block on any job. Without explicit permissions, the default token permissions (which may be broad) are granted to the GITHUB_TOKEN. A minimal `permissions:` block should be added.

Locations:

- `.github/workflows/ci.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all four findings across two workflow files:

**cd.yml:**
- Pinned `actions/checkout@v4` → `actions/checkout@34e114876b0b11c390a56381ad16ebd13914f8d5 # v4`
- Pinned `actions/setup-node@v4` → `actions/setup-node@49933ea5288caeca8642d1e84afbd3f7d6820020 # v4`
- Pinned `actions/cache@v4` → `actions/cache@0057852bfaa89a56745cba8c7296529d2fc39830 # v4`
- Added top-level `permissions: contents: write` (required for the Release step)

**ci.yml:**
- Pinned the same three actions to the same full SHAs
- Added top-level `permissions: contents: read` (minimal read-only for CI builds)

All SHAs were resolved via lookup_action_sha and are real immutable commit SHAs.

