<!-- markdownlint-disable -->

# Hardening Report: actions--delete-package-versions/v5.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **actions--delete-package-versions/v5.0.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference GitHub Actions using mutable version tags instead of full 40-character commit SHAs. This exposes the workflow to supply-chain attacks if the tag is moved to a malicious commit.

- .github/workflows/check-dist.yml: `actions/checkout@v2` (line 24), `actions/setup-node@v1` (line 27), `actions/upload-artifact@v2` (line 44)
- .github/workflows/licensed.yml: `actions/checkout@v2` (line 16)
- .github/workflows/test.yml: `actions/checkout@v2` (line 18), `actions/setup-node@v3` (line 19), `actions/checkout@v2` (line 30), `actions/setup-node@v3` (line 31)

All should be pinned to a full SHA, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v2`.

Locations:

- `.github/workflows/check-dist.yml:24`
- `.github/workflows/check-dist.yml:27`
- `.github/workflows/check-dist.yml:44`
- `.github/workflows/licensed.yml:16`
- `.github/workflows/test.yml:18`
- `.github/workflows/test.yml:19`
- `.github/workflows/test.yml:30`
- `.github/workflows/test.yml:31`

### missing-permissions (severity: medium)

Three workflow files have no top-level `permissions:` block and no job-level `permissions:` blocks. Without explicit permissions, workflows run with the default token permissions (which may be read/write depending on repository settings), violating the principle of least privilege.

- .github/workflows/check-dist.yml: no permissions defined
- .github/workflows/integration-tests.yml: no permissions defined
- .github/workflows/licensed.yml: no permissions defined

Each file should declare a top-level `permissions:` block with only the scopes required (e.g. `permissions: read-all` or specific scopes like `contents: read`).

Locations:

- `.github/workflows/check-dist.yml:1`
- `.github/workflows/integration-tests.yml:1`
- `.github/workflows/licensed.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all 8 unpinned action references by resolving full commit SHAs: actions/checkout@v2 → 0717577d45739eb3c851188b29f50ed6c0b2194e, actions/setup-node@v1 → f1f314fca9dfce2769ece7d933488f076716723e, actions/upload-artifact@v2 → 82c141cc518b40d92cc801eee768e7aafc9c2fa2, actions/setup-node@v3 → 3235b876344d2a9aa001b8d1453c930bba69e610. Added top-level permissions blocks to check-dist.yml (contents: read), licensed.yml (contents: read), and integration-tests.yml (permissions: {}). test.yml already had a permissions block (packages: write) and was not modified for permissions. The peter-evans/repository-dispatch action in integration-tests.yml was already pinned to a full SHA.

