# GitHub Copilot Instructions

Coding standards for Python projects — apply these principles to all suggestions.

## Project Standards

**Python 3.10+** with pipenv, Ruff, mypy, pytest, Docusaurus docs, GitHub Actions.

## Key Principles

### Code Quality
- Type all code (mypy strict, 100% target)
- Use Ruff for formatting (88 char lines)
- Follow PEP 8 + Google Style Guide
- Google-style docstrings on public functions
- No `print()` in production (use logging)

### Architecture
- `src/` directory layout (Python Packaging Authority)
- Single Responsibility Principle per class
- Dependency injection (no global state)
- Domain-specific exception classes
- pipenv exclusively for dependencies

### Testing
- pytest framework with fixtures
- 80% coverage minimum, 100% for critical code
- Mock external dependencies only
- FIRST principles: Fast, Independent, Repeatable, Self-checking, Timely

### Git Commits
- **Conventional Commits**: `<type>(<scope>): <subject>`
- **Mandatory scopes** (see PROJECT_SCOPES.md):
  - Core: `cli`, `playbook`, `schema`, `registry`
  - Analyzers: `analyzer`, `analyzer/trivy`, `analyzer/sbom`, etc.
  - Rendering: `report`, `templates`
  - Tooling: `ci`, `deps`, `docs`
- Atomic commits, present tense, functional descriptions

### Security
- Validate all user input at API boundaries
- Use parameterized queries (never string concatenation)
- Never hardcode secrets (use environment variables)
- No SQL injection, XSS, or CSRF vulnerabilities
- Use bcrypt/argon2 for password hashing

### Documentation
- Use Docusaurus for docs (markdown in `docs/`)
- Update memory bank after significant changes
- Use Mermaid for diagrams (C4 model preferred)
- Google Style Guide for writing
- Keep root README brief, link to detailed docs

## Pattern Examples

### Proper Function
```python
def fetch_user_by_email(email: str, db: Session) -> User | None:
    """Fetch user by email address.

    Args:
        email: User's email address
        db: Database session (injected)

    Returns:
        User if found, None otherwise
    """
    return db.query(User).filter(User.email == email).first()
```

### Custom Exception
```python
class DomainError(Exception):
    """Base exception for domain errors."""

class UserNotFoundError(DomainError):
    """User does not exist."""
```

### Test Structure
```python
def test_fetch_user_by_email_found(db_session):
    """Test fetching an existing user."""
    user = User(email="test@example.com")
    db_session.add(user)
    db_session.commit()

    result = fetch_user_by_email("test@example.com", db_session)
    assert result.email == "test@example.com"
```

### Commit Message
```
feat(analyzer/trivy): add CVSS severity filtering

Allows users to filter vulnerabilities by CVSS score.
Implemented via existing severity filter interface.

Closes #123
```

## When in Doubt

- **Formatting?** → Run `ruff format .`
- **Type safety?** → Use `mypy src/`
- **Tests?** → Run `pytest --cov=src`
- **Commits?** → Check PROJECT_SCOPES.md
- **Full guide?** → See CLAUDE.md
