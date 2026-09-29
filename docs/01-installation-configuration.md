# 1. Installation & configuration

[⬅ Retour au sommaire](README.md)

---

## 1.1 Installer Git

| Système | Commande / méthode |
|---------|--------------------|
| **Windows** | Télécharger sur <https://git-scm.com/download/win> (inclut *Git Bash*) ou `winget install --id Git.Git` |
| **macOS** | `brew install git` (ou `xcode-select --install`) |
| **Linux (Debian/Ubuntu)** | `sudo apt install git` |
| **Linux (Fedora)** | `sudo dnf install git` |

Vérifier l'installation :

```bash
git --version
```

---

## 1.2 Configuration obligatoire (une seule fois)

Votre nom et votre e-mail apparaissent dans **chaque commit**. Utilisez votre adresse professionnelle.

```bash
git config --global user.name "Prénom Nom"
git config --global user.email "prenom.nom@entreprise.com"
```

## 1.3 Configuration recommandée pour l'équipe

```bash
# Nom de la branche par défaut lors d'un git init
git config --global init.defaultBranch main

# Éditeur par défaut (VS Code)
git config --global core.editor "code --wait"

# git pull fait un rebase au lieu d'un merge (historique plus propre)
git config --global pull.rebase true

# Supprime automatiquement les références aux branches distantes supprimées
git config --global fetch.prune true

# Fins de ligne : Windows
git config --global core.autocrlf true
# Fins de ligne : macOS / Linux
git config --global core.autocrlf input

# Couleurs dans le terminal
git config --global color.ui auto
```

### Alias pratiques (facultatif)

```bash
git config --global alias.st "status -sb"
git config --global alias.co checkout
git config --global alias.sw switch
git config --global alias.br branch
git config --global alias.lg "log --oneline --graph --decorate --all"
git config --global alias.last "log -1 HEAD --stat"
git config --global alias.unstage "restore --staged"
```

Ensuite : `git st`, `git lg`, etc.

Voir toute votre configuration :

```bash
git config --global --list
```

---

## 1.4 Authentification auprès du serveur (GitHub / GitLab)

### Option A — Clé SSH (recommandée)

```bash
# 1. Générer une clé
ssh-keygen -t ed25519 -C "prenom.nom@entreprise.com"

# 2. Afficher la clé publique à copier
cat ~/.ssh/id_ed25519.pub
```

3. Coller la clé publique dans **GitHub → Settings → SSH and GPG keys** (ou GitLab → *Preferences → SSH Keys*).
4. Tester :

```bash
ssh -T git@github.com
```

### Option B — HTTPS + jeton d'accès (token)

Créez un *Personal Access Token* dans les paramètres de votre compte, puis utilisez-le à la place du mot de passe. Sous Windows, le *Git Credential Manager* (installé avec Git) le mémorise pour vous.

> ⚠️ Ne partagez jamais votre clé privée (`id_ed25519`) ni votre token.

---

## 1.5 Installer l'extension git-flow (facultatif)

L'outil `git-flow` automatise les commandes du workflow Gitflow (voir [chapitre 3](03-workflow-gitflow.md)).

| Système | Installation |
|---------|--------------|
| Windows | Inclus dans Git for Windows (`git flow` fonctionne dans Git Bash) |
| macOS | `brew install git-flow-avh` |
| Linux | `sudo apt install git-flow` |

Initialisation dans un dépôt :

```bash
git flow init -d   # -d = accepte les noms de branches par défaut
```

> ℹ️ L'extension est **facultative** : toutes les étapes du workflow sont aussi décrites avec les commandes Git classiques.

---

## 1.6 Outils graphiques conseillés

- **VS Code** (onglet *Source Control*) + extension *GitLens*
- **GitKraken**, **Sourcetree**, **Fork** (clients graphiques)
- **GitHub Desktop** (simple pour débuter)

> 💡 Les outils graphiques sont pratiques, mais comprenez d'abord les commandes : c'est ce qui vous sauvera en cas de problème.

---

## 1.7 Fichier `.gitignore`

Chaque projet doit contenir un `.gitignore` pour exclure les fichiers inutiles ou sensibles. Exemple :

```gitignore
# Dépendances
node_modules/
venv/
.venv/

# Build
dist/
build/
*.log

# Environnement & secrets
.env
.env.*
!.env.example

# IDE / OS
.vscode/
.idea/
.DS_Store
Thumbs.db
```

Modèles prêts à l'emploi : <https://github.com/github/gitignore>

---

[➡ Suite : Aide-mémoire des commandes](02-commandes-essentielles.md)
