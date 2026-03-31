# Documentation Index

Complete guide to all rules and resources in this repository.

## 📌 Start Here

1. **First time here?** → Read `README.md`
2. **Need quick answers?** → Check `QUICK_REFERENCE.md`
3. **Setting up a project?** → Copy `CLAUDE.md`, `.cursorrules`, etc. to your project
4. **Need detailed guidance?** → Browse specific guides below

## 📂 Repository Structure

```
agent-rules/
├── README.md                              ← Start here
├── QUICK_REFERENCE.md                     ← Cheat sheet
├── INDEX.md                               ← This file
│
├── CLAUDE.md                              ← For Claude Code
├── .cursorrules                           ← For Cursor
├── .windsurfrules                         ← For Windsurf
├── .github/
│   └── copilot-instructions.md           ← For GitHub Copilot
│
├── pyproject.toml.example                 ← Template config
│
└── rules/
    ├── python/
    │   ├── style.md                       ← Code formatting, naming, docstrings
    │   ├── typing.md                      ← Type annotations, safety
    │   └── architecture.md                ← Project structure, design patterns
    │
    └── workflow/
        ├── git.md                         ← Version control, commits, PRs
        ├── testing.md                     ← Tests, coverage, pytest
        └── code-review.md                 ← Review standards, security
```

## 🎯 Agent Configuration Files

### Use For
- **Claude Code / Cowork**: Copy `CLAUDE.md` to your project root
- **Cursor IDE**: Copy `.cursorrules` to your project root
- **GitHub Copilot**: Copy `.github/copilot-instructions.md` to your `.github/` folder
- **Windsurf**: Copy `.windsurfrules` to your project root

All contain the same core standards in agent-specific formats.

## 📖 Detailed Rule Guides

### Python Conventions

#### `rules/python/style.md` — Code Style & Formatting
Covers:
- Ruff configuration (88 char lines, 4-space indent)
- Naming conventions (PascalCase, snake_case, UPPER_SNAKE_CASE)
- PEP 8 compliance
- Google-style docstrings
- Import ordering

#### `rules/python/typing.md` — Type Safety
Covers:
- Full type annotation requirements (100% coverage)
- Python 3.10+ modern syntax (list[T], str | None)
- TypeVar and Protocol usage
- Pydantic v2 for data validation
- mypy/pyright configuration

#### `rules/python/architecture.md` — Structure & Design
Covers:
- src/ directory layout (Python Packaging Authority standard)
- Single Responsibility Principle
- Dependency injection patterns
- Custom exception hierarchies
- Logging best practices

### Workflow & Process

#### `rules/workflow/git.md` — Git & Version Control
Covers:
- Git Flow branching strategy
- Conventional Commits format
- Atomic, clear commit messages
- Pull request structure and guidelines
- Rebase vs merge strategies

#### `rules/workflow/testing.md` — Testing Strategy
Covers:
- FIRST principles (Fast, Independent, Repeatable, Self-checking, Timely)
- pytest framework and structure
- Unit, integration, E2E organization
- Coverage targets (80% minimum, 100% for critical code)
- Fixtures and mocking patterns
- Test naming conventions

#### `rules/workflow/code-review.md` — Code Review Standards
Covers:
- Review objectives and checklist
- Security concerns (SQL injection, secrets, XSS)
- Performance and testability checks
- Code ownership and CODEOWNERS
- Constructive review tone

## 🔧 Configuration Templates

### `pyproject.toml.example`
Complete example of proper Python project configuration including:
- Ruff formatter and linter settings
- mypy type checking configuration
- pytest configuration and markers
- Coverage settings (80% threshold)
- Build system and dependencies

Copy and customize for your project.

## 🚀 Quick Start Checklist

- [ ] Copy agent config file to project (`CLAUDE.md`, `.cursorrules`, etc.)
- [ ] Copy `pyproject.toml.example` and customize
- [ ] Run `ruff check . --fix` and `mypy src/`
- [ ] Add `pytest` with test fixtures
- [ ] Set up CI/CD to enforce standards
- [ ] Add `CODEOWNERS` file for code review
- [ ] Document any project-specific exceptions

## 🔍 Finding Specific Topics

### I need to...

| Topic | File |
|-------|------|
| Format my code | `rules/python/style.md` |
| Set up typing | `rules/python/typing.md` |
| Organize project files | `rules/python/architecture.md` |
| Write git commits | `rules/workflow/git.md` |
| Create tests | `rules/workflow/testing.md` |
| Review code | `rules/workflow/code-review.md` |
| Configure my project | `pyproject.toml.example` |
| Quick cheat sheet | `QUICK_REFERENCE.md` |

## 📊 Standards at a Glance

| Area | Standard |
|------|----------|
| **Python** | 3.10+ |
| **Formatter** | Ruff (88 chars) |
| **Type Checker** | mypy/pyright (100%) |
| **Testing** | pytest (80%+ coverage) |
| **Git Flow** | Conventional Commits |
| **Git Strategy** | Git Flow (main/develop) |
| **Docstrings** | Google style |
| **Architecture** | src/ layout, DI, SRP |
| **Exceptions** | Domain-specific |
| **Logging** | Standard library |

## 🤔 FAQ

**Q: Do I need to use Ruff?**
A: Yes, it replaces Black + isort + flake8. For consistency and automation.

**Q: Can I change the line length?**
A: Yes, modify the rule files to suit your team. But 88 chars is the default.

**Q: What if I disagree with a rule?**
A: These are based on industry standards. Discuss with your team and document any exceptions.

**Q: How do I enforce these rules?**
A: 
1. CI/CD: Run ruff, mypy, pytest
2. Code review: Use checklist from `code-review.md`
3. Pre-commit hooks: Format before commit
4. CODEOWNERS: Route reviews to domain experts

**Q: Are these rules mandatory?**
A: They're guidelines based on best practices. Adapt them to your team's needs.

## 📝 How to Use These Rules

### Option 1: Standalone Reference
Just read the guides whenever you need guidance on a specific topic.

### Option 2: Agent Configuration
Copy agent files to your project so code agents follow the rules automatically.

### Option 3: CI/CD Enforcement
Set up checks to run:
```bash
ruff check .
mypy src/
pytest --cov=src
```

### Option 4: Team Documentation
Copy `rules/` directory to your project docs as internal standards reference.

## 🔄 Updates & Maintenance

These rules are living documents. To update:
1. Edit the relevant `.md` file
2. Update agent configs to match
3. Commit with `docs:` prefix
4. Announce changes to team

## 📜 Standards Sources

All rules are based on established industry standards:
- [PEP 8](https://peps.python.org/pep-0008/) — Python style guide
- [Google Python Style Guide](https://google.github.io/styleguide/pyguide.html)
- [Conventional Commits](https://www.conventionalcommits.org/) — Git commits
- [Python Packaging Authority](https://packaging.python.org/) — Project layout
- [pytest Best Practices](https://docs.pytest.org/)
- [PEP 526](https://peps.python.org/pep-0526/) — Variable annotations
- [PEP 589](https://peps.python.org/pep-0589/) — TypedDict

---

**Questions?** Refer to the specific guide or ask your team lead.

**Want to contribute improvements?** Update the relevant file and propose the change.

**Last Updated**: 2026-03-31
