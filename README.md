# python-template

Copier template for Python projects with uv, ruff, ty, pytest, just, and GitHub Actions CI.

## What's included

| Tool | Purpose |
|---|---|
| [uv](https://docs.astral.sh/uv/) | Package management, virtualenv, Python installs |
| [just](https://github.com/casey/just) | Task runner (`just check` runs everything) |
| [ruff](https://docs.astral.sh/ruff/) | Linting and formatting |
| [ty](https://docs.astral.sh/ty/) | Type checking |
| [pytest](https://docs.pytest.org/) | Testing with coverage |
| [GitHub Actions](.github/workflows/ci.yml.jinja) | CI on push/PR to main |
| [pre-commit](https://pre-commit.com/) | Git hooks for ruff + ty on commit |
| [Codecov](https://codecov.io) | Coverage reporting |

## Usage

### With copier (recommended)

Auto-populates project name, author, package name, etc:

```bash
uv tool install copier
copier copy --trust gh:aligfellow/python-template my-new-project
cd my-new-project
just setup && just check
```

The `--trust` flag is required because the template runs `git init` and creates an initial commit automatically.

Then create a GitHub repo and push:

```bash
gh repo create my-new-project --public --source=. --push
```

Or if you prefer to create the repo on GitHub first:

```bash
git remote add origin git@github.com:your-user/my-new-project.git
git push -u origin main
```

### As a GitHub template

Plain file copy — you rename things manually:

1. Click **"Use this template"** on this repo page, or:
   ```bash
   gh repo create my-new-project --template aligfellow/python-template --public --clone
   ```
2. Rename `src/{{ package_name }}/` to your actual package name
3. Update `name`, `description`, `authors`, and `homepage` in `pyproject.toml`
4. Update `source` in `[tool.coverage.run]` to match your package name
5. Update `LICENSE` with your name and year

## What you get

```
my-new-project/
├── src/
│   └── my_package/
│       ├── __init__.py
│       └── cli.py          # if include_cli=true
├── tests/
│   ├── __init__.py
│   └── test_import.py
├── .github/workflows/
│   └── ci.yml
├── pyproject.toml           # ruff, ty, pytest, coverage config
├── justfile                 # check/lint/type/test/fix/build/setup
├── .pre-commit-config.yaml  # ruff on commit
├── CITATION.cff             # if include_citation=true
├── .gitignore
├── LICENSE
└── README.md
```

## Copier questions

| Question | Default |
|---|---|
| `project_name` | — |
| `package_name` | derived from project name |
| `description` | — |
| `author` | Dr Alister Goodfellow |
| `github_user` | aligfellow |
| `year` | 2026 |
| `python_version` | 3.10 |
| `ci_python_version` | 3.14 |
| `include_cli` | false |
| `include_citation` | false |

## Updating existing projects

If a project was created with `copier copy`, pull in template improvements with:

```bash
copier update --trust
```

`--trust` is required because the template defines tasks (used for `git init` on first copy). The tasks don't run during update, but copier requires trust for any template that declares them.

## Codecov setup

For repo, go to codecov.io and activate the repository under **Configuration > General**. Copy the `CODECOV_TOKEN` and add as a repository secret under **Settings > Secrets and variables > Actions**, naming the secret `CODECOV_TOKEN`.

## License

[MIT](LICENSE.jinja)
