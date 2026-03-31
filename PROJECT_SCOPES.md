# Project Commit Scopes

All commits to this project must use one of the defined scopes below. Scopes map to architectural components and enable clean, readable changelogs.

## Core & Logic

| Scope | Component | Examples |
|-------|-----------|----------|
| `cli` | Command-line interface, argument parsing, console output | User input handling, argument validation |
| `playbook` | Rule evaluation engine, section parsing, context management | Playbook execution logic, rule resolution |
| `schema` | Data interfaces, structure definition, JSON schema files | Model definitions, validation schemas |
| `registry` | Registry communication layer (HTTP, auth, manifest) | API calls, authentication, manifest fetching |

## Analyzers

| Scope | Component | Examples |
|-------|-----------|----------|
| `analyzer` | Base analyzer class or shared analyzer interfaces | Abstract methods, shared logic |
| `analyzer/trivy` | Vulnerability scanning and SBOM via Trivy | CVE detection, vulnerability parsing |
| `analyzer/sbom` | SBOM analysis and CycloneDX/SPDX generation | Format conversion, dependency resolution |
| `analyzer/hadolint` | Dockerfile linting | Build best practices, Dockerfile validation |
| `analyzer/skopeo` | Base image metadata extraction | Image inspection, tag resolution |
| `analyzer/freshness` | Image age and freshness score calculations | Date calculations, scoring logic |
| `analyzer/size` | Size and layer calculations | Layer analysis, size metrics |
| `analyzer/popularity` | Registry popularity metrics | Download stats, popularity scoring |
| `analyzer/endoflife` | Version support status | EOL checking, version lifecycle |
| `analyzer/scorecarddev` | OpenSSF Scorecard checks | Security scoring, compliance checks |
| `analyzer/provenance` | Provenance and supply chain evidence | SLSA verification, attestation handling |

## Rendering & Reporting

| Scope | Component | Examples |
|-------|-----------|----------|
| `report` | Report generation logic (folder creation, file writing) | Output formatting, report assembly |
| `templates` | Visual aspects, HTML structure, CSS stylesheets, Jinja2 | HTML markup, styling, templating |

## Tooling & CI

| Scope | Component | Examples |
|-------|-----------|----------|
| `ci` | GitHub Actions workflows (Super-Linter, Release Please) | Automation, CI configuration |
| `deps` | Environment management (Pipenv, pyproject.toml, Dockerfile) | Dependencies, build configuration |
| `docs` | Antora/Docusaurus documentation, READMEs, memory bank | User guides, API docs, memory updates |

## Usage Rules

### Choosing a Scope

1. **Identify the component** you modified (CLI, analyzer, report, etc.)
2. **Check the table above** for the corresponding scope
3. **Use that scope** in your commit message

### Format

```
<type>(<scope>): <subject>
```

Examples:

```
feat(cli): add --dry-run flag
fix(analyzer/trivy): handle missing CVE data
feat(templates): add dark mode CSS
chore(deps): upgrade Trivy to v0.48
docs(memory-bank): document analyzer architecture
refactor(playbook): simplify rule evaluation logic
```

### Subscopes

For complex components, use a subscope separator (`/`):

```
feat(analyzer/trivy): add CVSS severity filtering
fix(templates/html): correct grid layout
docs(memory-bank/architecture): update C4 diagram
```

### Multiple Components?

**Split into separate commits** (one scope per commit):

```
# ❌ Wrong
feat(cli,report): add flag and update template

# ✅ Correct (two commits)
commit 1: feat(cli): add --filter-by-severity flag
commit 2: feat(report): update report template for new flag
```

### No Clear Scope?

Use `chore` for utility changes:

```
chore: remove unused imports
chore: fix typo in docstring
chore: update .gitignore
```

## Functional vs Technical Description

The **subject line** should describe **what it does** (functional), not **how** (technical):

```
# ✅ Good — functional, clear in changelog
feat(cli): add --dry-run flag to preview changes without applying them

# ❌ Avoid — technical, unclear in changelog
feat(cli): add argparse argument for preview mode
```

**Technical details belong in the body:**

```
feat(analyzer/trivy): support SBOM output formats

Adds CycloneDX and SPDX format support for SBOM generation.
Implemented using the existing format abstraction layer.

Closes #123
See docs/sbom-formats.md for format specifications.
```

## Enforcing Scopes

### Using commitlint

Configure `.commitlintrc.js` to enforce scope requirements:

```javascript
module.exports = {
  extends: ['@commitlint/config-conventional'],
  rules: {
    'scope-enum': [2, 'always', [
      'cli', 'playbook', 'schema', 'registry',
      'analyzer', 'analyzer/trivy', 'analyzer/sbom', 'analyzer/hadolint',
      'analyzer/skopeo', 'analyzer/freshness', 'analyzer/size',
      'analyzer/popularity', 'analyzer/endoflife', 'analyzer/scorecarddev',
      'analyzer/provenance',
      'report', 'templates',
      'ci', 'deps', 'docs'
    ]],
    'scope-empty': [2, 'never'],
  }
};
```

### Pre-commit Hook

```bash
#!/bin/bash
# .git/hooks/commit-msg

npx commitlint --edit $1
```

## Changelog Generation

Conventional Commits enable automatic changelog generation:

- `feat:` sections are grouped by scope
- `fix:` sections are grouped by scope
- `Breaking changes:` flagged prominently
- Scopes make changelogs **readable and organized**

Example generated changelog:

```
## [1.2.0] - 2026-04-15

### Features

#### analyzer/trivy
- Add CVSS severity filtering (#234)
- Support CVE suppression lists (#235)

#### cli
- Add --dry-run flag (#236)
- Add --filter-by-severity option (#237)

#### templates
- Add dark mode CSS (#238)

### Bug Fixes

#### analyzer/sbom
- Fix SPDX format generation (#239)

#### report
- Correct report generation for empty results (#240)

### Chores

- Upgrade Trivy to v0.48 (#241)
- Update dependencies (#242)
```

## Questions?

- **"What scope should I use?"** — Find your component in the table above
- **"My change touches multiple scopes?"** — Create multiple commits, one per scope
- **"Scope not in the list?"** — Ask the team lead or propose a new scope
- **"What if I get it wrong?"** — Amend and push: `git commit --amend` then `git push --force-with-lease`

---

Last updated: 2026-03-31

This scope list is **definitive** for your project. All team members must follow these scopes.
