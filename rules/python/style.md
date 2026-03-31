# Style & Formatage Python

## Formatage

- Utiliser **Trunk** comme méta-linter/formateur (wraps Ruff, qui remplace Black, isort, flake8).
- Longueur de ligne maximale : **88 caractères**.
- Indentation : **4 espaces**, jamais de tabulations.
- Encodage : **UTF-8** partout.

## Nommage

| Élément          | Convention         | Exemple                  |
|------------------|--------------------|--------------------------|
| Module           | `snake_case`       | `data_loader.py`         |
| Classe           | `PascalCase`       | `DataLoader`             |
| Fonction/Méthode | `snake_case`       | `load_from_csv()`        |
| Constante        | `UPPER_SNAKE_CASE` | `MAX_RETRY_COUNT`        |
| Variable privée  | `_snake_case`      | `_internal_cache`        |
| Type variable    | `PascalCase` + `T` | `ItemT = TypeVar("ItemT")` |

## Imports

Ordre strict (géré automatiquement par Trunk/Ruff via `isort`) :

1. Bibliothèque standard
2. Packages tiers
3. Modules internes au projet

```python
# Correct
import os
from pathlib import Path

import httpx
from pydantic import BaseModel

from myproject.core import config
```

- Préférer les imports explicites (`from x import y`) aux imports génériques (`import x`).
- Ne jamais utiliser `from x import *`.

## Docstrings

- Format **Google style** pour toutes les fonctions et classes publiques.
- Toujours documenter les paramètres, retours et exceptions.

```python
def fetch_user(user_id: int, *, include_inactive: bool = False) -> User:
    """Récupère un utilisateur par son identifiant.

    Args:
        user_id: Identifiant unique de l'utilisateur.
        include_inactive: Si True, inclut les utilisateurs désactivés.

    Returns:
        L'objet User correspondant.

    Raises:
        UserNotFoundError: Si l'utilisateur n'existe pas.
    """
```

## Commentaires

- Les commentaires expliquent le **pourquoi**, pas le **quoi**.
- Pas de commentaires évidents ou redondants avec le code.
- Préférer un bon nommage à un commentaire explicatif.
