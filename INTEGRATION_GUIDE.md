# Integration Guide: Merging Agent Rules into Your Project

This guide explains how to integrate the agent rules repository with your existing project rules.

## Overview

You have two sets of rules:
1. **Base agent rules** (generic Python standards)
2. **Your project rules** (specific to your analyzers project)

Both are now unified in this repository:
- **Base rules**: `rules/` directory
- **Project rules**: `PROJECT_RULES.md` file
- **Project config**: Updated `CLAUDE.md`, `.cursorrules`, etc.

## What's New

### New Base Rules
The repository includes comprehensive documentation on:
- Python style, typing, architecture
- Testing, code review, git workflow
- CI/CD best practices

### Project-Specific Additions
`PROJECT_RULES.md` documents:
- pipenv as package manager (not pip)
- Defined commit scopes (cli, analyzer/*, templates, etc.)
- Antora documentation setup
- GitHub Actions + Release Please workflow
- Memory bank maintenance
- C4 diagram format

### Unified Configuration Files
- **CLAUDE.md** — Updated with project context
- **.cursorrules** — For Cursor IDE
- **.windsurfrules** — For Windsurf
- **.github/copilot-instructions.md** — For GitHub Copilot

## Step-by-Step Integration

### Step 1: Copy Configuration Files to Your Project

```bash
# Copy agent configuration files
cp agent-rules/CLAUDE.md your-project/
cp agent-rules/.cursorrules your-project/
cp agent-rules/.windsurfrules your-project/
cp agent-rules/.github/copilot-instructions.md your-project/.github/

# Copy project rules
cp agent-rules/PROJECT_RULES.md your-project/

# Copy documentation templates
cp agent-rules/QUICK_REFERENCE.md your-project/docs/
cp agent-rules/DEPLOYMENT.md your-project/docs/
```

### Step 2: Update Your README

Add a section linking to these standards:

```markdown
## Coding Standards

We follow the [Agent Rules](./CLAUDE.md) standards for all Python code.

**Quick references:**
- [Quick Reference](./QUICK_REFERENCE.md) — Cheat sheet
- [Project Rules](./PROJECT_RULES.md) — Project-specific standards
- [Commit Scopes](./PROJECT_RULES.md#commit-message-scopes) — Allowed commit scopes
- [Documentation Guide](./PROJECT_RULES.md#documentation) — How to document

For detailed guidance, see the `docs/coding-standards/` directory.
```

### Step 3: Ensure Your pyproject.toml Matches

Make sure your `pyproject.toml` includes:

```toml
[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[tool.ruff]
line-length = 88
target-version = "py310"

[tool.mypy]
python_version = "3.10"
strict = true
disallow_untyped_defs = true

[tool.pytest.ini_options]
testpaths = ["tests"]
addopts = ["--cov=src", "--cov-fail-under=80"]
```

### Step 4: Update Your CI/CD

If you're using GitHub Actions, ensure workflows include:

```yaml
- name: Lint with Ruff
  run: ruff check . --fix

- name: Type check with mypy
  run: mypy src/

- name: Test with pytest
  run: pytest --cov=src --cov-fail-under=80

- name: Run Super Linter
  uses: super-linter/super-linter@v5
```

### Step 5: Team Communication

1. **Announce the new standards** in team meeting/Slack
2. **Share** `PROJECT_RULES.md` focusing on:
   - Commit scope list
   - Documentation requirements
   - CI/CD expectations
3. **Share** `QUICK_REFERENCE.md` for daily use
4. **Answer questions** about scope assignment

### Step 6: Gradual Enforcement

**Week 1: Education**
- Everyone reads `PROJECT_RULES.md`
- Review scope list as a team
- Q&A on commit messages

**Week 2-3: Soft Enforcement**
- Run CI checks but don't block merges yet
- Use code review to teach, not reject
- Fix obvious issues (formatting, scopes)

**Week 4+: Full Enforcement**
- CI checks now block failing merges
- All new PRs follow standards
- Legacy code gradually cleaned up

## Mapping Your Existing Conventions

Your uploaded files mapped to the new structure:

| Your File | Integrated Into |
|-----------|-----------------|
| `python.md` | `rules/python/`, `PROJECT_RULES.md` |
| `git-workflow.md` | `rules/workflow/git.md`, `PROJECT_RULES.md` |
| `commitmessages.md` | `PROJECT_RULES.md` (commit scopes section) |
| `cicd.md` | `PROJECT_RULES.md` (CI/CD section) |
| `documentation.md` | `PROJECT_RULES.md` (documentation section) |
| `devcontainers.md` | `PROJECT_RULES.md` (devcontainers section) |
| `diagrams.md` | `PROJECT_RULES.md` (diagram guidelines section) |
| `craftmanship.md` | `PROJECT_RULES.md` (philosophy section) |
| `htmlcss.md` | (not used in this Python project, but documented) |

## Customization Examples

### Adding a New Analyzer Scope

When you add a new analyzer `foo`:

1. Add to `PROJECT_RULES.md`:
```markdown
- `analyzer/foo` : Description of what your analyzer does
```

2. Update this file to reflect the change
3. Commit with scope `docs:`
```
docs(project-rules): add analyzer/foo scope
```

### Modifying a Standard

If your team decides to change line length from 88 to 100:

1. Update `pyproject.toml`:
```toml
[tool.ruff]
line-length = 100
```

2. Add exception to your `CLAUDE.md`:
```markdown
# Project Overrides

- **Line length**: 100 characters (vs 88 in base rules)
  Reason: Increased readability for your domain-specific code
```

3. Commit:
```
chore(config): change Ruff line length to 100
```

## Conflict Resolution

When base rules and project rules conflict:

1. **Project rules take priority** for your project
2. Document the exception in `PROJECT_RULES.md`
3. Explain why in a comment
4. Commit the change

Example:

```markdown
## Overrides from Base Rules

### Type Checking Strictness
- We use `disallow_incomplete_defs = false` for legacy analyzers
- Reason: Gradual type checking migration
- Timeline: Will enforce strict mode by Q3 2026
```

## Ongoing Maintenance

### Keep Rules in Sync

When you update practices:

1. **Update relevant files** (CLAUDE.md, PROJECT_RULES.md, etc.)
2. **Update this guide** if adding new sections
3. **Communicate** changes to team
4. **Enforce** in code review

### Regular Reviews

Every quarter:
- Review commit scopes for unused/unclear ones
- Check if rules still match reality
- Update documentation
- Gather team feedback

## Questions?

Refer to:
- **Quick answers**: `QUICK_REFERENCE.md`
- **Detailed guides**: `rules/` directory
- **Project specifics**: `PROJECT_RULES.md`
- **How to use**: `DEPLOYMENT.md`

---

**Integration Status**: ✅ Complete

Your project now has:
- Base industry standards (rules/)
- Project-specific customizations (PROJECT_RULES.md)
- Agent configurations (CLAUDE.md, .cursorrules, etc.)
- Quick references (QUICK_REFERENCE.md)
- Deployment guides (DEPLOYMENT.md)

Ready to use!
