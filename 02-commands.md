# 2. Aide-mémoire des commandes Git

[⬅ Retour au sommaire](README.md)

---

## 2.0 Les 3 zones de Git (à comprendre absolument)

```text
 Répertoire de travail  ──git add──▶  Zone de staging (index)  ──git commit──▶  Dépôt local  ──git push──▶  Dépôt distant
   (vos fichiers)                    (ce qui sera commité)                     (.git)                      (GitHub/GitLab)
         ◀──────────────────────────────── git pull / git fetch + merge ◀──────────────────────────────────────┘
```

---

## 2.1 Démarrer

| Commande | Description |
|----------|-------------|
| `git init` | Crée un nouveau dépôt dans le dossier courant |
| `git clone <url>` | Copie un dépôt distant sur votre machine |
| `git clone <url> mon-dossier` | Clone dans un dossier précis |

## 2.2 Consulter l'état

| Commande | Description |
|----------|-------------|
| `git status` | État des fichiers (modifiés, en staging, non suivis) |
| `git status -sb` | Version courte |
| `git diff` | Modifications **non** encore ajoutées |
| `git diff --staged` | Modifications ajoutées (prêtes à être commitées) |
| `git log` | Historique des commits |
| `git log --oneline --graph --all` | Historique compact et graphique |
| `git log -p <fichier>` | Historique détaillé d'un fichier |
| `git show <commit>` | Détail d'un commit |
| `git blame <fichier>` | Qui a modifié chaque ligne, et quand |

## 2.3 Enregistrer des modifications

| Commande | Description |
|----------|-------------|
| `git add <fichier>` | Ajoute un fichier au staging |
| `git add .` | Ajoute tout le dossier courant |
| `git add -p` | Ajoute **morceau par morceau** (très utile pour faire des commits propres) |
| `git commit -m "message"` | Crée un commit |
| `git commit` | Ouvre l'éditeur pour un message détaillé |
| `git commit --amend` | Modifie le **dernier** commit (si pas encore poussé !) |
| `git rm <fichier>` | Supprime un fichier et enregistre la suppression |
| `git mv <ancien> <nouveau>` | Renomme / déplace un fichier |

## 2.4 Branches

| Commande | Description |
|----------|-------------|
| `git branch` | Liste les branches locales |
| `git branch -a` | Liste toutes les branches (locales + distantes) |
| `git switch <branche>` | Change de branche |
| `git switch -c <branche>` | Crée une branche et bascule dessus |
| `git switch -c <branche> origin/<branche>` | Récupère une branche distante en local |
| `git branch -d <branche>` | Supprime une branche (déjà fusionnée) |
| `git branch -D <branche>` | Force la suppression ⚠️ |
| `git branch -m <nouveau-nom>` | Renomme la branche courante |

> ℹ️ `git checkout <branche>` et `git checkout -b <branche>` fonctionnent aussi (ancienne syntaxe).

## 2.5 Synchroniser avec le dépôt distant

| Commande | Description |
|----------|-------------|
| `git remote -v` | Liste les dépôts distants |
| `git fetch` | Télécharge les nouveautés **sans** modifier vos fichiers |
| `git pull` | `fetch` + intégration dans la branche courante |
| `git pull --rebase` | `fetch` + rebase (historique linéaire) |
| `git push` | Envoie vos commits |
| `git push -u origin <branche>` | Premier push d'une nouvelle branche (lie la branche locale à la distante) |
| `git push origin --delete <branche>` | Supprime une branche distante |
| `git push --force-with-lease` | Push forcé **sécurisé** (uniquement sur *votre* branche) |

## 2.6 Fusionner & réécrire

| Commande | Description |
|----------|-------------|
| `git merge <branche>` | Fusionne `<branche>` dans la branche courante |
| `git merge --no-ff <branche>` | Fusion avec commit de merge obligatoire (utilisé dans Gitflow) |
| `git merge --abort` | Annule une fusion en cours (conflit) |
| `git rebase <branche>` | Rejoue vos commits au-dessus de `<branche>` |
| `git rebase -i HEAD~3` | Réorganise / fusionne / renomme les 3 derniers commits |
| `git rebase --continue` / `--abort` | Poursuit / annule un rebase |
| `git cherry-pick <commit>` | Applique un commit précis sur la branche courante |

## 2.7 Mettre de côté (stash)

| Commande | Description |
|----------|-------------|
| `git stash` | Met de côté les modifications en cours |
| `git stash push -m "wip formulaire"` | Idem, avec un nom |
| `git stash -u` | Inclut aussi les fichiers non suivis |
| `git stash list` | Liste les stashs |
| `git stash pop` | Réapplique le dernier stash et le supprime |
| `git stash apply stash@{1}` | Réapplique un stash précis sans le supprimer |
| `git stash drop` | Supprime le dernier stash |

## 2.8 Annuler

| Situation | Commande |
|-----------|----------|
| Annuler les modifs d'un fichier (non ajouté) | `git restore <fichier>` |
| Retirer un fichier du staging | `git restore --staged <fichier>` |
| Annuler le dernier commit, **garder** les modifs | `git reset --soft HEAD~1` |
| Annuler le dernier commit, modifs remises en non-staging | `git reset HEAD~1` |
| Annuler le dernier commit **et** les modifs ⚠️ | `git reset --hard HEAD~1` |
| Annuler un commit **déjà poussé** (crée un commit inverse) | `git revert <commit>` |
| Retrouver un commit « perdu » | `git reflog` |

> ⚠️ **Règle d'or** : sur un commit déjà poussé et partagé, utilisez `git revert`, **jamais** `git reset --hard` + `push --force`.

## 2.9 Tags (versions)

| Commande | Description |
|----------|-------------|
| `git tag` | Liste les tags |
| `git tag -a v1.2.0 -m "Version 1.2.0"` | Crée un tag annoté |
| `git push origin v1.2.0` | Pousse un tag |
| `git push --tags` | Pousse tous les tags |
| `git tag -d v1.2.0` | Supprime un tag local |

## 2.10 Commandes git-flow (extension)

| Commande | Équivalent |
|----------|-----------|
| `git flow init -d` | Initialise Gitflow dans le dépôt |
| `git flow feature start <nom>` | Crée `feature/<nom>` depuis `develop` |
| `git flow feature publish <nom>` | Pousse la feature sur le serveur |
| `git flow feature finish <nom>` | Fusionne dans `develop` et supprime la branche |
| `git flow release start <version>` | Crée `release/<version>` depuis `develop` |
| `git flow release finish <version>` | Fusionne dans `main` + `develop`, crée le tag |
| `git flow hotfix start <version>` | Crée `hotfix/<version>` depuis `main` |
| `git flow hotfix finish <version>` | Fusionne dans `main` + `develop`, crée le tag |

> ℹ️ Chez nous, les `feature` sont intégrées via **Pull Request** (et non via `git flow feature finish`), afin de garantir la revue de code. Voir le [chapitre 3](03-workflow-gitflow.md).

---