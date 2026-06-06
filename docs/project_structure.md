# Project Structure

After generating your package, you'll have a project that looks like this:

**TODO: Update the project_structure**
```
├───{{cookiecutter.project_name}}
│   .editorconfig
│   .gitignore
│   .python-version
│   CODE_OF_CONDUCT.md
│   LICENSE
│   pyproject.toml
│   uv.lock
│   zensical.toml
│   __init__.py
│   __main__.py
│
├───.github
│   │   dependabot.yml
│   │   pull_request_template.md
│   │
│   ├───ISSUE_TEMPLATE
│   │       bug_report.yml
│   │       config.yml
│   │       feature_request.yml
│   │
│   └───workflows
│           ci.yml
│           codeql.yml
│           docs.yml
│           publish.yml
│           zizmor.yml
│
├───CHANGELOG
├───docs
│       index.md
│       markdown.md
│
├───scripts
│       release.py
│       update_rc.py
│       utils.py
│       __init__.py
│
├───tests
└───{{cookiecutter.package_name}}
    │   __init__.py
    │   __main__.py
    │
    ├───cli
    ├───containers
    ├───core
    ├───gui
    │   │   resources.qrc
    │   │
    │   ├───icons
    │   └───pictures
    ├───templates
    │       resources.qrc
    │
    └───utilities
```

## Source code (`project_name/`)

The project uses a **project_name layout**, where your package code lives under `project_name/` rather than at the root. This prevents accidentally importing local code during testing, which is a common source of subtle bugs.

Put your code in `project_name/package_name/`. The `utils.py` file is a placeholder to get you started. Rename it, delete it, or add new modules alongside it.

## CLI (`cli.py`)

Your package includes a [Typer](https://typer.tiangolo.com/) command-line interface. After `uv sync`, you can run it three ways:

```bash
uv run my-package          # via the entry point
uv run my-package --help   # show help
uv run python -m my_package  # via __main__.py
```

The entry point is configured in `pyproject.toml` under `[project.scripts]`. To add commands, add new functions decorated with `@app.command()` in `cli.py`. See the [Typer docs](https://typer.tiangolo.com/tutorial/) for details.

## Justfile commands

Run `just list` to see all available commands. The key ones:

**TODO: Update to te current justfile commands**

| Command | What it does |
|---|---|
| `just qa` | Format, lint, type-check, and test (the daily driver) |
| `just test` | Run tests only |
| `just testall` | Run tests on Python 3.12, 3.13, and 3.14 |
| `just type-check` | Type-check with ty |
| `just type-check-watch` | Type-check in watch mode |
| `just docs-serve` | Preview docs locally at http://localhost:8000 |
| `just docs-build` | Build docs |
| `just coverage` | Run tests with coverage and generate HTML report |
| `just release` | Tag the current version and push to GitHub |
| `just build` | Build sdist and wheel |

## GitHub Actions workflows

Your project includes CI, publish, documentation, and Dependabot workflows, all security-hardened out of the box. See [GitHub Actions Workflows](github_actions.md) for details.

## Documentation site

The docs site is built with [Zensical](https://zensical.org/) and configured in `zensical.toml`. It uses the Material theme with light/dark mode.

The API reference page (`docs/api.md`) auto-generates documentation from your docstrings using [mkdocstrings](https://mkdocstrings.github.io/). Write docstrings in your code and they'll appear on the docs site automatically.

The post-generation hook enables GitHub Pages automatically. If it couldn't, go to your repo's Settings > Pages and set the source to **GitHub Actions**.

## Configuration (`pyproject.toml`)

Tool configuration lives in `pyproject.toml`:

- **Dependency groups**: `dev` (includes lint, test, typecheck), `docs` (zensical, mkdocstrings)
- **ruff**: line length 120, rules for pycodestyle, Pyflakes, isort, flake8-bugbear, pyupgrade
- **ty**: all rules enabled as errors by default. To relax a rule, uncomment the example in `[tool.ty]`
- **uv**: package mode enabled, dev groups installed by default
