# github-workflows-public — repo card

> A map, not a manual. Keep it ~1 screen; point to detail, don't inline it.

## What it is
Reusable GitHub Actions workflows shared across Mixmax public repositories. Consuming repos reference these via `uses: mixmaxhq/github-workflows-public/.github/workflows/<name>.yml@main`.

## serves
role: Shared CI/CD workflow library (checks + release) for public Mixmax repos
referenced-by: [any public Mixmax repo that opts in via `uses:` in its `.github/workflows/ci.yml`]

## Code map
- CI checks (lint, test, commitlint) -> `.github/workflows/checks.yml`
- Semantic-release publish                -> `.github/workflows/release.yml`

## Conventions
- Both workflows are `workflow_call` only — they are never triggered directly, only via `uses:` from a consuming repo.
- Consuming repos must define `.nvmrc` for the Node version and expose a `ci` npm/pnpm script (and optionally `ci:commitlint` and `semantic-release`).
- `checks.yml` auto-detects the package manager: pnpm if `pnpm-lock.yaml` exists, else npm.
- `release.yml` runs only on `refs/heads/master` (some consuming repos use `master` not `main`).
- Secrets (`npm_token`, `gh_token`) are passed via `secrets: inherit` in the calling workflow.

## Gotchas
- `release.yml` hardcodes the branch check to `master` — repos on `main` will never trigger a release from this workflow without a change here.
- `release.yml` still uses `actions/checkout@v3` / `actions/setup-node@v3` (older than checks.yml's v4) — keep in sync when upgrading.
- Changes to these workflows take effect immediately for all consumers pinned to `@main`; no versioning or tagging is used.

## Run / test
- No local run. Changes are tested by opening a PR in a consuming repo that references this repo at the branch under review.
- To test a workflow change: push to a branch and reference it via `uses: mixmaxhq/github-workflows-public/.github/workflows/<name>.yml@<branch>` in a test repo's workflow.

## Load the matching domain card
- This repo is cross-cutting tooling — it owns no product domain, so there is no domain card to load. When working here, load the card of the consuming service/domain if the change is driven by its needs.
