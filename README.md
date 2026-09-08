<div align="center">
  
# Python Package Template

The template repository for creating python packages, shared across DAPE.

![Python](https://img.shields.io/badge/Python-3.12-3670A0?style=for-the-badge&logo=python&logoColor=ffdd54)
![UV](https://img.shields.io/badge/UV-Fast-6E40C9?style=for-the-badge)
![Hatchling](https://img.shields.io/badge/Hatchling-PEP517-6E40C9?style=for-the-badge)
![Ruff](https://img.shields.io/badge/Ruff-Lint-000000?style=for-the-badge)
![Pre-commit](https://img.shields.io/badge/Pre--commit-Hooks-000000?style=for-the-badge)
![Pytest](https://img.shields.io/badge/Pytest-Unit%2BAsync-08979C?style=for-the-badge)
![Coverage](https://img.shields.io/badge/Cov-Reports-08979C?style=for-the-badge)
![GitHub Actions](https://img.shields.io/badge/Actions-CI%2FCD-F7B500?style=for-the-badge&logo=github-actions)
![Nexus](https://img.shields.io/badge/Nexus-Publish-6E40C9?style=for-the-badge)
![Makefile](https://img.shields.io/badge/Makefile-Scripts-F7B500?style=for-the-badge)

🦜🕸️

[![CI](https://github.com/iag-dape/python-package-template/actions/workflows/ci.yaml/badge.svg?branch=main)](https://github.com/iag-dape/python-package-template/actions/workflows/ci.yaml)

</div>

---

## Template Checklist
- [ ] Use [github-self-service](https://github.com/iag-hub/github-self-service/actions/workflows/newrepo.yml) portal to create a repository using this template.
- [ ] Rename module `src/dape/template` -> `src/dape/your_package_name`
- [ ] Rename tests module `src/dape/template` -> `src/dape/your_package_name`
- [ ] Update `pyproject.toml`: `[project]` section based on your package name / versioning etc.
- [ ] Update `README.md` references of `python-package-template` -> `your-package-name`
- [ ] Add your team nexus credentials as `HTTP_BASIC_NEXUS_USERNAME` & `HTTP_BASIC_NEXUS_PASSWORD` repository variables / secrets respectively. These will be need for the nexus publish cd workflow to succeed.
- [ ] Publish your package to nexus by creating a release.
- [ ] Register your package in the [python-package-index](https://github.com/iag-dape/python-package-index) repo via the [pyproject.toml](https://github.com/iag-dape/python-package-index/blob/main/pyproject.toml#L16-L42) and [config.yaml](https://github.com/iag-dape/python-package-index/blob/main/config.yaml). Create a PR wwhich will automatically update the readme index documentation.
- [ ] Remove this section

---

## Table of Contents
<!-- toc -->

- [Introduction](#introduction)
- [Quick Start](#quick-start)
- [Installation](#installation)
- [Usage](#usage)
- [Formatting and linting](#formatting-and-linting)
- [CICD](#cicd)
- [Credits](#credits)

<!-- tocstop -->

---

## Introduction

This template repository aims to create a reusable package template which streamlines the creation and publishing of isolated python packages in DAPE. This is aligned with the engineering vision @ IAG for better modularisation and reusability of code.

---

## Quick Start
Since this is just a package, and not a service, there is no real "run" action. But you can run the tests immediately.

Here are a list of available commands via make.

### Bare Metal (i.e. your machine)
1. `make install` - install the required dependencies.
2. `make test` - runs the tests.

## Installation

### For Dev work on the repo

Install `uv`, (_if you haven't already_)
https://docs.astral.sh/uv/getting-started/installation/#installation-methods
```shell
brew install uv
```

Initialise pre-commit (validates ruff on commit.)
```shell
uv run pre-commit install
```

Install dependencies (including dev dependencies)
```shell
uv sync
```

If you are adding a new dev dependency, please run:
```shell
uv add --dev {your-new-package}
```

### Namespaces

Packages all share the same namespace `dape`. To import this package into your project:

```python
from dape.template import placeholder_func
```

We encourage you to make your package available to all of dape via this `dape` namespace. The goal is to streamline development, POCs and overall collaboration.

---

## Usage

### Adding the dependency to your project
The library is available on Nexus. You can install it using the following command:

**Using pip**:

```shell
pip install --index-url https://nexus3.auiag.corp/repos/repository/ddo-pypi/ python-package-template
```

**Using UV**

Note: there is currently no nice way like poetry, hence we still needd to provide the full url.
https://github.com/astral-sh/uv/issues/10140

Add the dependency
```shell
uv add --index nexus=https://nexus3.auiag.corp/repos/repository/ddo-pypi/simple/ python-package-template
```

You should see the following added to your `pyproject.toml` file:
```toml
[[tool.uv.index]]
name    = "nexus"
url     = "https://nexus3.auiag.corp/repos/repository/ddo-pypi/simple"
explicit = true
```

Andd the package is pinned to that nexus index.
```toml
[tool.uv.sources]
python-package-template = { index = "nexus" }
```

**Using poetry**:

Add this to the `pyproject.toml` file:
```toml
[[tool.poetry.source]]
name = "nexus"
url = "https://nexus3.auiag.corp/repos/repository/ddo-pypi/simple"
priority = "supplemental"
```

Then run the following command to install the package:
```shell
poetry add --source nexus python-package-template
```

### How tos

**Example Usage**

```python
# Please update this based on your package!

from dape.template import placeholder_func

if __name__ == "__main__":
    print("This is a placeholder: ", placeholdder_func())
```

---

## Formatting and linting

We use Ruff as the formatter and linter.
The pre-commit has hooks which runs checking and applies linting automatically.
The CI validates the linting, ensuring main is always looking clean.

You can manually use these commands too:
1. `make lint` - check for linting issues.
2. `make format` - fix linting issues.

---

## CICD

### Publishing to Nexus

We publish to nexus using Github releases. Steps are as follows:

1. Manually update the version in `pyproject.toml` file using a PR and merge to main. Use `uv version --bump {patch/minor/major}` to update the version.
2. Create a new release in Github with the tag name as the version number. This will trigger the `publish` workflow. In the Release window, type in the version number and it will prompt to create a new tag.
3. Verify the release in [Nexus](https://nexus3.auiag.corp/repos/#browse/browse:ddo-pypi-internal:python-package-template)

---

## Credits
This template repository has taken inspiration from the following repositories.
- [lumi](https://github.com/iag-dape/lumi)
- [casi-ragnarok](https://github.com/iag-dape/casi-ragnarok)
- [full-stack-fastapi-template](https://github.com/fastapi/full-stack-fastapi-template)
