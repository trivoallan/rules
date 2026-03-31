# Integration Guide

Step-by-step instructions for adopting these standards in an existing Python project.

## Prerequisites

- Python 3.10+
- pipenv (`pip install pipenv`)
- Git repository initialized

---

## Step 1 — Copy agent configuration files

```bash
git clone https://github.com/trivoallan/rules.git agent-rules

# Claude Code / Cowork
cp agent-rules/CLAUDE.md your-project/

# Cursor
cp agent-rules/.cursorrules your-project/

# Windsurf
cp agent-rules/.windsurfrules your-project/

# GitHub Copilot
mkdir -p your-project/.github
cp agent-rules/.github/copilot-instructions.md your-project/.github/

# Optional: detailed reference guides
cp -r agent-rules/rules/ your-project/docs/coding-standards/
```

## Step 2 — Configure your pyproject.toml

Ensure your `pyproject.toml` includes these tool configurations:

```toml
[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[tool.ruff]
line-length = 88
target-version = "py310"
select = ["E", "F", "I", "N", "W", "UP", "B", "C4", "RUF"]

[tool.mypy]
python_version = "3.10"
strict = true
disallow_untyped_defs = true
disallow_any_explicit = true

[tool.pytest.ini_options]
testpaths = ["tests"]
addopts = ["--cov=src", "--cov-fail-under=80"]
```

A complete example is available in [`pyproject.toml.example`](pyproject.toml.example).

## Step 3 — Update your CI/CD pipeline

Add these steps to your GitHub Actions workflow:

```yaml
- name: Trunk Check
  uses: trunk-io/trunk-action@v1

- name: Tests
  run: pytest --cov=src --cov-fail-under=80
```

Trunk wraps Ruff, mypy, and other configured linters/formatters in a single step.
For automated releases, add the [Release Please action](https://github.com/googleapis/release-please-action).

## Step 4 — Set up commit scopes

Project-specific commit scopes are defined in [`PROJECT_SCOPES.md`](PROJECT_SCOPES.md). Copy and adapt this file:

```bash
cp agent-rules/PROJECT_SCOPES.md your-project/
```

Then update the scope list to match your project's architecture. To enforce scopes automatically:

```bash
# Install commitlint
npm install --save-dev @commitlint/cli @commitlint/config-conventional
```

```js
// commitlint.config.js
module.exports = {
  extends: ['@commitlint/config-conventional'],
  rules: {
    'scope-enum': [2, 'always', [
      'cli', 'schema', 'registry',
      // Add your scopes here
    ]],
    'scope-empty': [2, 'never'],
  },
};
```

## Step 5 — Communicate to the team

1. Share [`CLAUDE.md`](CLAUDE.md) and [`QUICK_REFERENCE.md`](QUICK_REFERENCE.md) with your team
2. Review the commit scopes list together ([`PROJECT_SCOPES.md`](PROJECT_SCOPES.md))
3. Set up branch protection rules on GitHub (require CI + 1 review)

**Gradual rollout:**

- **Week 1**: Education — everyone reads the standards, Q&A on commit scopes
- **Week 2–3**: Soft enforcement — CI runs but doesn't block merges, feedback via code review
- **Week 4+**: Full enforcement — CI blocks failing merges, all new PRs follow standards

---

## Customizing the rules

### Changing line length

1. Update `pyproject.toml`: `line-length = 100`
2. Document the override in `CLAUDE.md` under a `## Project Overrides` section with the reason

### Adding a new commit scope

1. Add the scope to `PROJECT_SCOPES.md` with a description
2. Update `commitlint.config.js` if used
3. Commit: `docs(project-scopes): add analyzer/foo scope`

### Relaxing mypy strictness for legacy code

```markdown
## Project Overrides

### Type checking
- `disallow_incomplete_defs = false` for legacy modules under `src/legacy/`
- Reason: gradual migration — target full strict mode by Q3 2026
```

---

## Ongoing maintenance

- **On each significant architecture change**: update `rules/` and agent configs
- **Quarterly**: review commit scopes, remove unused ones, gather team feedback
- **On tool upgrades** (Ruff, mypy, pytest): update `pyproject.toml.example` and CI configs
