# AGENTS.md

This file provides guidance to AI coding agents when working with code in this repository.

## Project Overview

A composite GitHub Action (`action.yml`) that installs the [ecspresso](https://github.com/kayac/ecspresso) CLI on Linux runners. The CLI itself is developed in `kayac/ecspresso`; this repository only contains the action.

## Git Workflow

- Default branch: `main`
- Do not commit directly to `main`. Always create a feature branch first.
- Releases are managed by [tagpr](https://github.com/Songmu/tagpr). Merging the tagpr release PR creates a full version tag (e.g. `v3.1.0`) and a GitHub Release, and the workflow moves the major tag (e.g. `v3`) to it.
- Never move or delete full version tags. Immutable releases are enabled.

## Testing

`.github/workflows/test.yml` runs the action from the working tree (`uses: ./`) on pull requests and pushes to `main`. Add a test case there when changing the action's behavior.

## Pinning Rules

- Workflows under `.github/workflows/` are onboarded to the [workflow dependency lockfile](https://docs.github.com/en/actions/concepts/security/workflow-dependency-lockfiles) (`.github/workflows/actions.lock`). Reference actions by version tag (e.g. `actions/checkout@v6.0.2`) and run `gh actions-lock` to update the lockfile. Do not pin them by SHA.
- Actions used inside `action.yml` must be pinned by full commit SHA with a version comment, using `pinact run`. Lockfiles do not cover action files.

## Code Style

- Never expand `${{ inputs.* }}` or `${{ github.* }}` directly in `run:`. Pass them through `env:`.
- Keep the `kayac/ecspresso` forwarder `action.yml` inputs in sync when adding or removing inputs.
- Update README.md when inputs or behavior change.
