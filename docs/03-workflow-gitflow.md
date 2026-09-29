# 3. Notre workflow d'équipe : Gitflow

[⬅ Retour au sommaire](README.md)

---

## 3.1 Pourquoi Gitflow ?

Nous avons comparé plusieurs workflows avant de choisir :

| Workflow | Principe | Avantages | Inconvénients |
|----------|----------|-----------|---------------|
| **Centralisé** | Tout le monde commit sur `main` | Très simple | Conflits fréquents, `main` souvent cassée |
| **GitHub Flow** | `main` + branches de feature courtes | Simple, idéal en déploiement continu | Pas de gestion des versions / de la recette |
| **Trunk-Based** | Intégration très fréquente dans le tronc | Rapide | Exige beaucoup de tests automatisés et de *feature flags* |
| **Gitflow** ✅ | Branches dédiées : `main`, `develop`, `feature`, `release`, `hotfix` | Versions maîtrisées, production toujours stable, travail en parallèle | Plus de branches à gérer |

**Notre choix : Gitflow**, car :

- nous livrons des **versions numérotées** (v1.0.0, v1.1.0…) après une phase de recette ;
- plusieurs développeurs travaillent **en parallèle** sur des fonctionnalités différentes ;
- la branche `main` doit **toujours** refléter ce qui est en production ;
- nous devons pouvoir **corriger un bug en production** sans livrer les fonctionnalités en cours.