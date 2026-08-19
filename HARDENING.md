<!-- markdownlint-disable -->

# Hardening Report: kawax--composer-update-action/v4.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **kawax--composer-update-action/v4.0.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple `uses:` references in workflow files are pinned to mutable tags or branch names instead of full 40-character commit SHAs. This exposes the workflow to supply-chain attacks if the referenced action is compromised or its tag is moved. Failing references: `actions/checkout@v3` (tag), `shivammathur/setup-php@v2` (tag), `paambaati/codeclimate-action@v3.2.0` (tag) in test.yml; `actions/checkout@v3` (tag), `kawax/composer-update-action@master` (branch) in update.yml.

Locations:

- `.github/workflows/test.yml:14`
- `.github/workflows/test.yml:16`
- `.github/workflows/test.yml:24`
- `.github/workflows/update.yml:17`
- `.github/workflows/update.yml:19`

### missing-permissions (severity: medium)

Neither workflow file defines a top-level `permissions:` block, and no job within either file defines job-level permissions. Without explicit permissions, workflows run with the default (potentially broad) token permissions, violating the principle of least privilege.

Locations:

- `.github/workflows/test.yml:1`
- `.github/workflows/update.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all 5 unpinned action references by resolving them to full 40-character commit SHAs (with original tag/branch preserved as inline comments): actions/checkout@v3→a37ce91, shivammathur/setup-php@v2→f3e473d, paambaati/codeclimate-action@v3.2.0→b649ad2, kawax/composer-update-action@master→691851b. Added top-level permissions blocks to both workflow files: test.yml gets `contents: read` (minimum for checkout/test), update.yml gets `contents: write` and `pull-requests: write` (required for the composer-update-action to push dependency updates and open PRs).

