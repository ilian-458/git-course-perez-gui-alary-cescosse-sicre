# 5. Bonnes pratiques & conventions

[⬅ Retour au sommaire](README.md)

---

## 5.1 Nommage des branches

**Format** : `<type>/<ticket>-<description-courte>`

- en **minuscules**, mots séparés par des **tirets** `-`
- sans accents, espaces ni caractères spéciaux
- description courte (3 à 5 mots)

| Type | Exemple |
|------|---------|
| `feature/` | `feature/PROJ-42-connexion-email` |
| `bugfix/` | `bugfix/PROJ-57-date-mal-formatee` |
| `release/` | `release/1.3.0` |
| `hotfix/` | `hotfix/1.3.1` |

❌ À éviter : `test`, `ma-branche`, `Feature/Login`, `fix_bug_pierre`, `nouvelle branche`

---
## 5.2 Messages de commit — Conventional Commits

**Format** :

```text
<type>(<portée>): <description à l'impératif, en minuscule, sans point final>

[corps optionnel : le POURQUOI du changement]

[pied optionnel : Refs: PROJ-42 / BREAKING CHANGE: ...]
```

### Types autorisés

| Type | Usage |
|------|-------|
| `feat` | Nouvelle fonctionnalité |
| `fix` | Correction de bug |
| `docs` | Documentation uniquement |
| `style` | Formatage, espaces, point-virgule (aucun impact sur le code) |
| `refactor` | Restructuration du code sans changer le comportement |
| `perf` | Amélioration des performances |
| `test` | Ajout ou modification de tests |
| `build` | Système de build, dépendances |
| `ci` | Configuration de l'intégration continue |
| `chore` | Tâches diverses (version, config…) |
| `revert` | Annulation d'un commit précédent |

### Exemples

✅ **Bons messages**

```text
feat(panier): ajoute le calcul automatique des frais de port
fix(auth): empêche la connexion avec un compte désactivé
docs(readme): précise la procédure d'installation
refactor(api): extrait la gestion des erreurs dans un middleware
```

```text
fix(paiement): corrige l'arrondi des montants TTC

Les montants étaient arrondis avant l'application de la TVA,
ce qui provoquait un écart d'un centime sur certaines commandes.

Refs: PROJ-88
```

❌ **Mauvais messages**

```text
fix
modifs
WIP
ça marche enfin !!!
update fichiers
correction du bug de jean + ajout page contact + refacto css
```

### Règles

- **Un commit = un changement logique.** Si votre message contient « et », faites peut-être deux commits.
- Ligne de titre ≤ **72 caractères**.
- Expliquez le **pourquoi** dans le corps, le *quoi* se voit dans le diff.
- Le code de chaque commit doit **compiler / fonctionner**.

---