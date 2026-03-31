# Architecture & Structure Python

## Organisation des fichiers

Suivre la structure recommandée par la **Python Packaging Authority (PyPA)** :

```
my_project/
├── pyproject.toml                 # Configuration unifiée du projet
├── README.md
├── LICENSE
├── .gitignore
├── ruff.toml                      # Configuration Ruff
├── pyproject.toml                 # ou setup.cfg pour les anciens projets
├── src/
│   └── my_project/
│       ├── __init__.py
│       ├── core/
│       │   ├── __init__.py
│       │   ├── models.py          # Modèles de données
│       │   └── config.py          # Configuration
│       ├── api/
│       │   ├── __init__.py
│       │   ├── routes.py          # Points d'entrée API
│       │   └── dependencies.py    # Injection de dépendances
│       └── utils/
│           ├── __init__.py
│           └── helpers.py
├── tests/
│   ├── conftest.py                # Fixtures pytest
│   ├── test_core.py
│   └── test_api.py
└── docs/
    └── index.md
```

**Rationale** : Placer le code source dans `src/` évite les conflits de dépendances lors du développement (voir [PEP 517](https://peps.python.org/pep-0517/)).

## Classes et responsabilité unique

- Une classe = une responsabilité (Single Responsibility Principle).
- Éviter les classe "fourre-tout" avec 20+ méthodes.
- Préférer la composition à l'héritage (principe de Liskov).

```python
# Correct
class UserRepository:
    """Gère la persistence des utilisateurs."""
    def get_by_id(self, user_id: int) -> User: ...

class UserValidator:
    """Valide les données utilisateur."""
    def validate_email(self, email: str) -> bool: ...

# Incorrect — responsabilités mélangées
class User:
    def save(self): ...
    def validate(self): ...
    def send_email(self): ...
```

## Dépendances et injection

- Utiliser l'injection de dépendances plutôt que des variables globales.
- Pour FastAPI : utiliser `Depends()`.
- Pour les projets génériques : utiliser une classe `Container` simple.

```python
from typing import Callable

class Container:
    def __init__(self, db: Database):
        self.db = db

    def get_user_repo(self) -> UserRepository:
        return UserRepository(self.db)

# Usage
container = Container(db=get_database())
repo = container.get_user_repo()
```

## Gestion des erreurs

- Créer des exceptions spécifiques au domaine métier (ne pas lever `Exception`).
- Hériter de `ValueError`, `TypeError` ou créer une hiérarchie cohérente.

```python
class DomainError(Exception):
    """Base pour les erreurs métier."""

class UserNotFoundError(DomainError):
    """L'utilisateur demandé n'existe pas."""

class InvalidEmailError(DomainError):
    """L'email fourni est invalide."""
```

## Logging

- Utiliser le module standard `logging`, jamais `print()` en production.
- Configurer un logger par module : `logger = logging.getLogger(__name__)`.
- Utiliser les bons niveaux : DEBUG, INFO, WARNING, ERROR, CRITICAL.

```python
import logging

logger = logging.getLogger(__name__)

def process_user(user_id: int):
    logger.info(f"Processing user {user_id}")
    try:
        user = fetch_user(user_id)
    except UserNotFoundError:
        logger.warning(f"User {user_id} not found")
```

## Package Management

**Use pipenv exclusively** for all Python project dependency management.

```bash
# Installation de dépendances
pipenv install package_name

# Installation de dépendances de développement
pipenv install --dev pytest ruff mypy

# Exécution en environnement pipenv
pipenv run python -m my_module

# Générer le lock file
pipenv lock
```

**Important:**
- Commiter `Pipfile` ET `Pipfile.lock` au contrôle de version
- Ne jamais utiliser `pip` directement
- `Pipfile` : dépendances directes avec versions flexibles
- `Pipfile.lock` : garantit les reproductibilité des builds
