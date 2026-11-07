<div align="center">

# 📚 Documentation Nathéo CMS

[![Documentation](https://img.shields.io/badge/Docs-En%20ligne-blue?style=flat-square)](https://counteraccro.github.io/natheo.doc/)
[![CMS Version](https://img.shields.io/badge/NatheoCMS-v2.0.0-8b5cf6?style=flat-square)](https://github.com/counteraccro/natheo)
[![License](https://img.shields.io/badge/License-GNU-green?style=flat-square)](https://github.com/counteraccro/natheo/blob/master/LICENSE)

**Documentation technique et fonctionnelle complète de Nathéo CMS**

[Voir la documentation](https://counteraccro.github.io/natheo.doc/) · [Retour au CMS](https://github.com/counteraccro/natheo) · [Signaler une erreur](https://github.com/counteraccro/natheo.doc/issues)

</div>

> [!NOTE]  
> La documentation correspond à la version stable `v2.0.0` du CMS.
> Le suivi du projet se trouve dans la [roadmap](Docs/Projet/roadmap.md), le
> [changelog](Docs/Projet/changelog.md) et l'[historique des versions](Docs/Projet/versions.md).

---

## 📖 À propos

Ce repository contient l'ensemble de la documentation de **Nathéo CMS**, un CMS headless open source. Le site est généré par Jekyll avec le thème [just-the-docs](https://github.com/just-the-docs/just-the-docs), puis publié par GitHub Pages à partir de la branche `main`.

🌐 **Accéder à la documentation** : [https://counteraccro.github.io/natheo.doc/](https://counteraccro.github.io/natheo.doc/)

---

## 📋 Contenu de la documentation

La documentation est organisée en plusieurs sections :

| Section | Contenu |
|---|---|
| [Nathéo CMS](Docs/Projet/index.md) | Présentation du projet, roadmap, changelog et versions |
| [Démarrage](Docs/Demarrage/index.md) | Pré-requis, installation (installeur graphique ou CLI), configuration et base de données |
| [Architecture](Docs/Architecture/index.md) | Stack, backend Symfony, frontend Vue/Vite, traductions, modèle de données, composants, commandes et tests |
| [Guide d'administration](Docs/GuideAdmin/index.md) | Chaque écran du back-office : contenu, système, outils, modules, compte et recherche globale |
| [Front](Docs/Front/index.md) | Le rendu public du site et le thème de démonstration Natheo-Horizon |
| [API](Docs/API/index.md) | Authentification, format des réponses, pagination et référence de chaque endpoint |

---

## 🛠️ Développement local

Le site est construit par GitHub Pages à chaque push sur `main` : aucun workflow ni `Gemfile` n'est versionné dans ce repository.

Pour prévisualiser en local, il faut Ruby et Bundler, ainsi qu'un `Gemfile` à créer à la racine, sans le commiter :

```ruby
source "https://rubygems.org"
gem "github-pages", group: :jekyll_plugins
```

```bash
# Cloner le repository
git clone https://github.com/counteraccro/natheo.doc.git
cd natheo.doc

# Installer les dépendances
bundle install

# Lancer le serveur local
bundle exec jekyll serve

# Accéder à la documentation
# http://localhost:4000/natheo.doc/
```

---

## ✍️ Contribuer à la documentation

Les contributions pour améliorer la documentation sont les bienvenues ! 

### Comment contribuer ?

1. **Fork** ce repository
2. **Créez** une branche pour votre contribution
```bash
   git checkout -b docs/amelioration-section-x
```
3. **Modifiez** la documentation (respectez la structure existante)
4. **Commitez** vos changements
```bash
   git commit -m "docs: amélioration de la section X"
```
5. **Poussez** vers votre fork
```bash
   git push origin docs/amelioration-section-x
```
6. **Ouvrez** une Pull Request vers `main`

### Guidelines de contribution

- ✅ Utilisez un français clair et professionnel
- ✅ Ajoutez des exemples de code quand c'est pertinent
- ✅ Incluez des captures d'écran pour les tutoriels UI
- ✅ Vérifiez l'orthographe et la grammaire
- ✅ Testez vos modifications en local avant de soumettre

### Points d'attention

- **Menu latéral** : il est généré à partir du front-matter (`title`, `parent`, `grand_parent`, `nav_order`, `has_children`). Le `parent` doit reprendre mot pour mot le `title` de la page parente, sinon la page disparaît du menu sans erreur de build.
- **Exemples de code** : un bloc contenant une syntaxe à accolades de type Twig ou Vue doit être encadré des balises Liquid `raw` et `endraw`, sinon le build peut échouer.
- **JavaScript de `_includes/head_custom.html`** : utilisez uniquement des commentaires `/* */`. Le HTML publié est minifié sur une seule ligne, et un commentaire `//` y désactiverait tout le reste du script.

### Structure des fichiers
```
natheo.doc/
├── Docs/
│   ├── Projet/             # Présentation, roadmap, changelog, versions
│   ├── Demarrage/          # Pré-requis, installation, configuration, bases de données
│   ├── Architecture/       # Documentation technique (stack, backend, frontend, composants)
│   ├── GuideAdmin/         # Documentation fonctionnelle du back-office
│   ├── Front/              # Documentation de la partie publique du site
│   └── API/                # Règles communes et référence des endpoints
├── _includes/              # Personnalisation du thème (CSS, JS, navigation)
├── _layouts/               # Gabarits surchargés
├── _sass/color_schemes/    # Palette de couleurs du thème
├── index.md                # Page d'accueil
└── _config.yml             # Configuration Jekyll
```

---

## 🐛 Signaler une erreur

Vous avez trouvé une erreur, une information obsolète ou un lien cassé dans la documentation ?

[Créer une issue](https://github.com/counteraccro/natheo.doc/issues/new) en précisant :
- 📄 La page concernée (URL)
- ❌ Le problème identifié
- ✅ La correction suggérée (si vous en avez une)

---

## 🔗 Liens utiles

- 🏠 [Site de documentation](https://counteraccro.github.io/natheo.doc/)
- 💻 [Repository du CMS](https://github.com/counteraccro/natheo)
- 🐛 [Issues CMS](https://github.com/counteraccro/natheo/issues)

---

## 📜 Licence

Cette documentation est sous licence GNU-V3, comme le projet Nathéo CMS.

Voir le fichier [LICENSE](https://github.com/counteraccro/natheo/blob/master/LICENSE) pour plus de détails.

---

## 👥 Contributeurs de la documentation

Merci à tous ceux qui contribuent à améliorer cette documentation !

<!-- ALL-CONTRIBUTORS-LIST:START -->
<!-- Ajoutez ici la liste des contributeurs -->
<!-- ALL-CONTRIBUTORS-LIST:END -->

---

<div align="center">

**Documentation maintenue par l'équipe Nathéo**

[⬆ Retour en haut](#-documentation-nathéo-cms)

</div>

*Dernière mise à jour le 07/11/2026*
