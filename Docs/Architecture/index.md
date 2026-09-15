---
title: "Architecture"
nav_icon: "M17.25 6.75 22.5 12l-5.25 5.25m-10.5 0L1.5 12l5.25-5.25m7.5-3-4.5 16.5"
has_children: true
nav_order: 4
---

Documentation technique du projet : stack, architecture backend/frontend, traductions, tests et fonctionnement interne.

<div class="doc-card-grid" markdown="0">
  <a class="doc-card" href="stack_technique.md">
    <span class="doc-card-title">Stack technique</span>
    <p class="doc-card-desc">Technologies utilisées : backend PHP/Symfony, frontend Vue/Tailwind, thèmes Bootstrap, base de données, asynchrone et outillage de build</p>
  </a>
  <a class="doc-card" href="backend.md">
    <span class="doc-card-title">Architecture backend</span>
    <p class="doc-card-desc">Organisation du code PHP/Symfony : rangement, communication entre les classes, règles à connaître avant de modifier ou d'ajouter une fonctionnalité</p>
  </a>
  <a class="doc-card" href="frontend.md">
    <span class="doc-card-title">Architecture frontend</span>
    <p class="doc-card-desc">Articulation entre Twig et Vue côté admin</p>
  </a>
  <a class="doc-card" href="traductions_i18n.md">
    <span class="doc-card-title">Traductions & multilingue</span>
    <p class="doc-card-desc">Les deux mécanismes de traduction du CMS : le contenu rédigé (pages, tags...) et les textes fixes de l'interface</p>
  </a>
  <a class="doc-card" href="references_globales.md">
    <span class="doc-card-title">Références globales</span>
    <p class="doc-card-desc">Correspondance entre les valeurs numériques/mots-clés de l'API et les enums PHP qui les définissent</p>
  </a>
  <a class="doc-card" href="commandes.md">
    <span class="doc-card-title">Commandes Symfony</span>
    <p class="doc-card-desc">Pense-bête des commandes utiles au quotidien, standards et maison (natheo:)</p>
  </a>
  <a class="doc-card" href="tests.md">
    <span class="doc-card-title">Tests unitaires</span>
    <p class="doc-card-desc">Fonctionnement de PHPUnit sur le projet (WebTestCase, DAMA DoctrineTestBundle...)</p>
  </a>
  <a class="doc-card" href="surcharge_controllers.md">
    <span class="doc-card-title">Surcharge des controllers</span>
    <p class="doc-card-desc">Mécanisme pour personnaliser un écran sans modifier directement les fichiers du cœur</p>
  </a>
  <a class="doc-card" href="modele_donnees.md">
    <span class="doc-card-title">Modèle de données</span>
    <p class="doc-card-desc">Vue d'ensemble des tables et de leurs relations, générée à partir des entités Doctrine</p>
  </a>
</div>
