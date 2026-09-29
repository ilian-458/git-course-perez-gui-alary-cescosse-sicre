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
