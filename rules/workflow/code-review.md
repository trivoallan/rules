# Code Review & Standards de qualité

## Objectifs d'une revue de code

1. **Correctness** : le code résout le problème sans bugs.
2. **Clarity** : le code est lisible et maintenable.
3. **Consistency** : respecte les conventions du projet.
4. **Learning** : opportunité de partage de connaissances.

Ne pas être tatillon sur les points stylistes — le linting s'en charge.

## Points clés à vérifier

### Logique et correction
- [ ] Le code fait ce qu'il prétend faire.
- [ ] Gestion des cas limites (null, vide, erreurs).
- [ ] Pas de race conditions ou mutations inadvertises.
- [ ] Pas de N+1 queries ou boucles inefficaces.

```python
# ❌ N+1 query
for user_id in user_ids:
    user = db.query(User).filter(User.id == user_id).first()  # Requête par user
    print(user.name)

# ✅ Query unique
users = db.query(User).filter(User.id.in_(user_ids)).all()
for user in users:
    print(user.name)
```

### Sécurité
- [ ] Pas d'injection SQL/XSS (utiliser les paramètres liés, ne pas concaténer).
- [ ] Pas de secrets en dur (clés, tokens, mots de passe).
- [ ] Validation des entrées utilisateur.
- [ ] Authentification/autorisation en place.

```python
# ❌ Injection SQL
query = f"SELECT * FROM users WHERE email = '{email}'"

# ✅ Requête paramétrée
query = "SELECT * FROM users WHERE email = ?"
db.execute(query, (email,))
```

### Testabilité
- [ ] Code testable (pas d'I/O hard-codée, injection de dépendances).
- [ ] Tests couvrent les chemins principaux et les cas limites.
- [ ] Tests pas flaky.

### Performance
- [ ] Pas de boucles n² évidentes.
- [ ] Mémoire : pas de fuites évidentes (variables globales qui s'accumulent).
- [ ] Si critique : benchmark ou profiling attendus.

### Maintenabilité
- [ ] Noms de variables/fonctions clairs.
- [ ] Fonctions courtes (< 30 lignes idéalement).
- [ ] Pas de dead code ou commentaires obsolètes.
- [ ] Dépendances bien limitées (low coupling, high cohesion).

## Processus de revue

1. **Auteur** : créer une PR avec une description claire.
2. **Reviewer** : parcourir en 15-30min, poser des questions si doutes.
3. **Discussion** : l'auteur répond aux remarques, propose des changements.
4. **Approbation** : reviewer approuve et merge (ou l'auteur merge si auto-approved).

### Tone
- ✅ "Avez-vous considéré l'utilisation d'une set au lieu d'une liste ici ?"
- ❌ "C'est inefficace."

- ✅ "J'aurais nommé ça `user_email_domain` pour plus de clarté."
- ❌ "Mauvais nommage."

## Exigences de merge

Configurer GitHub/GitLab pour exiger :

- [ ] Au minimum 1 approbation (2 pour `main`).
- [ ] Tests passants (100% succès en CI).
- [ ] Pas de conflits avec la branche cible.
- [ ] Branches à jour avec `develop` ou `main`.

```yaml
# .github/settings.yml (GitHub)
branch_protection_rules:
  - pattern: "main"
    required_status_checks: ["test", "lint", "type-check"]
    required_approving_review_count: 2
    dismiss_stale_reviews: true
    require_code_owner_reviews: true
```

## Code ownership

Créer un fichier `CODEOWNERS` pour assigner automatiquement des reviewers :

```
# .github/CODEOWNERS
/src/auth/**        @security-team
/src/api/**         @api-team
/src/db/**          @data-team
/tests/**           @everyone
README.md           @tech-leads
```
