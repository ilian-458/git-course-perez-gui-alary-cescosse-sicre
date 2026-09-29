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
## 5.3 Bonnes pratiques au quotidien

### ✅ À faire

- `git pull` / `git fetch` **en début de journée**.
- Commits **petits et fréquents**.
- **Relire son diff** avant de commiter (`git diff --staged`).
- Utiliser `git add -p` plutôt que `git add .` pour maîtriser ce qu'on envoie.
- **Pousser sa branche** tous les soirs (sauvegarde).
- Garder des branches **courtes** (idéalement < 1 semaine).
- Se mettre à jour avec `develop` régulièrement.
- Supprimer ses branches une fois fusionnées.
- Utiliser `--force-with-lease` plutôt que `--force`.

### ❌ À ne pas faire

- Commiter directement sur `main` ou `develop`.
- `git push --force` sur une branche partagée.
- Réécrire l'historique (rebase, amend, reset) de commits **déjà partagés** avec d'autres.
- Commiter des fichiers générés (`node_modules/`, `dist/`, `*.log`…).
- Commiter des **secrets** (`.env`, clés API, mots de passe, certificats).
- Commiter de gros fichiers binaires (vidéos, archives…) → utiliser **Git LFS** si nécessaire.
- Laisser des branches « zombies » ouvertes pendant des semaines.
- Mélanger reformatage et changement fonctionnel dans le même commit.

---
## 5.4 Pull Requests

### Règles

- Une PR = **un sujet** (un ticket).
- Taille raisonnable : idéalement **< 400 lignes** modifiées. Au-delà, découper.
- Titre au format Conventional Commits + référence du ticket.
- Au moins **1 approbation** et une **CI verte** avant merge.
- C'est **l'auteur** qui merge après approbation.
- Les PR en cours peuvent être ouvertes en **Draft** pour obtenir des retours tôt.

### Modèle de description (`.github/pull_request_template.md`)

```markdown
## 🎯 Objectif
<!-- Que fait cette PR et pourquoi ? -->

Ticket : PROJ-XXX

## 🛠️ Changements
- ...
- ...

## 🧪 Comment tester
1. ...
2. ...

## 📸 Captures d'écran (si UI)

## ✅ Checklist
- [ ] Le code compile et les tests passent
- [ ] J'ai ajouté / mis à jour les tests
- [ ] J'ai mis à jour la documentation si nécessaire
- [ ] Pas de secret, de log de debug ou de code commenté
- [ ] Ma branche est à jour avec develop
```
