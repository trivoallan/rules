# Claude Code Rules — Production Python Project Standards

Comprehensive coding standards for Python projects, including project-specific conventions. Based on industry standards (PEP 8, Google Style Guide, Conventional Commits).

## Project Context

- **Language**: Python 3.10+
- **Package Manager**: **pipenv** (Pipfile + Pipfile.lock)
- **Linter/Formatter**: Ruff (88 char lines)
- **Type Checker**: mypy (strict mode, 100% coverage target)
- **Testing Framework**: pytest (80%+ coverage minimum)
- **VCS**: Git with Conventional Commits (mandatory scopes)
- **Documentation**: Docusaurus + memory bank
- **CI/CD**: GitHub Actions + Release Please + Super Linter
- **Diagrams**: Mermaid (C4 model preferred)
- **Philosophy**: Prefer established libraries, Python first

---

## Core Rules

### Style & Formatting

- Use **Ruff** for all formatting and linting
- Line length: **88 characters** maximum (Black/Ruff standard)
- Indentation: **4 spaces only**
- Naming: **PascalCase** (classes), **snake_case** (functions/variables), **UPPER_SNAKE_CASE** (constants)
- Docstrings: **Google style** for all public functions/classes
- Imports: Organized by stdlib → third-party → local (Ruff handles automatically)

### Type Safety

- **All code must be fully typed** — target 100% mypy coverage
- Use Python 3.10+ modern syntax: `list[T]`, `dict[K, V]`, `str | None` (not `List`, `Dict`, `Optional`)
- Use **Pydantic v2** for data validation at system boundaries
- Never use `Any` without justification; prefer specific types or `Protocol`
- **mypy in strict mode** — all implicit `Any` errors

### Architecture

- **Source layout**: `src/myproject/` (Python Packaging Authority standard)
- **Single Responsibility Principle** — one responsibility per class
- **Dependency Injection** — avoid global state, inject dependencies
- **Custom Exceptions** — create domain-specific exceptions (inherit from base `DomainError`)
- **Logging**: Standard library `logging` module only (never `print()` in production)
- **Package Management**: **pipenv exclusively** — never use pip directly
  - Maintain both `Pipfile` (flexible versions) and `Pipfile.lock` (reproducible)
  - Commit both files to version control

### Git & Commits

- Follow **Conventional Commits** format: `<type>(<scope>): <subject>`
- **Scopes are mandatory** — must represent the architectural component modified
- Types: `feat`, `fix`, `docs`, `style`, `refactor`, `perf`, `test`, `chore`
- Atomic commits: one logical change = one commit
- Present tense: "add feature" not "added feature"
- **Subject is functional** (what it does), technical details in body

### Commit Scopes (Your Project)

**All commits must use one of these scopes:**

**Core & Logic:**
- `cli` — Command-line interface, argument parsing
- `playbook` — Rule evaluation engine, section parsing
- `schema` — Data interfaces, JSON schema validation
- `registry` — Registry communication (HTTP, auth, manifests)

**Analyzers:**
- `analyzer` — Base class or shared interfaces
- `analyzer/trivy` — Vulnerability scanning via Trivy
- `analyzer/sbom` — SBOM analysis (CycloneDX, SPDX)
- `analyzer/hadolint` — Dockerfile linting
- `analyzer/skopeo` — Base image metadata
- `analyzer/freshness` — Image age scoring
- `analyzer/size` — Layer/size analysis
- `analyzer/popularity` — Registry popularity metrics
- `analyzer/endoflife` — Version support status
- `analyzer/scorecarddev` — OpenSSF Scorecard
- `analyzer/provenance` — Supply chain evidence

**Rendering & Reporting:**
- `report` — Report generation logic
- `templates` — HTML, CSS, Jinja2 templates

**Tooling:**
- `ci` — GitHub Actions workflows
- `deps` — Pipenv, pyproject.toml, Dockerfiles
- `docs` — Docusaurus, memory bank, READMEs

### Pull Requests

