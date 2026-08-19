<!-- markdownlint-disable -->

# Hardening Report: kawax--composer-update-action/v3.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **kawax--composer-update-action/v3.0.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Both workflow files reference actions using mutable version tags instead of pinned full-length SHA commit hashes, making the workflows vulnerable to supply-chain attacks if those tags are moved.

.github/workflows/test.yml:
  - uses: actions/checkout@v2
  - uses: shivammathur/setup-php@v2
  - uses: paambaati/codeclimate-action@v2.7.5

.github/workflows/update.yml:
  - uses: actions/checkout@v2
  - uses: kawax/composer-update-action@v3

All of these should be pinned to a full 40-character commit SHA (e.g. actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4).

Locations:

- `.github/workflows/test.yml:13`
- `.github/workflows/test.yml:15`
- `.github/workflows/test.yml:22`
- `.github/workflows/update.yml:13`
- `.github/workflows/update.yml:15`

### missing-permissions (severity: medium)

Neither .github/workflows/test.yml nor .github/workflows/update.yml defines a top-level `permissions:` key, and no job within either file defines a job-level `permissions:` block. Without explicit permissions, workflows inherit the default repository permissions (which may be write-all), granting broader access than necessary.

Locations:

- `.github/workflows/test.yml:1`
- `.github/workflows/update.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed both workflow files:

.github/workflows/test.yml:
- Added top-level `permissions: contents: read`
- Pinned actions/checkout@v2 → @0717577d45739eb3c851188b29f50ed6c0b2194e # v2
- Pinned shivammathur/setup-php@v2 → @f3e473d116dcccaddc5834248c87452386958240 # v2
- Pinned paambaati/codeclimate-action@v2.7.5 → @7bcf9e73c0ee77d178e72c0ec69f1a99c1afc1f3 # v2.7.5

.github/workflows/update.yml:
- Added top-level `permissions: contents: write, pull-requests: write` (composer-update-action needs to commit changes and open PRs)
- Pinned actions/checkout@v2 → @0717577d45739eb3c851188b29f50ed6c0b2194e # v2
- Pinned kawax/composer-update-action@v3 → @cdbeeb4dc83b68c4fb03ce82767c6497c6f27df2 # v3

All original tags preserved as inline comments for readability.

