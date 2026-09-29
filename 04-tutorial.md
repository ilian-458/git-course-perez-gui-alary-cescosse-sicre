Chaque tutoriel donne **les commandes Git classiques** et, quand c'est utile, l'équivalent avec l'extension **git-flow**.

- [Tuto 1 — Récupérer un projet (premier jour)](#tuto-1--récupérer-un-projet-premier-jour)
- [Tuto 2 — Développer une fonctionnalité (feature)](#tuto-2--développer-une-fonctionnalité-feature)
- [Tuto 3 — Préparer une release](#tuto-3--préparer-une-release)
- [Tuto 4 — Corriger un bug en production (hotfix)](#tuto-4--corriger-un-bug-en-production-hotfix)
- [Tuto 5 — Résoudre un conflit](#tuto-5--résoudre-un-conflit)
- [Tuto 6 — Mettre sa branche à jour avec develop](#tuto-6--mettre-sa-branche-à-jour-avec-develop)
- [Tuto 7 — Faire une revue de code](#tuto-7--faire-une-revue-de-code)
- [Tuto 8 — Nettoyer ses commits avant la PR](#tuto-8--nettoyer-ses-commits-avant-la-pr)

---

## Tuto 1 — Récupérer un projet (premier jour)

```bash
# 1. Cloner le dépôt
git clone git@github.com:entreprise/mon-projet.git
cd mon-projet

# 2. Récupérer la branche develop
git switch develop

# 3. (Optionnel) Initialiser git-flow
git flow init -d

# 4. Vérifier
git branch -a
git log --oneline --graph -10
```

✅ Vous êtes prêt à travailler.

---

## Tuto 2 — Développer une fonctionnalité (feature)

**Contexte** : vous devez réaliser le ticket `PROJ-42 — Ajouter la connexion par e-mail`.

### Étape 1 : partir d'un develop à jour

```bash
git switch develop
git pull
```

### Étape 2 : créer la branche

```bash
git switch -c feature/PROJ-42-connexion-email
```

> git-flow : `git flow feature start PROJ-42-connexion-email`

### Étape 3 : coder et commiter régulièrement

```bash
git status                 # que se passe-t-il ?
git diff                   # qu'est-ce que j'ai modifié ?
git add -p                 # j'ajoute morceau par morceau
git commit -m "feat(auth): ajoute le formulaire de connexion par e-mail"

# ... on continue ...
git add src/auth/validator.js
git commit -m "feat(auth): valide le format de l'adresse e-mail"
```

### Étape 4 : pousser la branche (sauvegarde + visibilité)

```bash
git push -u origin feature/PROJ-42-connexion-email
# Les fois suivantes, simplement :
git push
```

### Étape 5 : se remettre à jour avant la PR

```bash
git fetch origin
git rebase origin/develop
# En cas de conflit → voir Tuto 5
git push --force-with-lease   # nécessaire après un rebase (votre branche uniquement !)
```

### Étape 6 : ouvrir la Pull Request

Sur GitHub/GitLab :

- **Base** : `develop` ← **Compare** : `feature/PROJ-42-connexion-email`
- **Titre** : `feat(auth): connexion par e-mail [PROJ-42]`
- Remplir la description (voir [modèle de PR](05-bonnes-pratiques.md#54-pull-requests))
- Assigner au moins **un relecteur**

### Étape 7 : intégrer les retours de la revue

```bash
# Corriger le code, puis :
git add .
git commit -m "fix(auth): prend en compte les retours de revue"
git push
```

### Étape 8 : merge et nettoyage

Après approbation et CI verte, cliquez sur **Squash and merge**. Ensuite, en local :

```bash
git switch develop
git pull
git branch -d feature/PROJ-42-connexion-email
```

🎉 Fonctionnalité livrée dans `develop` !

---

## Tuto 3 — Préparer une release

**Rôle** : responsable de release. **Contexte** : on prépare la version `1.3.0`.

### Étape 1 : créer la branche de release

```bash
git switch develop
git pull
git switch -c release/1.3.0
```

> git-flow : `git flow release start 1.3.0`

### Étape 2 : mettre à jour la version et le changelog

```bash
# Exemple pour un projet Node.js (sans créer de tag) :
npm version 1.3.0 --no-git-tag-version
# Éditer CHANGELOG.md
git add package.json package-lock.json CHANGELOG.md
git commit -m "chore(release): prépare la version 1.3.0"
git push -u origin release/1.3.0
```

### Étape 3 : recette

- Déployer `release/1.3.0` sur l'environnement de **pré-production**.
- Les bugs trouvés sont corrigés **sur la branche release** (directement ou via `bugfix/*` + PR).
- ❌ Aucune nouvelle fonctionnalité sur cette branche.

### Étape 4 : finaliser

```bash
# 1. Merge dans main + tag
git switch main
git pull
git merge --no-ff release/1.3.0 -m "Merge release/1.3.0"
git tag -a v1.3.0 -m "Version 1.3.0"
git push origin main --follow-tags

# 2. Report dans develop
git switch develop
git pull
git merge --no-ff release/1.3.0 -m "Merge release/1.3.0 into develop"
git push origin develop

# 3. Nettoyage
git branch -d release/1.3.0
git push origin --delete release/1.3.0
```

> git-flow : `git flow release finish 1.3.0` puis `git push origin main develop --tags`
>
> ℹ️ Si `main` et `develop` sont protégées, les étapes 1 et 2 se font via **deux Pull Requests** (`release/1.3.0 → main`, puis `release/1.3.0 → develop`) en choisissant **Create a merge commit**, puis on crée le tag sur `main`.

### Étape 5 : déploiement

Le tag `v1.3.0` déclenche (ou sert de référence pour) le déploiement en production.

---

## Tuto 4 — Corriger un bug en production (hotfix)

**Contexte** : la version `v1.3.0` est en production, le paiement plante. On livre `1.3.1`.

```bash
# 1. Partir de main (la production)
git switch main
git pull
git switch -c hotfix/1.3.1
```

> git-flow : `git flow hotfix start 1.3.1`

```bash
# 2. Corriger + incrémenter la version
git add .
git commit -m "fix(paiement): corrige l'arrondi des montants TTC"
git commit -am "chore(release): version 1.3.1"
git push -u origin hotfix/1.3.1
```

```bash
# 3. Merge dans main + tag (ou via PR si main est protégée)
git switch main
git merge --no-ff hotfix/1.3.1
git tag -a v1.3.1 -m "Hotfix 1.3.1"
git push origin main --follow-tags

# 4. Report dans develop (ou dans release/* si une release est en cours !)
git switch develop
git pull
git merge --no-ff hotfix/1.3.1
git push origin develop

# 5. Nettoyage
git branch -d hotfix/1.3.1
git push origin --delete hotfix/1.3.1
```

> git-flow : `git flow hotfix finish 1.3.1` puis `git push origin main develop --tags`

⚠️ **N'oubliez jamais l'étape 4**, sinon le bug réapparaîtra à la prochaine release.

---

## Tuto 5 — Résoudre un conflit

Un conflit survient quand deux personnes ont modifié **les mêmes lignes** d'un fichier.

### 1. Git vous signale le conflit

```text
CONFLICT (content): Merge conflict in src/config.js
```

```bash
git status   # liste les fichiers en conflit ("both modified")
```

### 2. Ouvrir le fichier

```text
<<<<<<< HEAD
const timeout = 3000;
=======
const timeout = 5000;
>>>>>>> feature/PROJ-42-connexion-email
```

- Entre `<<<<<<<` et `=======` : la version de la branche courante.
- Entre `=======` et `>>>>>>>` : la version entrante.

### 3. Choisir / combiner, puis supprimer les marqueurs

```js
const timeout = 5000;
```

> 💡 VS Code propose des boutons *Accept Current / Accept Incoming / Accept Both*.
> En cas de doute, **discutez avec l'auteur de l'autre modification**.

### 4. Marquer comme résolu et continuer

```bash
git add src/config.js

# Si vous étiez en train de faire un merge :
git commit

# Si vous étiez en train de faire un rebase :
git rebase --continue
```

### 5. Vous êtes perdu ? Annulez tout et recommencez

```bash
git merge --abort    # ou
git rebase --abort
```

---

## Tuto 6 — Mettre sa branche à jour avec develop

**Méthode recommandée : rebase** (historique linéaire, branche personnelle uniquement)

```bash
git switch feature/PROJ-42-connexion-email
git fetch origin
git rebase origin/develop
git push --force-with-lease
```

**Alternative : merge** (si plusieurs personnes travaillent sur la même branche)

```bash
git switch feature/PROJ-42-connexion-email
git fetch origin
git merge origin/develop
git push
```

> 💡 Faites-le **souvent** (au moins une fois par jour) : de petits conflits réguliers valent mieux qu'un énorme conflit à la fin.

---

## Tuto 7 — Faire une revue de code

1. Récupérer la branche en local pour tester si nécessaire :

   ```bash
   git fetch origin
   git switch feature/PROJ-51-export-pdf
   ```

2. Vérifier (voir la [checklist de revue](05-bonnes-pratiques.md#55-revue-de-code)) :
   - Le code fait-il ce que le ticket demande ?
   - Est-il lisible, testé, sans secret ni code mort ?
3. Laisser des commentaires **constructifs** et précis.
4. **Approve** ou **Request changes**.

---

## Tuto 8 — Nettoyer ses commits avant la PR

Vous avez fait `wip`, `fix`, `fix2`, `oups`… Réorganisons avant de demander une revue :

```bash
git rebase -i origin/develop
```

L'éditeur s'ouvre :

```text
pick a1b2c3d feat(auth): ajoute le formulaire
pick d4e5f6a wip
pick 7g8h9i0 fix typo
pick j1k2l3m feat(auth): valide l'e-mail
```

Remplacez `pick` par :

| Mot-clé | Effet |
|---------|-------|
| `pick` | garder le commit |
| `reword` | garder mais modifier le message |
| `squash` | fusionner avec le commit précédent (combine les messages) |
| `fixup` | fusionner avec le précédent (ignore ce message) |
| `drop` | supprimer le commit |

```text
pick a1b2c3d feat(auth): ajoute le formulaire
fixup d4e5f6a wip
fixup 7g8h9i0 fix typo
pick j1k2l3m feat(auth): valide l'e-mail
```

Enregistrez, fermez, puis :

```bash
git push --force-with-lease
```

> ⚠️ Uniquement sur **votre** branche de feature, jamais sur `develop` ou `main`.

---