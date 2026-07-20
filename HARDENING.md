<!-- markdownlint-disable -->

# Hardening Report: actions--delete-package-versions/v3.0.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **actions--delete-package-versions/v3.0.1** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference GitHub Actions using mutable version tags instead of full 40-character commit SHAs. This exposes the workflow to supply-chain attacks if the tag is moved to a malicious commit. Affected references:
- .github/workflows/check-dist.yml: `actions/checkout@v2` (line 23), `actions/setup-node@v1` (line 25), `actions/upload-artifact@v2` (line 42)
- .github/workflows/licensed.yml: `actions/checkout@v2` (line 8)
- .github/workflows/test.yml: `actions/checkout@v2` (line 16), `actions/checkout@v2` (line 24)

Locations:

- `.github/workflows/check-dist.yml:23`
- `.github/workflows/check-dist.yml:25`
- `.github/workflows/check-dist.yml:42`
- `.github/workflows/licensed.yml:8`
- `.github/workflows/test.yml:16`
- `.github/workflows/test.yml:24`

### missing-permissions (severity: medium)

None of the workflow files define a top-level `permissions:` block, and no individual jobs define job-level `permissions:` blocks either. Without explicit permissions, workflows run with the default token permissions which may be overly broad (e.g., write access to contents and packages). Each workflow file should declare minimal required permissions.

Locations:

- `.github/workflows/check-dist.yml:1`
- `.github/workflows/licensed.yml:1`
- `.github/workflows/test.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all 6 unpinned action references across 3 workflow files by replacing mutable version tags with full 40-character commit SHAs (preserving the original tag in a comment for readability): actions/checkout@v2 → @ee0669bd1cc54295c223e0bb666b733df41de1c5, actions/setup-node@v1 → @f1f314fca9dfce2769ece7d933488f076716723e, actions/upload-artifact@v2 → @82c141cc518b40d92cc801eee768e7aafc9c2fa2. Added `permissions: {}` top-level block to all 3 workflow files (check-dist.yml, licensed.yml, test.yml) since none of these workflows require special GITHUB_TOKEN permissions.

