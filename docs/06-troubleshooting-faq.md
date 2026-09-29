# 6. Dépannage & FAQ

[⬅ Retour au sommaire](README.md)

> 🧘 **Premier réflexe : pas de panique.** Avec Git, presque tout est récupérable grâce à `git reflog`.
> Avant toute commande destructive, faites une sauvegarde : `git branch sauvegarde-avant-bidouille`.

---

### ❓ J'ai commité sur `develop` (ou `main`) au lieu de ma branche

*Tant que ce n'est pas poussé :*

```bash
git branch feature/PROJ-42-ma-tache   # crée la branche avec vos commits
git reset --hard origin/develop       # remet develop comme sur le serveur
git switch feature/PROJ-42-ma-tache   # continuez ici
```

---

### ❓ Je me suis trompé dans le message de mon dernier commit

*Pas encore poussé :*

```bash
git commit --amend -m "feat(auth): nouveau message correct"
```

*Déjà poussé sur votre branche de feature :* idem, puis `git push --force-with-lease`.

---

### ❓ J'ai oublié un fichier dans mon dernier commit

```bash
git add fichier-oublie.js
git commit --amend --no-edit
```

---

### ❓ Je veux annuler mon dernier commit

| Cas | Commande |
|-----|----------|
| Pas poussé, je garde mes modifications | `git reset --soft HEAD~1` |
| Pas poussé, je jette tout ⚠️ | `git reset --hard HEAD~1` |
| **Déjà poussé** | `git revert HEAD` puis `git push` |

---

### ❓ J'ai des modifications en cours mais je dois changer de branche

```bash
git stash push -m "wip formulaire"
git switch autre-branche
# ... plus tard ...
git switch ma-branche
git stash pop
```

---

### ❓ `git push` est refusé : « rejected — non-fast-forward »

Quelqu'un a poussé avant vous. Récupérez ses changements puis repoussez :

```bash
git pull --rebase
git push
```

❌ Ne faites **pas** `git push --force` pour « forcer le passage ».
---

### ❓ `git pull` me dit que j'ai des modifications locales qui seraient écrasées

```bash
git stash
git pull
git stash pop
```

---

### ❓ J'ai supprimé une branche / fait un `reset --hard` par erreur

```bash
git reflog
# repérez la ligne correspondant à l'état voulu, par ex. :  a1b2c3d HEAD@{5}: commit: feat: ...
git branch recuperation a1b2c3d
```

`reflog` garde la trace de tous les déplacements de `HEAD` pendant environ 90 jours.

---

### ❓ J'ai commité un fichier qui aurait dû être ignoré

```bash
# 1. L'ajouter au .gitignore
echo ".env" >> .gitignore

# 2. Le retirer du suivi Git (sans le supprimer du disque)
git rm --cached .env
git commit -m "chore: retire .env du suivi git"
```

🔐 **S'il s'agit d'un secret** : il reste dans l'historique → **changez-le immédiatement** et prévenez le Tech Lead (voir [Sécurité](05-bonnes-pratiques.md#56-sécurité)).

---

### ❓ Je suis au milieu d'un merge / rebase et je suis perdu

```bash
git status            # Git indique où vous en êtes
git merge --abort     # annule le merge
git rebase --abort    # annule le rebase
```

Vous revenez exactement à l'état d'avant.

---

### ❓ « You are in 'detached HEAD' state »

Vous êtes sur un commit précis, pas sur une branche. Pour revenir :

```bash
git switch develop
```

Si vous avez fait des commits dans cet état et voulez les garder :

```bash
git switch -c ma-nouvelle-branche
```

---

### ❓ Je veux récupérer un seul commit d'une autre branche

```bash
git switch ma-branche
git cherry-pick <hash-du-commit>
```

---

### ❓ Je veux voir qui a modifié une ligne et pourquoi

```bash
git blame -L 40,60 src/panier.js
git show <hash>
```

---

### ❓ Je veux retrouver le commit qui a introduit un bug

```bash
git bisect start
git bisect bad                 # la version actuelle est buggée
git bisect good v1.2.0         # cette version fonctionnait
# Git vous place sur un commit intermédiaire : testez puis tapez
git bisect good   # ou   git bisect bad
# ... répétez jusqu'à ce que Git trouve le coupable ...
git bisect reset
```

---

### ❓ Ma branche distante a été supprimée mais apparaît encore

```bash
git fetch --prune
```

---

### ❓ Supprimer les branches locales déjà fusionnées

```bash
# Bash (Linux / macOS / Git Bash)
git branch --merged develop | grep -vE "^\*|main|develop" | xargs git branch -d
```

```powershell
# PowerShell
git branch --merged develop | Where-Object { $_ -notmatch '^\*|main|develop' } | ForEach-Object { git branch -d $_.Trim() }
```

---

### ❓ Différence entre `merge` et `rebase` ?

| | `merge` | `rebase` |
|---|---------|----------|
| Historique | Conserve tout, ajoute un commit de merge | Linéaire, réécrit vos commits |
| Sécurité | Sans risque sur une branche partagée | ⚠️ Uniquement sur **votre** branche |
| Usage chez nous | Intégrations `release`/`hotfix` → `main`/`develop` | Mettre à jour sa `feature` avec `develop` |

---

### ❓ Différence entre `fetch` et `pull` ?

- `git fetch` : télécharge les nouveautés du serveur **sans toucher** à vos fichiers.
- `git pull` : `fetch` **+** intègre les changements dans votre branche courante.

---

### ❓ Différence entre `reset`, `revert` et `restore` ?

| Commande | Agit sur | Réécrit l'historique ? |
|----------|----------|------------------------|
| `git restore` | Les fichiers (répertoire de travail / staging) | Non |
| `git reset` | La position de la branche (commits locaux) | **Oui** ⚠️ |
| `git revert` | Crée un nouveau commit qui annule un ancien | Non ✅ (sûr pour les commits poussés) |

---

## 🆘 Toujours bloqué ?

1. Ne lancez pas de commande au hasard trouvée sur internet.
2. Faites une sauvegarde : `git branch sauvegarde-$(date +%Y%m%d)`.
3. Copiez la sortie de `git status` et `git log --oneline -10`.
4. Demandez de l'aide sur `#git-help` ou à votre Tech Lead.

---

[⬅ Précédent : Bonnes pratiques](05-bonnes-pratiques.md) · [🏠 Retour au sommaire](README.md)
