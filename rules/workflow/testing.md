# Testing & Qualité du Code

## Principes (Testivité)

Suivre la définition de [Test Driven Development (TDD)](https://en.wikipedia.org/wiki/Test-driven_development) et [FIRST principles](https://en.wikipedia.org/wiki/FIRST_(disambiguation)) :

- **F**ast : tests rapides (< 1s idéalement)
- **I**ndependent : tests isolation, pas de dépendances entre tests
- **R**epeatable : résultats déterministes, pas de flakiness
- **S**elf-checking : pass/fail clair, pas de vérification manuelle
- **T**imely : tests écrits en même temps que le code

## Structure des tests

Utiliser **pytest** (standard Python) avec la structure :

```
tests/
├── conftest.py                 # Fixtures partagées
├── unit/
│   ├── test_models.py
│   └── test_validators.py
├── integration/
│   ├── test_api.py
│   └── test_database.py
└── e2e/
    └── test_user_workflow.py
```

## Couverture de code

- **Minimum 80%** de couverture (mesurée par `coverage.py`).
- **100% pour les fonctions critiques** : auth, paiements, exports.
- Mesurer avec : `pytest --cov=src --cov-report=html`

**Ne pas viser 100% aveuglément** — certaines branches (erreurs rares, fallbacks) peuvent ne pas être testées.

## Types de tests

### Tests unitaires
- Testent une fonction/classe isolée.
- Aucune I/O réelle (baser les dépendances).
- Rapides (< 100ms par test).

```python
# tests/unit/test_validators.py
from src.core.validators import EmailValidator

def test_valid_email():
    assert EmailValidator.is_valid("user@example.com")

def test_invalid_email_no_domain():
    assert not EmailValidator.is_valid("user@")
```

### Tests d'intégration
- Testent plusieurs composants ensemble (API + DB, service + cache).
- Utilisent une base de données de test ou un conteneur Docker.
- Plus lents mais essentiels pour vérifier l'interaction réelle.

```python
# tests/integration/test_api.py
from fastapi.testclient import TestClient
from src.main import app

@pytest.fixture
def client():
    return TestClient(app)

def test_create_user_endpoint(client, db_session):
    response = client.post("/users", json={"name": "Alice", "email": "alice@example.com"})
    assert response.status_code == 201
    assert db_session.query(User).count() == 1
```

### Tests end-to-end (E2E)
- Testent le flux complet d'un utilisateur (UI → API → DB).
- Generalement dans un environnement staging.
- Peuvent être lents (> 10s), donc peu nombreux.

## Fixtures pytest

Utiliser `conftest.py` pour les fixtures communes :

```python
# tests/conftest.py
import pytest
from sqlalchemy import create_engine
from sqlalchemy.orm import sessionmaker

@pytest.fixture
def db_session():
    engine = create_engine("sqlite:///:memory:")
    Session = sessionmaker(bind=engine)
    session = Session()
    yield session
    session.close()

@pytest.fixture
def sample_user(db_session):
    user = User(name="Test User", email="test@example.com")
    db_session.add(user)
    db_session.commit()
    return user
```

## Mocking et stubbing

- Mocker uniquement les dépendances externes (API tiers, DB).
- Ne pas mocker le code qu'on teste (cela rend le test inutile).

```python
# ✅ Correct
@patch("requests.get")
def test_fetch_external_api(mock_get):
    mock_get.return_value.json.return_value = {"id": 123}
    result = fetch_user_from_external_api(123)
    assert result["id"] == 123

# ❌ Incorrect — moque l'une des fonctions à tester
@patch("src.validators.EmailValidator.is_valid")
def test_is_valid(mock_validator):
    mock_validator.return_value = True
    # Ce test ne teste rien du tout !
```

## Nommage des tests

Format : `test_<function>_<scenario>`

```python
def test_fetch_user_by_id_found():
    ...

def test_fetch_user_by_id_not_found():
    ...

def test_create_user_with_valid_email():
    ...

def test_create_user_with_invalid_email_raises_error():
    ...
```

## Assertions claires

Utiliser des assertions explicites (pytest) :

```python
# ✅ Clair
assert user.email == "test@example.com"
assert response.status_code == 201

# ❌ Vague
assert user
assert response
```
