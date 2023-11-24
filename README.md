# compas-actions.docs

## Introduction
The `compas-actions.docs` GitHub action is designed for building documentation for COMPAS or its plugins. It facilitates documentation generation, including handling various dependencies and publishing the docs.

## Usage
To use this action in your workflow, add it to your `.yml` file with the necessary inputs. It's a composite action that includes multiple steps like installing dependencies, setting up a virtual display, and deploying documentation.

```yaml
steps:
- uses: your-repo/compas-actions.docs@v3
  with:
    doc_url: <URL of your docs site>
    github_token: ${{ secrets.GITHUB_TOKEN }}
    # Other optional inputs
```

## Inputs
1. `doc_url`: URL of the docs site. (required)
2. `github_token`: GitHub token for publishing docs. (required)
3. `test_docs`: Whether to test docstrings in this action. (default: 'true')
4. `python`: Python version to build docs with. (default: "3.10")
5. `use_conda`: Whether to build docs with conda. (default: "false")
6. `use_virtual_display`: Use virtual display on Linux when matplotlib is imported. (default: "false")
7. `use_latex`: Install dependencies for LaTex. (default: "false")

## Outputs
1. `commit_type`: Type of the commit - main, pull, or tag.
2. `current_version`: Version number if the commit is a tag.
3. `subfolder`: Subfolder name where the docs are built.

## Examples
Here's an example of integrating `compas-actions.docs` into a GitHub workflow:

```yaml
name: docs

on:
  push:
    branches:
      - main
    tags:
      - 'v*'
  pull_request:
    branches:
      - main

jobs:
  docs:
    runs-on: ubuntu-latest
    steps:
      - uses: compas-dev/compas-actions.docs@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          doc_url: https://compas.dev/compas
```

If your plugin involves C++ extensions and can only be built in certain OS, you can use the `use_conda` input to build the docs with conda on that specific system. Here's an example:

```yaml
name: docs

on:
  push:
    branches:
      - main
    tags:
      - 'v*'
  pull_request:
    branches:
      - main

jobs:
  docs:
    runs-on: windows-latest
    steps:
      - uses: compas-dev/compas-actions.docs@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          use_conda: true
          doc_url: https://compas.dev/compas_cgal
```

