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
| `git dif` | Modifications **non** encore ajoutées |
| `git dif --staged` | Modifications ajoutées (prêtes à être commitées) |
| `git log` | Historique des commits |
| `git log --oneline --graph --all` | Historique compact et graphique |
| `git log -p <fichier>` | Historique détaillé d'un fichier |
| `git show <commit>` | Détail d'un commit |
| `git blame <fichier>` | Qui a modifié chaque ligne, et quand |
