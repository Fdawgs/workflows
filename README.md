# workflows

[![GitHub release](https://img.shields.io/github/release/Fdawgs/workflows.svg)](https://github.com/Fdawgs/workflows/releases/latest/)

> @fdawgs' reusable workflows

## Overview

GitHub [introduced reusable workflows](https://github.blog/2021-11-29-github-actions-reusable-workflows-is-generally-available/) on 2021-11-29 which, as the name suggests, are workflows that can be referenced across the entirety of GitHub. A reusable workflow is called by using the `uses` keyword in another workflow.

For more information, including limitations, [see the GitHub Docs](https://docs.github.com/en/actions/learn-github-actions/reusing-workflows).

This repository contains reusable workflows that are used across multiple repositories maintained by @fdawgs. These workflows are designed to be generic enough to be used in other repositories with minimal modification.

Reusable workflows are prefixed with `reusable-` to distinguish them from regular workflows in this repository.

## Example Usage

### Using the Link Check workflow

```yml
name: Check Markdown for Broken Links

permissions:
    contents: read

jobs:
    link-check:
        name: Link Check
        permissions:
            contents: read
        uses: fdawgs/workflows/.github/workflows/reusable-link-check.yml@v2
```

### Using the Code Quality workflow

This workflow is not intended for use by external repositories as it is designed to be used in @fdawgs' Node.js projects and requires specific scripts to be defined in the `package.json` file. However, it can be used as a reference for creating reusable workflows for other types of projects.

```yml
name: CI

permissions:
    contents: read

jobs:
    code-quality:
        name: Code Quality
        uses: ./.github/workflows/reusable-code-quality.yml@v2
        permissions:
            contents: read
        secrets: inherit
        with:
            commitlint: true
            lint: false
    unit-tests:
        name: Unit Tests
        needs: code-quality
        runs-on: ubuntu-latest
        steps:
            - run: echo "Tests go here"
```

The workflow can be configured with the following inputs:
- `commitlint`: A boolean value indicating whether to run commit message linting (default: `false`)
- `lint`: A boolean value indicating whether to run linting (default: `false`)

If `lint` is set to `true`, the workflow will run linting using ESLint, Prettier, and Licensee licensing checker, and requires a `package.json` file with the [following scripts](./.github/workflows/reusable-code-quality.yml).

## Contributing

Contributions are welcome, and any help is greatly appreciated!

See [the contributing guide](https://github.com/Fdawgs/.github/blob/main/CONTRIBUTING.md) for details on how to get started.
Please adhere to this project's [Code of Conduct](https://github.com/Fdawgs/.github/blob/main/CODE_OF_CONDUCT.md) when contributing.

## License

`workflows` is licensed under the [MIT](./LICENSE) license.

