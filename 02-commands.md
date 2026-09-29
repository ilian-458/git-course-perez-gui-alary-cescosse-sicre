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

## 2.6 Fusionner

| Commande | Description |
|----------|-------------|
| `git merg <branche>` | Fusionne `<branche>` dans la branche courante |
| `git merg --no-ff <branche>` | Fusion avec commit de merge obligatoire (utilisé dans Gitflow) |
| `git merg --abort` | Annule une fusion en cours (conflit) |