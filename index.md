---
title: "Accueil"
nav_icon: "m4 12 8-8 8 8M6 10.5V19a1 1 0 0 0 1 1h3v-3a1 1 0 0 1 1-1h2a1 1 0 0 1 1 1v3h3a1 1 0 0 0 1-1v-8.5"
nav_order: 1
---

<div class="home-hero" markdown="0">
  <h1 class="home-hero-title">Documentation de Nathéo CMS</h1>
  <p class="home-hero-lead">
    Nathéo est un CMS <strong>headless</strong> open source : un back-office complet pour gérer pages, menus,
    médias, commentaires et utilisateurs, et une API JSON pour afficher ce contenu sur le front de votre choix.
  </p>
  <p class="home-current-version"><span class="version-badge-latest">Version actuelle</span><span class="version-entry-title">v2.0.0-beta.1</span><span class="version-tag version-tag-beta">Bêta</span><a href="Docs/Projet/versions.html">Voir toutes les versions →</a></p>
  <ul class="home-stack">
    <li>Symfony 8.1</li>
    <li>PHP 8.2+</li>
    <li>Vue 3 + TypeScript</li>
    <li>Vite</li>
    <li>Tailwind 4</li>
    <li>MySQL / PostgreSQL</li>
  </ul>
</div>

## Par où commencer ?

<div class="home-paths" markdown="0">
  <div class="home-path">
    <span class="home-path-icon">🚀</span>
    <span class="home-path-title">J'installe le CMS</span>
    <p class="home-path-desc">Vérifier l'environnement, puis installer via l'installeur graphique ou en ligne de commande.</p>
    <ul>
      <li><a href="Docs/Demarrage/pre_requis.html">Pré-requis</a></li>
      <li><a href="Docs/Demarrage/installation_prod.html">Installation via l'installeur</a></li>
      <li><a href="Docs/Demarrage/installation_dev.html">Installation développeur</a></li>
      <li><a href="Docs/Demarrage/configuration_installation.html">Options de configuration</a></li>
    </ul>
  </div>
  <div class="home-path">
    <span class="home-path-icon">🛠️</span>
    <span class="home-path-title">J'administre le contenu</span>
    <p class="home-path-desc">Prendre en main le back-office, écran par écran, avec captures et rôles requis.</p>
    <ul>
      <li><a href="Docs/GuideAdmin/Contenu/Pages/listing.html">Gérer les pages</a></li>
      <li><a href="Docs/GuideAdmin/Contenu/Menus/listing.html">Construire les menus</a></li>
      <li><a href="Docs/GuideAdmin/Contenu/Mediatheque/mediatheque.html">Utiliser la médiathèque</a></li>
      <li><a href="Docs/GuideAdmin/Systeme/Utilisateurs/listing.html">Gérer les utilisateurs</a></li>
    </ul>
  </div>
  <div class="home-path">
    <span class="home-path-icon">🔌</span>
    <span class="home-path-title">Je développe un front</span>
    <p class="home-path-desc">Consommer le contenu du CMS depuis un site ou une application via l'API JSON.</p>
    <ul>
      <li><a href="Docs/API/index.html">Règles communes de l'API</a></li>
      <li><a href="Docs/GuideAdmin/Systeme/ApiToken/jetons_api.html">Créer un jeton d'accès</a></li>
      <li><a href="Docs/API/References/find_page.html">Récupérer une page</a></li>
      <li><a href="Docs/API/References/find_menu.html">Récupérer un menu</a></li>
    </ul>
  </div>
  <div class="home-path">
    <span class="home-path-icon">🧩</span>
    <span class="home-path-title">Je contribue au code</span>
    <p class="home-path-desc">Comprendre l'architecture interne, les composants Vue réutilisables et les outils de dev.</p>
    <ul>
      <li><a href="Docs/Architecture/stack_technique.html">Stack technique</a></li>
      <li><a href="Docs/Architecture/backend.html">Architecture backend</a></li>
      <li><a href="Docs/Architecture/frontend.html">Architecture frontend</a></li>
      <li><a href="Docs/Architecture/composants/index.html">Composants</a></li>
    </ul>
  </div>
</div>

## Toute la documentation

<div class="doc-card-grid" markdown="0">
  <a class="doc-card" href="Docs/Projet/index.html">
    <span class="doc-card-title">Nathéo CMS</span>
    <p class="doc-card-desc">Présentation du projet et de l'approche headless, roadmap, changelog et historique des versions</p>
  </a>
  <a class="doc-card" href="Docs/Demarrage/index.html">
    <span class="doc-card-title">Démarrage</span>
    <p class="doc-card-desc">Pré-requis, installation (installeur graphique ou CLI), configuration et choix du SGBD</p>
  </a>
  <a class="doc-card" href="Docs/Architecture/index.html">
    <span class="doc-card-title">Architecture</span>
    <p class="doc-card-desc">Stack, backend Symfony, frontend Vue/Vite, traductions, modèle de données, commandes et tests</p>
  </a>
  <a class="doc-card" href="Docs/GuideAdmin/index.html">
    <span class="doc-card-title">Guide d'administration</span>
    <p class="doc-card-desc">Chaque écran du back-office : contenu, système, outils, modules, compte et recherche globale</p>
  </a>
  <a class="doc-card" href="Docs/Front/index.html">
    <span class="doc-card-title">Front</span>
    <p class="doc-card-desc">Le thème de démonstration Natheo-Horizon et le rendu public du site</p>
  </a>
  <a class="doc-card" href="Docs/API/index.html">
    <span class="doc-card-title">API</span>
    <p class="doc-card-desc">Authentification, format des réponses, pagination et référence de chaque endpoint</p>
  </a>
</div>

## Un aperçu du back-office

<div class="screenshot-row">
  <figure>
    <img src="Docs/Projet/files/dashboard.png" alt="Tableau de bord de l'administration">
    <figcaption>Le tableau de bord : statistiques, derniers commentaires et pages les plus vues.</figcaption>
  </figure>
  <figure>
    <img src="Docs/Projet/files/gestion-menu.png" alt="Gestion des menus">
    <figcaption>La gestion des menus, avec aperçu du rendu et arborescence.</figcaption>
  </figure>
  <figure>
    <img src="Docs/Projet/files/options-systems.png" alt="Options système">
    <figcaption>Les options système : nom du site, thème public, langue par défaut…</figcaption>
  </figure>
</div>

## Liens utiles

- **Code source** : [counteraccro/natheo sur GitHub](https://github.com/counteraccro/natheo)
- **Suivi du projet** : [roadmap](Docs/Projet/roadmap.md), [changelog](Docs/Projet/changelog.md) et [notes de version](https://github.com/counteraccro/natheo/releases)
- **Une question, un bug ?** Ouvrir une [issue sur GitHub](https://github.com/counteraccro/natheo/issues)

Utilisez la barre de recherche en haut de page pour retrouver rapidement un écran, un endpoint ou une commande.
