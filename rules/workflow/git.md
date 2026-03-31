# Git Flow & Bonnes pratiques

## Stratégie de branchage

Suivre **Git Flow** (adapté à votre CI/CD) :

| Branche       | Usage                                  | Protection        |
|---------------|----------------------------------------|-------------------|
| `main`        | Code en production, tag releases       | ✅ Requiert PR     |
| `develop`     | Base pour les features, intégration    | ✅ Requiert PR     |
| `feature/*`   | Nouvelles fonctionnalités              | ❌ Pas de protection |
| `bugfix/*`    | Corrections de bugs                    | ❌ Pas de protection |
| `hotfix/*`    | Corrections urgentes en production     | ❌ Pas de protection |
| `release/*`   | Préparation d'une release              | ✅ Requiert PR     |

## Commits

Suivre la convention **Conventional Commits** ([conventionalcommits.org](https://www.conventionalcommits.org/)) :

```
<type>(<scope>): <subject>

<body>

<footer>
```

**Types autorisés** (cf. [Angular Convention](https://github.com/angular/angular/blob/main/CONTRIBUTING.md#type)) :

- `feat` — Nouvelle fonctionnalité
- `fix` — Correction de bug
- `docs` — Documentation uniquement  P
- `style` — Formatage, imports (pas de changement logique)
- `refactor` — Refactoring sans changement logique
- `perf` — Améliorations de performance
- `test` — Ajout ou correction de tests
- `chore` — Mise à jour de dépendances, config, CI

**Scopes sont OBLIGATOIRES** — doivent représenter le composant architectural modifié.

Voir `PROJECT_SCOPES` pour la liste des scopes définis pour votre projet.

**Exemples** :

```
feat(auth): add JWT token refresh mechanism

Implement automatic token refresh when within 5min of expiry.
Closes #123
```

```
fix(api): handle null values in user response

Previously, missing optional fields would cause JSON serialization
to fail. Now they are omitted from the response.
```

- ✅ Commits atomiques : un changement logique = un commit.
- ✅ Messages clairs au présent : "add feature" pas "added feature".
- ❌ Messages vagues : "fix stuff", "update".
- ❌ Commits géants couvrant 5 fonctionnalités.

## Pull Requests

- **1 PR = 1 changement logique** (feature, bugfix, refactor).
- **Limiter à 400 lignes de code** pour faciliter la revue.
- Si plus gros : découper en plusieurs PRs ou en stacks ([Graphite](https://graphite.dev/), [Mutation](https://mutation.app/)).

### Titre et description

```markdown
# [Feature] User role-based access control

## Description
Implement role-based access control (RBAC) to restrict API endpoints
based on user roles.

## Changes
- Add `Role` enum (admin, editor, viewer)
- Add role check middleware
- Update /users and /posts endpoints with role guards

## Testing
- Unit tests for role validation (100% coverage)
- Integration tests with different user roles
- Manual testing on staging

## Related
Closes #456
Depends on #450
```

### Checklist avant merge

- [ ] Tests locaux : `pytest -v --cov`
- [ ] Linting : `ruff check .`
- [ ] Typing : `mypy src/`
- [ ] Pas de code non-utilisé (dead code)
- [ ] Pas de secrets en dur (utiliser `.env` ou vaults)
- [ ] Documentation mise à jour
- [ ] Changelog alimenté (si applicable)

## Rebase vs Merge

- Préférer **rebase** pour garder un historique linéaire et propre.
- Utiliser **squash merge** si PR = 1 commit logique.
- Éviter les **merge commits** conventionnels sur `develop`/`main`.

```bash
# Correct : historique propre
git rebase develop
git push -f origin feature/my-feature

# Lors du merge sur GitHub
# → Utiliser "Squash and merge"
```
