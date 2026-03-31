# Quick Reference Guide

## 🚀 At a Glance

| Aspect | Standard |
|--------|----------|
| **Language** | Python 3.10+ |
| **Formatter** | Ruff (88 char lines) |
| **Linter** | Ruff |
| **Type Checker** | mypy/pyright (100% coverage) |
| **Testing** | pytest (80%+ coverage) |
| **Git** | Conventional Commits + Git Flow |
| **Package Layout** | src/ directory structure |

## 📝 Naming Conventions

```python
MyClass           # PascalCase for classes
my_function()     # snake_case for functions
MY_CONSTANT       # UPPER_SNAKE_CASE for constants
_private_var      # _snake_case for private
```

## 🔤 Type Annotations (Python 3.10+)

```python
# ✅ Modern syntax
def fetch_users(limit: int) -> list[User]:
    ...

name: str | None = None
config: dict[str, int] = {}
callback: Callable[[int], str] = lambda x: str(x)

# ❌ Old syntax (don't use)
def fetch_users(limit: int) -> List[User]:  # No
name: Optional[str] = None  # No
config: Dict[str, int] = {}  # No
```

## 📦 Project Structure

```
src/
  my_project/
    __init__.py
    core/
      __init__.py
      models.py
      config.py
    api/
      __init__.py
      routes.py
    utils/
      __init__.py

tests/
  conftest.py
  unit/
    test_models.py
  integration/
    test_api.py

pyproject.toml
README.md
CLAUDE.md
.cursorrules
.windsurfrules
```

## 🎯 Git Commits

```bash
# Format: <type>(<scope>): <subject>
feat(auth):       add JWT refresh
fix(api):         handle null values
docs(readme):     update installation
refactor(db):     optimize queries
test(models):     add edge cases
chore(deps):      upgrade pydantic
```

**Types**: feat, fix, docs, style, refactor, perf, test, chore

## ✅ Docstring Template

```python
def process_user(user_id: int, *, notify: bool = False) -> User:
    """Brief description of what the function does.

    Longer explanation if needed, provide context and use cases.

    Args:
        user_id: Description of user_id parameter.
        notify: Whether to send notification email.

    Returns:
        Description of return value.

    Raises:
        UserNotFoundError: If user doesn't exist.
        InvalidUserIdError: If user_id is negative.
    """
```

## 🧪 Test Naming

```python
def test_fetch_user_by_id_found():
    """Test case when user exists."""

def test_fetch_user_by_id_not_found():
    """Test case when user doesn't exist."""

def test_create_user_with_valid_email():
    """Test successful user creation."""

def test_create_user_with_invalid_email_raises_error():
    """Test that invalid email raises exception."""
```

Pattern: `test_<function>_<scenario>`

## 🛡️ Security Checklist

- [ ] No hardcoded secrets (use env vars)
- [ ] Parameterized queries (no string concatenation)
- [ ] Input validation at boundaries
- [ ] Authentication/authorization on all endpoints
- [ ] No sensitive data in logs
- [ ] Password hashing (bcrypt, argon2)
- [ ] CORS properly configured

## 📊 Code Review Checklist

- [ ] Code is correct (solves the problem)
- [ ] No N+1 database queries
- [ ] No SQL injection vulnerabilities
- [ ] All user input is validated
- [ ] Tests cover happy path and edge cases
- [ ] Type annotations are complete
- [ ] No hardcoded credentials
- [ ] Error handling is appropriate
- [ ] Performance is acceptable
- [ ] Documentation is updated

## 🚢 Pre-Merge Checklist

```bash
# Run locally before committing
trunk fmt
trunk check --fix
pytest --cov=src --cov-report=term-missing

# Push and verify CI passes
git push origin feature/my-feature
# → Check GitHub/GitLab CI status
# → Request review
# → Address feedback
# → Merge when approved
```

## 🔗 Full Documentation

- **Style**: See `rules/python/style.md`
- **Typing**: See `rules/python/typing.md`
- **Architecture**: See `rules/python/architecture.md`
- **Git Workflow**: See `rules/workflow/git.md`
- **Testing**: See `rules/workflow/testing.md`
- **Code Review**: See `rules/workflow/code-review.md`

## 💡 Common Patterns

### Dependency Injection
```python
class UserService:
    def __init__(self, db: Database, cache: Cache):
        self.db = db
        self.cache = cache

# Usage
service = UserService(db=get_db(), cache=get_cache())
```

### Custom Exceptions
```python
class DomainError(Exception):
    """Base for domain errors."""

class UserNotFoundError(DomainError):
    """User doesn't exist."""
```

### Logging
```python
import logging

logger = logging.getLogger(__name__)

logger.info(f"Processing user {user_id}")
logger.warning("Slow query detected")
logger.error("Failed to fetch user", exc_info=True)
```

## ⚡ Quick Commands

```bash
# Format code
trunk fmt

# Check and fix issues
trunk check --fix

# Run tests with coverage
pytest --cov=src

# Create new feature branch
git checkout -b feature/my-feature develop

# Commit with conventional format
git commit -m "feat(api): add user search endpoint"

# Push for PR
git push origin feature/my-feature
```

---

**Need more details?** See the full guides in `rules/` directory.
