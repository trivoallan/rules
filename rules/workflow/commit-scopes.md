# Commit Message Scopes & Types

## Conventional Commits Format

All commits must follow [Conventional Commits](https://www.conventionalcommits.org/) specification:

```
<type>(<scope>): <subject>

<body>

<footer>
```

## Allowed Types

Types from [Angular Convention](https://github.com/angular/angular/blob/main/CONTRIBUTING.md#type):

- **feat** — A new feature
- **fix** — A bug fix
- **docs** — Documentation only changes
- **style** — Changes that do not affect code meaning (formatting, missing semicolons, etc.)
- **refactor** — Code change that neither fixes a bug nor adds a feature
- **perf** — Code change that improves performance
- **test** — Adding missing tests or correcting existing tests
- **chore** — Changes to build process, dependencies, or tooling (deps, build, ci)

## Scope Rules

**Scopes are mandatory** and must represent the architectural component being modified.

### Choosing a Scope

1. Look at what files you changed
2. Map them to the architectural component they belong to
3. Use the appropriate scope from your project's scope list

Example: If you modify `src/cli/main.py`, scope = `cli`

### Scope Guidelines

- **Be specific**: `cli` not `code` or `changes`
- **Be consistent**: Use existing scopes from the project
- **Be functional**: Think about the feature/component, not the file type

## Functional vs Technical Description

**Description must be:**
- Easy to understand in changelogs
- Functional in focus (what does it do?)
- Link to documentation when relevant

**Example:**

```
# ✅ Good — functional, clear in changelog
feat(cli): add --dry-run flag to preview changes without applying them

# ❌ Avoid — technical, not clear in changelog
feat(cli): add argparse argument for preview mode
```

**Body reserved for:**
- Technical implementation details
- Why the change was needed
- Links to issues/documentation

Example:

```
feat(analyzer/trivy): support SBOM output formats

Adds CycloneDX and SPDX format support for SBOM generation.

Closes #123
See docs/sbom-formats.md for details
```

## Anatomy of a Good Commit Message

```
feat(scope): Brief functional description (present tense)
                                                        ↑
                                            Max 72 characters


Longer explanation if needed. Explain what and why, not how.
Reference technical details only if necessary. This part can
be multiple paragraphs and should help reviewers understand
the business impact or rationale.

Closes #123
Related: #456
See: https://docs.example.com/feature-x
```

## Examples by Type

### Feature
```
feat(analyzer): add vulnerability filtering by severity level

Users can now filter detected vulnerabilities by CVSS severity.
Implemented using existing security filters system.

Closes #89
```

### Bug Fix
```
fix(report): correct report generation when image has no tags

Previously, reports would fail if the image had no tags. Now
we gracefully handle the empty tag case.

Fixes #234
```

### Documentation
```
docs(memory-bank): update deployment section for Kubernetes

Added new Kubernetes deployment guidance to reflect recent
infrastructure changes.

Related: #456
```

### Refactor
```
refactor(schema): simplify validation interface

Consolidated 3 validation functions into a unified interface
without changing external behavior. Improves maintainability.
```

### Chore / Dependencies
```
chore(deps): upgrade ruff to 0.2.0

Updates Ruff formatter and linter to latest version for
improved performance.
```

## Scope Format Rules

- Use lowercase: `cli` not `CLI`
- Use hyphens for multi-word: `analyzer-config` not `analyzerConfig`
- Keep concise: `cli` not `command-line-interface`
- Be specific: `analyzer/trivy` not just `analyzer`

## Subscopes

For complex components, use subscopes:

```
feat(analyzer/trivy): add CVSS severity filtering
fix(templates/html): correct CSS grid layout
docs(memory-bank): update architecture section
```

## Special Cases

### Multiple Scopes?
Split into separate commits:
```
# Instead of:
feat(cli,report): add flags and update template

# Do:
commit 1: feat(cli): add dry-run flag
commit 2: feat(report): update template for new flag
```

### No Clear Scope?
Use `chore` for utility changes:
```
chore: clean up unused imports
chore: fix typo in docstring
```

## Commit Message Checklist

Before committing, verify:

- [ ] Type is one of: feat, fix, docs, style, refactor, perf, test, chore
- [ ] Scope is specified (mandatory)
- [ ] Scope exists in the project scope list or is a clear extension
- [ ] Subject is in present tense ("add" not "added")
- [ ] Subject is concise (< 72 characters)
- [ ] Subject is functional (what it does, not how)
- [ ] Body explains why (if needed)
- [ ] Related issues are linked in footer

## Tooling

Use tools to enforce commit message standards:

```bash
# Install commitlint
npm install -g commitlint

# Configure in .commitlintrc.js
module.exports = {
  extends: ['@commitlint/config-conventional'],
  rules: {
    'scope-enum': [2, 'always', ['cli', 'analyzer', 'report', ...]]
  }
};

# Run pre-commit hook
echo 'commitlint --edit' > .git/hooks/commit-msg
chmod +x .git/hooks/commit-msg
```

## Changelog Generation

Conventional Commits enable automatic changelog generation:

- `feat:` → Features section
- `fix:` → Bug Fixes section
- `perf:` → Performance Improvements section
- Breaking changes (footer) → Breaking Changes section

This is why commit message quality matters!
