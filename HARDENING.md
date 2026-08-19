<!-- markdownlint-disable -->

# Hardening Report: kawax--composer-update-action/v1.1.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **kawax--composer-update-action/v1.1.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Both workflow files reference GitHub Actions using mutable tag refs instead of full 40-character SHA commit hashes, making them vulnerable to supply-chain attacks if the referenced tags are moved or compromised.

.github/workflows/test.yml:
  - uses: actions/checkout@v2
  - uses: shivammathur/setup-php@v2

.github/workflows/update.yml:
  - uses: actions/checkout@v2
  - uses: kawax/composer-update-action@v1

All four references should be pinned to their full commit SHA (e.g. actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v2).

Locations:

- `.github/workflows/test.yml:13`
- `.github/workflows/test.yml:15`
- `.github/workflows/update.yml:13`
- `.github/workflows/update.yml:15`

### missing-permissions (severity: medium)

Neither .github/workflows/test.yml nor .github/workflows/update.yml declares a top-level `permissions:` block, and no job-level `permissions:` blocks are present in either file. Without explicit permissions, the GITHUB_TOKEN is granted its default (broad) permissions, which may include write access to repository contents. A minimal permissions block (e.g. `permissions: read-all` or specific scopes) should be added.

Locations:

- `.github/workflows/test.yml:1`
- `.github/workflows/update.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed both workflow files: (1) Pinned all four action references to full 40-char SHAs — actions/checkout@v2 → 0717577d45739eb3c851188b29f50ed6c0b2194e, shivammathur/setup-php@v2 → f3e473d116dcccaddc5834248c87452386958240, kawax/composer-update-action@v1 → b4bcd029735930e7a73a6babffad5364ccd3e462. (2) Added top-level permissions blocks: test.yml gets `contents: read` (minimal for checkout+test), update.yml gets `contents: write` and `pull-requests: write` (needed for the composer update action to push changes and open PRs).

