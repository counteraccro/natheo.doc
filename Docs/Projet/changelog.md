---
title: "Changelog"
parent: "Nathéo CMS"
nav_order: 2
---

> ℹ️ Ce changelog n'est pas complet. Le
> reste est consultable sur les [issues fermées du projet](https://github.com/counteraccro/natheo/issues?q=is%3Aissue+is%3Aclosed).

## Septembre 2026

<div class="changelog-entry" markdown="1">
<p class="changelog-entry-title">Nouveaux blocs sur le tableau de bord</p>
<p class="changelog-entry-desc">Ajout des blocs « Dernières pages » et « Pages les plus vues » (migration du code associé en TypeScript).</p>
<p class="changelog-entry-links"><a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/326">Issue #326</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/329">PR #329</a></p>
</div>

<div class="changelog-entry" markdown="1">
<p class="changelog-entry-title">Statistiques sur le tableau de bord</p>
<p class="changelog-entry-desc">Ajout de statistiques (nombre de pages, de commentaires, de vues, d'appels API) sur le tableau de bord, chargées en AJAX pour ne pas ralentir l'affichage.</p>
<p class="changelog-entry-links"><a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/249">Issue #249</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/325">PR #325</a></p>
</div>

## Août 2026

<div class="changelog-entry" markdown="1">
<p class="changelog-entry-title">Refonte du module d'installation</p>
<p class="changelog-entry-desc">Refonte graphique du module d'installation et passage du JavaScript en TypeScript, blocage du lancement en environnement de production, prise en compte des migrations lors de l'installation, et correction de la prise en compte du schéma SQL par les migrations.</p>
<p class="changelog-entry-links"><a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/181">Issue #181</a> <a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/307">Issue #307</a> <a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/321">Issue #321</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/319">PR #319</a></p>
</div>

<div class="changelog-entry" markdown="1">
<p class="changelog-entry-title">Correction du Gestionnaire SQL</p>
<p class="changelog-entry-desc">Plusieurs erreurs JavaScript rendaient le Gestionnaire SQL inutilisable.</p>
<p class="changelog-entry-links"><a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/320">Issue #320</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/324">PR #324</a></p>
</div>

<div class="changelog-entry" markdown="1">
<p class="changelog-entry-title">Nettoyage de l'ancienne gestion des pages</p>
<p class="changelog-entry-desc">Suppression des anciens dossiers Vue (<code>Page_old</code>) laissés après la refonte de la gestion des pages.</p>
<p class="changelog-entry-links"><a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/312">Issue #312</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/313">PR #313</a></p>
</div>

## Juillet 2026

<div class="changelog-entry" markdown="1">
<p class="changelog-entry-title">Refonte de la gestion des pages</p>
<p class="changelog-entry-desc">Refonte graphique et technique de la gestion des pages (passage en TypeScript et Tailwind 4).</p>
<p class="changelog-entry-links"><a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/289">Issue #289</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/306">PR #306</a></p>
</div>

<div class="changelog-entry" markdown="1">
<p class="changelog-entry-title">Nettoyage à l'installation du CMS</p>
<p class="changelog-entry-desc">Les données temporaires générées par le CMS (<code>var/pageHistory</code>, <code>public/dump</code>) n'étaient pas purgées lors de l'installation.</p>
<p class="changelog-entry-links"><a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/216">Issue #216</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/305">PR #305</a></p>
</div>

<div class="changelog-entry" markdown="1">
<p class="changelog-entry-title">Harmonisation du bloc d'aide à la configuration</p>
<p class="changelog-entry-desc">Normalisation de l'en-tête du bloc « aide à la configuration », jusque-là différent des autres blocs du back-office.</p>
<p class="changelog-entry-links"><a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/303">Issue #303</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/304">PR #304</a></p>
</div>

<div class="changelog-entry" markdown="1">
<p class="changelog-entry-title">Passage des clés d'options système en enum</p>
<p class="changelog-entry-desc">Conversion des constantes des options système en enum PHP.</p>
<p class="changelog-entry-links"><a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/90">Issue #90</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/302">PR #302</a></p>
</div>

<div class="changelog-entry" markdown="1">
<p class="changelog-entry-title">Correction d'une erreur JS à la création d'un menu</p>
<p class="changelog-entry-desc">Correction d'une erreur Vue (prop <code>id</code> invalide) sur la page de création d'un menu.</p>
<p class="changelog-entry-links"><a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/300">Issue #300</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/301">PR #301</a></p>
</div>

<div class="changelog-entry" markdown="1">
<p class="changelog-entry-title">Harmonisation des interfaces du back-office</p>
<p class="changelog-entry-desc">Harmonisation des titres de blocs entre les différentes pages d'édition du back-office.</p>
<p class="changelog-entry-links"><a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/279">Issue #279</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/297">PR #297</a></p>
</div>

## Juin 2026

<div class="changelog-entry" markdown="1">
<p class="changelog-entry-title">Correction de plusieurs avertissements JS</p>
<p class="changelog-entry-desc">Corrections de divers avertissements JavaScript (éditeur d'email, listing des jetons API, gestionnaire de base de données).</p>
<p class="changelog-entry-links"><a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/295">Issue #295</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/296">PR #296</a></p>
</div>

<div class="changelog-entry" markdown="1">
<p class="changelog-entry-title">Nouvelle commande console pour les tests unitaires</p>
<p class="changelog-entry-desc">Ajout de la commande <code>natheo:install-test-unit</code> pour créer ou mettre à jour la base de données de test.</p>
<p class="changelog-entry-links"><a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/266">Issue #266</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/283">PR #283</a></p>
</div>

<div class="changelog-entry" markdown="1">
<p class="changelog-entry-title">Refonte de la gestion des menus</p>
<p class="changelog-entry-desc">Refonte graphique de la gestion des menus avec le nouveau design du back-office, conversion des positions de menu en enum, nettoyage du code obsolète, et corrections de l'ordre des éléments d'un menu et de la réinitialisation des menus par défaut.</p>
<p class="changelog-entry-links"><a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/126">Issue #126</a> <a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/247">Issue #247</a> <a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/251">Issue #251</a> <a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/277">Issue #277</a> <a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/280">Issue #280</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/278">PR #278</a></p>
</div>

<div class="changelog-entry" markdown="1">
<p class="changelog-entry-title">Contrôle des identifiants en mode édition</p>
<p class="changelog-entry-desc">En mode édition, un identifiant inexistant n'était pas détecté ; un message d'erreur est maintenant affiché (comme c'était déjà le cas pour les FAQ).</p>
<p class="changelog-entry-links"><a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/281">Issue #281</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/285">PR #285</a></p>
</div>

## Mai 2026

<div class="changelog-entry" markdown="1">
<p class="changelog-entry-title">CLI d'installation du CMS sur un serveur</p>
<p class="changelog-entry-desc">Travaux autour d'un outil en ligne de commande pour installer facilement le CMS sur un serveur (choix de la version du CMS, gestion des versions de PHP/Vue/Composer...).</p>
<p class="changelog-entry-links"><a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/187">Issue #187</a></p>
</div>

<div class="changelog-entry" markdown="1">
<p class="changelog-entry-title">Respect des droits sur le tableau de bord</p>
<p class="changelog-entry-desc">Les blocs « derniers commentaires » et « pages les plus vues » du tableau de bord ne s'affichent maintenant que pour les utilisateurs ayant les droits nécessaires (rôle contributeur ou plus).</p>
<p class="changelog-entry-links"><a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/184">Issue #184</a> <a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/275">Issue #275</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/276">PR #276</a></p>
</div>

<div class="changelog-entry" markdown="1">
<p class="changelog-entry-title">Masquage du bouton « Nouveau » selon les droits</p>
<p class="changelog-entry-desc">Le bouton « Nouveau + » et les liens qu'il propose sont maintenant masqués pour les utilisateurs qui n'ont pas les droits d'accès correspondants.</p>
<p class="changelog-entry-links"><a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/194">Issue #194</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/274">PR #274</a></p>
</div>

<div class="changelog-entry" markdown="1">
<p class="changelog-entry-title">Correction d'une regex de configuration</p>
<p class="changelog-entry-desc">Correction de l'expression régulière utilisée pour retrouver une clé dans le fichier de configuration.</p>
<p class="changelog-entry-links"><a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/272">Issue #272</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/273">PR #273</a></p>
</div>

<div class="changelog-entry" markdown="1">
<p class="changelog-entry-title">Correction de la purge des notifications</p>
<p class="changelog-entry-desc">Correction de la requête MariaDB utilisée pour purger les anciennes notifications déjà lues.</p>
<p class="changelog-entry-links"><a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/270">Issue #270</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/271">PR #271</a></p>
</div>

## Avril 2026

<div class="changelog-entry" markdown="1">
<p class="changelog-entry-title">Refonte de la gestion des commentaires</p>
<p class="changelog-entry-desc">Refonte de la gestion des commentaires avec le nouveau design du back-office, et conversion de leurs statuts en enum PHP.</p>
<p class="changelog-entry-links"><a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/75">Issue #75</a> <a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/241">Issue #241</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/245">PR #245</a></p>
</div>

<div class="changelog-entry" markdown="1">
<p class="changelog-entry-title">Retour des API via des DTO</p>
<p class="changelog-entry-desc">Remplacement des entités Doctrine par des DTO en retour des API, pour améliorer les performances.</p>
<p class="changelog-entry-links"><a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/78">Issue #78</a></p>
</div>

<div class="changelog-entry" markdown="1">
<p class="changelog-entry-title">Correction des icônes de la sidebar</p>
<p class="changelog-entry-desc">Les icônes n'apparaissaient plus dans le listing de la sidebar d'administration.</p>
<p class="changelog-entry-links"><a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/242">Issue #242</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/244">PR #244</a></p>
</div>

<div class="changelog-entry" markdown="1">
<p class="changelog-entry-title">Refonte de la médiathèque</p>
<p class="changelog-entry-desc">Refonte de la médiathèque avec les nouveaux standards du CMS, et suppression des méthodes dépréciées associées.</p>
<p class="changelog-entry-links"><a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/234">Issue #234</a> <a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/235">Issue #235</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/237">PR #237</a></p>
</div>

## Mars 2026

<div class="changelog-entry" markdown="1">
<p class="changelog-entry-title">Refonte de la FAQ</p>
<p class="changelog-entry-desc">Refonte de la FAQ avec le nouveau design du back-office : correction de l'affichage des FAQ désactivées côté front, de la génération des liens internes et des liens custom, et nettoyage du code devenu obsolète.</p>
<p class="changelog-entry-links"><a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/226">Issue #226</a> <a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/227">Issue #227</a> <a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/228">Issue #228</a> <a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/229">Issue #229</a> <a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/230">Issue #230</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/231">PR #231</a></p>
</div>
