# Typage Python

## Principes

- **Tout le code doit être typé.** Viser un score mypy/pyright de 100%.
- Utiliser les types natifs modernes (Python 3.10+) : `list[int]`, `dict[str, Any]`, `str | None`.
- Ne pas utiliser les anciens types de `typing` quand un équivalent natif existe (`List`, `Dict`, `Optional`, `Union`).

## Annotations de fonctions

Toujours annoter les paramètres et le type de retour :

```python
# Correct
def process_items(items: list[str], *, limit: int = 100) -> dict[str, int]:
    ...

# Incorrect — annotations manquantes
def process_items(items, limit=100):
    ...
```

## Types courants

```python
from collections.abc import Callable, Iterable, Sequence
from typing import Any, TypeAlias, TypeVar

# Alias de type pour la lisibilité
UserId: TypeAlias = int
Headers: TypeAlias = dict[str, str]

# Generics
T = TypeVar("T")

def first_or_none(items: Sequence[T]) -> T | None:
    return items[0] if items else None
```

## Pydantic pour la validation

- Utiliser **Pydantic v2** pour les modèles de données qui traversent une frontière (API, config, fichiers).
- Annoter avec `Field()` pour la documentation et la validation.

```python
from pydantic import BaseModel, Field

class UserCreate(BaseModel):
    name: str = Field(min_length=1, max_length=100)
    email: str = Field(pattern=r"^[\w.-]+@[\w.-]+\.\w+$")
    role: str = Field(default="viewer")
```

## Règles strictes

- `Any` est acceptable uniquement pour les interfaces de sérialisation/désérialisation générique. Justifier son usage par un commentaire.
- Ne jamais utiliser `# type: ignore` sans code d'erreur : écrire `# type: ignore[assignment]`.
- Préférer `Protocol` à l'héritage abstrait pour le duck typing structuré.
