---
title: "Roadmap"
parent: "Nathéo CMS"
nav_order: 1
---

Liste non exhaustive des futures évolutions du CMS ainsi que son historique d'évolution.

<div class="roadmap-section">
<h3 class="roadmap-section-title"><span class="roadmap-section-icon">🐛</span> Bugs</h3>
<p class="roadmap-empty">Recherche en cours...</p>
</div>

<div class="roadmap-section">
<h3 class="roadmap-section-title"><span class="roadmap-section-icon">🔧</span> Changements mineurs</h3>
<ul class="roadmap-list">
<li class="roadmap-item is-pending">
  <span class="roadmap-item-icon"></span>
  <span class="roadmap-item-text">Exportation / importation sous forme de CSV des traductions du contenu du site (en base de données)</span>
</li>
<li class="roadmap-item is-done">
  <span class="roadmap-item-icon"></span>
  <div class="roadmap-item-body">
    <span class="roadmap-item-text">Éditeur Markdown <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/45">PR #45</a></span>
    <ul class="roadmap-sublist">
      <li class="roadmap-item is-pending"><span class="roadmap-item-icon"></span><span class="roadmap-item-text">Ajouter des liens de page dans l'éditeur de texte</span></li>
      <li class="roadmap-item is-pending"><span class="roadmap-item-icon"></span><span class="roadmap-item-text">Ajouter un bouton qui ouvre un nouvel onglet pour avoir un aperçu du rendu</span></li>
    </ul>
  </div>
</li>
<li class="roadmap-item is-done">
  <span class="roadmap-item-icon"></span>
  <span class="roadmap-item-text">Dans les listings, pouvoir filtrer uniquement avec ses contenus <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/46">PR #46</a></span>
</li>
<li class="roadmap-item is-pending">
  <span class="roadmap-item-icon"></span>
  <div class="roadmap-item-body">
    <span class="roadmap-item-text">Dans le dashboard créer les blocks suivants :</span>
    <ul class="roadmap-sublist">
      <li class="roadmap-item is-done"><span class="roadmap-item-icon"></span><span class="roadmap-item-text">Derniers commentaires <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/49">PR #49</a></span></li>
      <li class="roadmap-item is-pending"><span class="roadmap-item-icon"></span><span class="roadmap-item-text">Mes dernières pages</span></li>
      <li class="roadmap-item is-pending"><span class="roadmap-item-icon"></span><span class="roadmap-item-text">Mes notifications</span></li>
    </ul>
  </div>
</li>
<li class="roadmap-item is-pending">
  <span class="roadmap-item-icon"></span>
  <span class="roadmap-item-text">Revoir le code dans <code>DatabaseTablePrefixListener.php</code> pour pouvoir proprement séparer le schéma SQL du nom de la base de données</span>
</li>
<li class="roadmap-item is-pending">
  <span class="roadmap-item-icon"></span>
  <span class="roadmap-item-text">Mise en place API pour les commentaires</span>
</li>
<li class="roadmap-item is-pending">
  <span class="roadmap-item-icon"></span>
  <span class="roadmap-item-text">Mise en place API pour les options systems</span>
</li>
</ul>
</div>

<div class="roadmap-section">
<h3 class="roadmap-section-title"><span class="roadmap-section-icon">🚀</span> Changement majeur</h3>
<ul class="roadmap-list">
<li class="roadmap-item is-pending">
  <span class="roadmap-item-icon"></span>
  <div class="roadmap-item-body">
    <span class="roadmap-item-text">Mise en place d'un système de versioning pour les pages</span>
    <ul class="roadmap-sublist">
      <li class="roadmap-item is-pending"><span class="roadmap-item-icon"></span><span class="roadmap-item-text">Pouvoir éditer une page publier sans modifier la page actuellement publier</span></li>
    </ul>
  </div>
</li>
<li class="roadmap-item is-pending">
  <span class="roadmap-item-icon"></span>
  <div class="roadmap-item-body">
    <span class="roadmap-item-text">Gérer les mises à jour des traductions du CMS (fichier yaml)</span>
    <ul class="roadmap-sublist">
      <li class="roadmap-item is-pending"><span class="roadmap-item-icon"></span><span class="roadmap-item-text">Créer une copie des traductions actuelles pour les sauvegarder</span></li>
      <li class="roadmap-item is-pending"><span class="roadmap-item-icon"></span><span class="roadmap-item-text">Proposer un merge automatique des traductions actuelles vers les nouvelles pour ne pas perdre les éventuelles modifications faites par l'utilisateur.</span></li>
    </ul>
  </div>
</li>
</ul>
</div>

<div class="roadmap-section">
<h3 class="roadmap-section-title"><span class="roadmap-section-icon">📦</span> Nouveau module</h3>
<ul class="roadmap-list">
<li class="roadmap-item is-pending">
  <span class="roadmap-item-icon"></span>
  <div class="roadmap-item-body">
    <span class="roadmap-item-text">Module calendrier</span>
    <ul class="roadmap-sublist">
      <li class="roadmap-item is-pending"><span class="roadmap-item-icon"></span><span class="roadmap-item-text">Pouvoir gérer une prise de rendez-vous depuis le CMS</span></li>
      <li class="roadmap-item is-pending"><span class="roadmap-item-icon"></span><span class="roadmap-item-text">Gérer les rdv</span></li>
    </ul>
  </div>
</li>
<li class="roadmap-item is-pending">
  <span class="roadmap-item-icon"></span>
  <div class="roadmap-item-body">
    <span class="roadmap-item-text">Module de formulaire</span>
    <ul class="roadmap-sublist">
      <li class="roadmap-item is-pending"><span class="roadmap-item-icon"></span><span class="roadmap-item-text">Pouvoir créer un formulaire</span></li>
      <li class="roadmap-item is-pending"><span class="roadmap-item-icon"></span><span class="roadmap-item-text">Gérer le contenu de celui-ci + les réponses possibles</span></li>
    </ul>
  </div>
</li>
</ul>
</div>

<div class="roadmap-section">
<h3 class="roadmap-section-title"><span class="roadmap-section-icon">🗂️</span> Divers</h3>
<ul class="roadmap-list">
<li class="roadmap-item is-pending">
  <span class="roadmap-item-icon"></span>
  <span class="roadmap-item-text">APIsation du back-office</span>
</li>
<li class="roadmap-item is-pending">
  <span class="roadmap-item-icon"></span>
  <span class="roadmap-item-text">Refonte design back-office</span>
</li>
<li class="roadmap-item is-pending">
  <span class="roadmap-item-icon"></span>
  <span class="roadmap-item-text">Utilisation des DTO au lieu des objets Doctrine pour les API</span>
</li>
<li class="roadmap-item is-done">
  <span class="roadmap-item-icon"></span>
  <div class="roadmap-item-body">
    <span class="roadmap-item-text">Ajouter nouveau droit</span>
    <ul class="roadmap-sublist">
      <li class="roadmap-item is-done"><span class="roadmap-item-icon"></span><span class="roadmap-item-text">Ne vois que ses propres contenus créés. <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/46">PR #46</a></span></li>
    </ul>
  </div>
</li>
</ul>
</div>
