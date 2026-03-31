# Agent Rules Repository

A comprehensive, standards-based ruleset for Python projects designed for code agents (Claude Code, Cursor, GitHub Copilot, Windsurf, etc.). All rules are based on established industry guidelines (PEP 8, Google Style Guide, Conventional Commits).

## 📋 Quick Start

### For Different Agents

The same rules are available in multiple formats:

- **Claude Code**: Use `CLAUDE.md`
- **Cursor**: Use `.cursorrules`
- **GitHub Copilot**: Use `.github/copilot-instructions.md`
- **Windsurf**: Use `.windsurfrules`
- **Generic projects**: Reference the detailed guides in `rules/`

### Copy to Your Project

```bash
# Clone or download this repository
git clone https://github.com/trivoallan/rules.git agent-rules

# Copy relevant files to your project
cp agent-rules/CLAUDE.md your-project/
cp agent-rules/.cursorrules your-project/
cp agent-rules/.windsurfrules your-project/
cp -r agent-rules/.github/copilot-instructions.md your-project/.github/

# Or copy detailed rules if you want reference docs
cp -r agent-rules/rules/ your-project/docs/coding-standards/
```

## 📚 Contents

### Agent-Specific Files
- `CLAUDE.md` — Configuration for Claude Code / Cowork mode
- `.cursorrules` — Configuration for Cursor editor
- `.windsurfrules` — Configuration for Windsurf editor
- `.github/copilot-instructions.md` — Configuration for GitHub Copilot

### Detailed Rule Documents

#### Python Conventions (`rules/python/`)
- **`style.md`** — Code formatting, naming conventions, docstrings
  - Ruff configuration, PEP 8 style
  - Naming: PascalCase, snake_case, UPPER_SNAKE_CASE
  - Google-style docstrings

- **`typing.md`** — Type annotations and type safety
  - Full type coverage (100% mypy/pyright)
  - Python 3.10+ syntax (list[T], str | None)
  - Pydantic v2 for validation
  - TypeVar and Protocol usage

- **`architecture.md`** — Project structure and design patterns
  - Recommended directory layout (src/ structure)
  - Single Responsibility Principle
  - Dependency injection patterns
  - Custom exception hierarchies
  - Logging best practices

#### Workflow Standards (`rules/workflow/`)
- **`git.md`** — Version control and commit standards
  - Git Flow branching strategy
  - Conventional Commits format
  - Atomic, clear commit messages
  - Pull request guidelines

- **`testing.md`** — Testing practices and coverage
  - pytest framework and structure
  - Unit, integration, E2E test organization
  - 80%+ coverage minimum (100% for critical code)
  - FIRST principles (Fast, Independent, Repeatable, Self-checking, Timely)
  - Fixtures and mocking best practices

- **`code-review.md`** — Code review standards and process
  - What to check: correctness, security, testability, performance
  - Security checklist (SQL injection, secrets, XSS)
  - Code ownership and CODEOWNERS file
  - Constructive review tone

## 🎯 Key Principles

### Style & Formatting
- **Ruff** for automatic formatting and linting
- 88-character line length (Black/Ruff standard)
- 4-space indentation
- Google-style docstrings

### Type Safety
- Full type annotations on all functions and variables
- Modern Python 3.10+ syntax
- 100% mypy/pyright coverage target
- Pydantic v2 for data validation

### Architecture
- `src/` directory layout (Python Packaging Authority standard)
- Single responsibility per class
- Dependency injection (no global state)
- Domain-specific exceptions
- Always use `logging`, never `print()` in production

### Git & Commits
- **Conventional Commits** format
- Atomic commits (one logical change = one commit)
- Squash/rebase for clean history
- Commit types: feat, fix, docs, style, refactor, perf, test, chore

### Testing
- **pytest** with proper organization
- 80% minimum coverage (100% for critical paths)
- FIRST principles: Fast, Independent, Repeatable, Self-checking, Timely
- Mock external dependencies only

### Code Review
- Focus on correctness, security, testability, performance
- Check for N+1 queries, SQL injection, hardcoded secrets
- Constructive tone and learning opportunities
- Require status checks and approvals before merge

## 🚀 Using These Rules

### Option 1: Copy Agent Config to Your Project
Copy the agent-specific file(s) (`CLAUDE.md`, `.cursorrules`, etc.) to your project root. Agents will automatically detect and follow these rules.

### Option 2: Reference in Your Project README
Include a link to this repository in your project README with a note that you follow these standards:

```markdown
## Coding Standards

We follow the [Agent Rules](https://github.com/trivoallan/rules) for all Python code.
Our conventions cover style, typing, architecture, testing, and git workflow.
```

### Option 3: Copy Detailed Rules as Internal Documentation
Copy the `rules/` directory to your project as internal reference documentation:

```bash
cp -r rules/ docs/coding-standards/
```

## 📖 Standards Reference

### Quick Command Reference

```bash
# Format and lint code
ruff format .
ruff check . --fix

# Type check
mypy src/

# Run tests with coverage
pytest --cov=src --cov-report=html

# Create a proper commit
git commit -m "feat(auth): add JWT refresh mechanism"
```

### Python Version

- **Minimum**: Python 3.10
- **Recommended**: Python 3.11+
- Uses modern type syntax: `list[T]`, `str | None`, `type[X]`

### Key Tools

| Tool | Purpose |
|------|---------|
| **Ruff** | Formatter + Linter (replaces Black, isort, flake8) |
| **mypy/pyright** | Static type checker (target 100% coverage) |
| **pytest** | Testing framework |
| **Pydantic v2** | Data validation at system boundaries |
| **Git** | Version control with Conventional Commits |

## 🔄 Updating These Rules

### Adding New Standards
1. Edit the relevant `.md` file in `rules/`
2. Update the agent-specific files (`CLAUDE.md`, `.cursorrules`, etc.)
3. Commit with `docs: <description>`

### Removing or Changing Rules
1. Update the rule file(s)
2. Make sure all agent configs are in sync
3. Consider backwards compatibility for existing projects

## ❓ FAQ

### Q: Can I customize these rules for my team?
**A**: Yes! Copy the files and modify them. Consider:
- Changing line length (default 88)
- Adjusting naming conventions
- Adding project-specific exceptions
- Customizing git flow (main/develop names)

### Q: Do I need all agent configs?
**A**: No. Only copy the files for agents you actually use.

### Q: How do I enforce these rules?
**A**:
1. Use CI/CD to run `ruff`, `mypy`, `pytest`
2. Require passing checks before merge
3. Code review using `code-review.md` checklist
4. Use `CODEOWNERS` for domain-specific reviews

### Q: Are these rules Python-only?
**A**: Currently yes, optimized for Python 3.10+. Extensions for other languages welcome!

## 📄 License

These rules are provided as-is for reference and adaptation. Customize freely for your team and projects.

## 🤝 Contributing

Found issues or have suggestions? Open an issue or submit improvements.

---

**Last Updated**: 2026-03-31
**Python Target**: 3.10+
**Based on**: PEP 8, Google Style Guide, Conventional Commits, Python Packaging Authority