- **One change per PR** (feature, bugfix, refactor)
- **Keep under 400 lines** for reviewability
- **Clear description**: what, why, test plan, related issues
- **Pass all CI checks** before merge
- **Require code review** (1+ approvals minimum)
- **Squash or rebase** for clean history

### Testing

- **Framework**: pytest
- **Coverage**: Minimum 80% (100% for critical code)
- **Organization**: tests/unit, tests/integration, tests/e2e
- **FIRST principles**: Fast, Independent, Repeatable, Self-checking, Timely
- **Fixtures**: Use conftest.py for shared setup
- **Mocking**: Mock external dependencies only, test code normally

### Code Review

**Checklist:**
- [ ] Logic is correct (solves the problem, no bugs)
- [ ] No N+1 database queries or performance issues
- [ ] No hardcoded secrets or security issues (SQL injection, XSS)
- [ ] Input validation present at API boundaries
- [ ] Tests cover happy path and edge cases
- [ ] Type annotations complete
- [ ] Documentation updated (if needed)
- [ ] Naming is clear
- [ ] Error handling is appropriate

### Documentation

- **Framework**: Docusaurus (markdown-based)
- **Location**: `docs/` directory with `docusaurus.config.js`
- **Structure**: Getting started, guides, API, analyzers, memory bank
- **Memory Bank**: Update `docs/memory-bank/` after significant changes (decisions, patterns, known issues, status)
- **Diagrams**: Mermaid format (C4 model preferred)
- **Style**: Google Style Guide for writing
- **Root README**: Brief intro + links to detailed docs (not detailed docs)

### CI/CD

- **Platform**: GitHub Actions only
- **Release**: Release Please for semantic versioning
- **Quality**: Super Linter for all PR checks
- **Versioning**: Semantic Versioning (semver.org)
- **Changelog**: Auto-generated from commit messages

### Code Philosophy

- **Prefer established libraries** over building from scratch
- **Prefer Python** over JavaScript/Node when possible
- **Document why** you chose a particular library

---

## Quick Checklist

Before committing:

```bash
# Format and lint
ruff format .
ruff check . --fix

# Type check
mypy src/

# Run tests
pytest --cov=src --cov-fail-under=80

# Verify commit message
# Type: feat/fix/docs/style/refactor/perf/test/chore
# Scope: One from PROJECT_SCOPES.md
# Subject: Present tense, functional, < 72 chars

# Then push
git push origin feature/my-feature
```

Before submitting PR:

- [ ] All checks pass locally (`ruff`, `mypy`, `pytest`)
- [ ] Commit messages use correct scope
- [ ] Description explains what and why
- [ ] Tests added/updated
- [ ] Documentation updated (if needed)
- [ ] Memory bank updated (if major change)

---

## When to Ask Questions

Ask clarifying questions if:
- The style guide doesn't address a specific scenario
- Trade-offs between conventions are unclear
- You're uncertain how to structure a complex module
- Unclear which scope to use for a commit (see PROJECT_SCOPES.md)

---

## Reference Documents

- **PROJECT_SCOPES.md** — Definitive list of commit scopes
- **Quick Reference** — Cheat sheet for commands, patterns, examples
- **Detailed Guides**:
  - `rules/python/style.md` — Formatting, naming, docstrings
  - `rules/python/typing.md` — Type annotations, Pydantic
  - `rules/python/architecture.md` — Structure, DI, exceptions, logging
  - `rules/workflow/git.md` — Git Flow, branching, PRs
  - `rules/workflow/testing.md` — pytest, fixtures, coverage
  - `rules/workflow/code-review.md` — Review standards, security
  - `rules/workflow/documentation.md` — Docusaurus, memory bank, Mermaid
  - `rules/workflow/commit-scopes.md` — Conventional Commits guide

---

**Last Updated**: 2026-03-31
**Status**: ✅ Production Ready
**Python Target**: 3.10+
**Standards**: PEP 8, Google Style Guide, Conventional Commits, Python Packaging Authority
