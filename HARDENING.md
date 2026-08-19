<!-- markdownlint-disable -->

# Hardening Report: kawax--composer-update-action/v1.2.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **kawax--composer-update-action/v1.2.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Both workflow files reference GitHub Actions using mutable version tags instead of full 40-character commit SHAs. This exposes the workflow to supply-chain attacks if the referenced tag is moved or the action is compromised. Failing references: test.yml uses `actions/checkout@v2` and `shivammathur/setup-php@v2`; update.yml uses `actions/checkout@v2` and `kawax/composer-update-action@v1`. All should be pinned to full SHA digests.

Locations:

- `.github/workflows/test.yml:12`
- `.github/workflows/test.yml:14`
- `.github/workflows/update.yml:13`
- `.github/workflows/update.yml:15`

### missing-permissions (severity: medium)

Neither .github/workflows/test.yml nor .github/workflows/update.yml defines a top-level `permissions:` block, and no job within either file defines its own `permissions:` block. Without explicit permissions, workflows run with the default (potentially broad) token permissions, violating the principle of least privilege.

Locations:

- `.github/workflows/test.yml:1`
- `.github/workflows/update.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed both workflow files: (1) Pinned all action references to full 40-character commit SHAs — actions/checkout@v2 → @ee0669bd1cc54295c223e0bb666b733df41de1c5, shivammathur/setup-php@v2 → @f3e473d116dcccaddc5834248c87452386958240, kawax/composer-update-action@v1 → @b4bcd029735930e7a73a6babffad5364ccd3e462. Original tags preserved as inline comments. (2) Added top-level permissions blocks: test.yml gets 'contents: read' (read-only for testing), update.yml gets 'contents: write' (required for the composer update action to push commits).

