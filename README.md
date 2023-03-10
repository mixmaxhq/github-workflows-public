# Reusable GitHub Actions Workflows

Reusing GitHub Actions workflows across repositories.

## Workflows

Specify Node version in `.nvmrc` and create a file `.github/workflows/ci.yml` with the workflow definition.

```
name: continuous-integration

on:
  push:
    branches:
      - master
  pull_request:
    branches:
      - master

jobs:
  checks:
    uses: mixmaxhq/github-workflows-public/.github/workflows/checks.yml@main

  release:
    uses: mixmaxhq/github-workflows-public/.github/workflows/release.yml@main
    needs: checks
    secrets: inherit
```
