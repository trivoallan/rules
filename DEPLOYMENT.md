# Deployment Guide

How to use this agent rules repository in your projects.

## 🎯 Quick Setup (5 minutes)

### Step 1: Choose Your Agent(s)
Pick the configuration file(s) you need:
- Claude Code / Cowork: `CLAUDE.md`
- Cursor: `.cursorrules`
- GitHub Copilot: `.github/copilot-instructions.md`
- Windsurf: `.windsurfrules`

### Step 2: Copy to Your Project
```bash
# For Claude Code / Cowork
cp agent-rules/CLAUDE.md my-project/

# For Cursor
cp agent-rules/.cursorrules my-project/

# For Windsurf
cp agent-rules/.windsurfrules my-project/

# For GitHub Copilot
mkdir -p my-project/.github
cp agent-rules/.github/copilot-instructions.md my-project/.github/
```

### Step 3: Configure pyproject.toml
```bash
cp agent-rules/pyproject.toml.example my-project/pyproject.toml
# Edit as needed for your project
```

### Step 4: Commit
```bash
cd my-project
git add CLAUDE.md .cursorrules .windsurfrules .github/copilot-instructions.md pyproject.toml
git commit -m "chore: add agent rules configuration"
```

**Done!** Your agents now follow the standards.

## 🔄 Keeping Rules in Sync

### Option A: Copy Once (Simplest)
- Copy agent files to each project
- Each project maintains its own copy
- Good for: Independent projects, teams

### Option B: Submodule (For Monorepos)
```bash
# Add this repo as a git submodule
git submodule add https://github.com/your-org/agent-rules.git rules-repo

# Link to the submodule
ln -s rules-repo/CLAUDE.md CLAUDE.md
ln -s rules-repo/.cursorrules .cursorrules
```
- Updates to rules propagate to all projects
- Good for: Monorepos, shared standards across team

### Option C: CI/CD Check (For Governance)
```bash
# In your CI/CD pipeline
- name: Validate Agent Rules
  run: |
    if ! cmp -s CLAUDE.md canonical-rules/CLAUDE.md; then
      echo "Agent rules are out of sync with canonical version"
      exit 1
    fi
```
- Enforces rules compliance
- Good for: Regulated environments, strict governance

## 📋 Customization Guide

### Adding Project-Specific Rules

1. Copy `CLAUDE.md` to your project
2. Add your specific rules under a new section:

```markdown
# Project-Specific Standards

## Database
- Use SQLAlchemy ORM exclusively
- Always use migrations with Alembic
- No direct SQL queries in business logic

## API
- All endpoints require authentication
- Use OpenAPI/FastAPI for documentation
- Rate limit: 1000 requests per hour

---

# Base Standards (from agent-rules repository)
[rest of content...]
```

### Relaxing Requirements

If you need to relax a requirement, document it:

```markdown
# Exceptions

## Coverage
- We use 70% minimum (instead of 80%) because:
  - Legacy code is hard to test
  - Team is ramping up testing practices
  - Review coverage threshold quarterly

---

[rest of content...]
```

## ✅ Pre-Launch Checklist

Before adding agent rules to your project:

- [ ] All team members have read `README.md`
- [ ] Project is updated to Python 3.10+ (if applicable)
- [ ] `ruff`, `mypy`, `pytest` installed and configured
- [ ] CI/CD runs `ruff check`, `mypy`, `pytest`
- [ ] Existing code passes linting (or exemption added to pyproject.toml)
- [ ] Code review checklist from `rules/workflow/code-review.md` is understood
- [ ] Git branch protection requires PR reviews
- [ ] CODEOWNERS file created (optional but recommended)

## 🚀 Enforcement Setup

### GitHub Actions CI Example

```yaml
# .github/workflows/ci.yml
name: CI

on: [push, pull_request]

jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-python@v4
        with:
          python-version: "3.10"
      - run: pip install ruff mypy pytest pytest-cov
      - run: ruff check .
      - run: ruff format --check .
      - run: mypy src/
      - run: pytest --cov=src --cov-fail-under=80

  review:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - run: |
          # Ensure PR has description
          if [ -z "${{ github.event.pull_request.body }}" ]; then
            echo "PR must include a description"
            exit 1
          fi
```

### Pre-commit Hook Example

```bash
# .git/hooks/pre-commit
#!/bin/bash

# Format code
ruff format .

# Check for issues
ruff check . --fix || exit 1
mypy src/ || exit 1

# Add fixed files
git add -A

exit 0
```

## 📊 Team Adoption Timeline

### Week 1: Introduction
- Team reads README and QUICK_REFERENCE
- Copy rules to project
- Set up CI/CD checks
- Discuss any project-specific customizations

### Week 2-3: Gradual Enforcement
- Run checks but don't block merges
- Use code review checklist for feedback
- Team gets comfortable with tools
- Fix low-hanging fruit (formatting, obvious issues)

### Week 4+: Full Enforcement
- CI/CD checks now block failing merges
- All new PRs follow standards
- Legacy code gradually cleaned up (if needed)
- Standards become second nature

## 🛠 Troubleshooting

### "My old code doesn't pass linting"

**Option 1: Exempt specific files**
```toml
# pyproject.toml
[tool.ruff.lint]
exclude = ["src/legacy_module/"]
```

**Option 2: Cleanup gradually**
```bash
# Find all issues
ruff check src/

# Fix what you can automatically
ruff check src/ --fix

# Document the rest
# git commit -m "chore: fix ruff issues in core module"
```

### "A rule doesn't apply to my project"

1. Document the exception in your `CLAUDE.md`
2. Explain why it doesn't apply
3. Agree as a team
4. Implement the exception in your config

### "Our team disagrees with a rule"

1. Discuss and propose an alternative
2. Document the new rule and rationale
3. Update all agent config files consistently
4. Commit with clear justification

## 📞 Support

- **Questions?** Check `QUICK_REFERENCE.md` or the detailed guides in `rules/`
- **Need help?** Reach out to your tech lead or team
- **Found an issue?** Document the exception and update your config

---

**Next Steps:**
1. Copy relevant files to your project
2. Run lint/type/test checks to identify issues
3. Fix issues or document exemptions
4. Set up CI/CD enforcement
5. Make new code follow the standards
6. Gradually improve legacy code
