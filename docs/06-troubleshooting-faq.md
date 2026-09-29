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
