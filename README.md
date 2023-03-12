# pre-commit hooks to run tools with Poetry

This repository contains [pre-commit](https://pre-commit.com) hooks to invoke
various Python-related hooks through [Poetry](https://python-poetry.org). With
these hooks, you can manage your dev dependencies via a single `poetry update`
command.

## Why not use the typical pre-commit hooks for these tools?

pre-commit usually creates isolated environments for each tool. This approach
works well for tools that are implemented in other languages - for example,
tools for Python that are written in TypeScript - or that have dependencies
that may conflict with the repo. However, there are two pain points you might
encounter:

1. Tools that require access to the full set of dependencies to function well,
such as type checkers, may not be able to find these dependencies without
specially configuring pre-commit to also add these dependencies to the tool's
isolated environment.
2. If you also install these tools some other way (such as dev dependencies),
nothing keeps the tool version in sync between your pre-commit config and your
other method of installing them.

If you use Poetry, and either of these points apply to you, you may find it
easier to use these hooks and let Poetry manage the tools.

## How to use these hooks?

To use these hooks, add the following to your `.pre-commit-config.yaml`:
```
- repo: https://github.com/wlritchi/pre-commit-tools-via-poetry
  rev: '1.0.0'
  hooks:
  - id: autoflake
  - id: isort
  # etc
```

Additionally, you will need to add these tools to your dev dependencies if you
don't already have them installed; unlike most pre-commit hooks, these hooks
will not install them for you.

The configuration and defaults for each hook are generally intended to mirror
the upstream hooks for that tool, if they exist.

## Supported hooks

- [autoflake](https://pypi.org/project/autoflake/)
- [black](https://black.readthedocs.io/en/stable/) *see notes
- [flake8](https://flake8.pycqa.org/en/latest/)
- [isort](https://pycqa.github.io/isort/)
- [mypy](https://www.mypy-lang.org/)
- [pyright](https://github.com/Microsoft/pyright) *see notes

## Hook-specific notes

### Black

Black includes optional support for formatting Jupyter notebooks. To include
this support, install with `black[jupyter]` and use the `black-jupyter` hook.

### Pyright

Pyright is not implemented in Python and does not come with its own pre-commit
hook. However, [a Python wrapper](https://pypi.org/project/pyright) exists, and
running the tool through Poetry using this wrapper may allow Pyright to locate
your dependencies more easily and provide more complete type checking results.
To use the Pyright hook, you will to add this wrapper to your dev dependencies,
or require that all developers have Pyright installed on their systems through
some other means.
