---
title: "Changelog"
parent: "Nathéo CMS"
nav_order: 2
---

> ℹ️ Ce changelog n'est pas complet. Le
> reste est consultable sur les [issues fermées du projet](https://github.com/counteraccro/natheo/issues?q=is%3Aissue+is%3Aclosed).

## Novembre 2026

<div class="changelog-entry" markdown="1">
<p class="changelog-entry-title"><span class="changelog-tag changelog-tag-feature">Feature</span>Traductions : relecture et interface complète en anglais et en espagnol</p>
<p class="changelog-entry-desc">Français relu sur tous les domaines, anglais et espagnol complets, test qui refuse les traductions incomplètes. Lien d'aide de la notification de bienvenue corrigé.</p>
<p class="changelog-entry-links"><a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/421">Issue #421</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/452">PR #452</a></p>
</div>

<div class="changelog-entry" markdown="1">
<p class="changelog-entry-title"><span class="changelog-tag changelog-tag-feature">Feature</span>Convention de nommage des clés de traduction</p>
<p class="changelog-entry-desc">Toutes les clés suivent le format <code>&lt;domaine&gt;.&lt;écran&gt;.&lt;élément&gt;</code>, un test contrôle la convention et les clés mortes sont supprimées. Les clés de la sidebar stockées en base sont migrées.</p>
<p class="changelog-entry-links"><a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/431">Issue #431</a> <a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/432">Issue #432</a> <a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/433">Issue #433</a> <a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/434">Issue #434</a> <a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/435">Issue #435</a> <a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/436">Issue #436</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/446">PR #446</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/447">PR #447</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/448">PR #448</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/449">PR #449</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/450">PR #450</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/451">PR #451</a></p>
</div>

<div class="changelog-entry" markdown="1">
<p class="changelog-entry-title"><span class="changelog-tag changelog-tag-bug">Bug</span>Onglets inactifs</p>
<p class="changelog-entry-desc">Les onglets des notifications, du gestionnaire de base de données et de la recherche globale réagissent de nouveau au clic.</p>
<p class="changelog-entry-links"><a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/444">Issue #444</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/445">PR #445</a></p>
</div>

<div class="changelog-entry" markdown="1">
<p class="changelog-entry-title"><span class="changelog-tag changelog-tag-bug">Bug</span>Flash du mode clair au changement de page</p>
<p class="changelog-entry-desc">Le thème et le mode sombre sont appliqués avant le premier affichage de la page.</p>
<p class="changelog-entry-links"><a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/438">Issue #438</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/443">PR #443</a></p>
</div>

<div class="changelog-entry" markdown="1">
<p class="changelog-entry-title"><span class="changelog-tag changelog-tag-feature">Feature</span>Sidebar sur les pages filles</p>
<p class="changelog-entry-desc">Sur une page d'ajout, d'édition ou de détail, le lien de la liste reste actif et son sous-menu reste ouvert.</p>
<p class="changelog-entry-links"><a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/441">Issue #441</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/442">PR #442</a></p>
</div>

<div class="changelog-entry" markdown="1">
<p class="changelog-entry-title"><span class="changelog-tag changelog-tag-feature">Feature</span>Cohérence des boutons de l'admin</p>
<p class="changelog-entry-desc">Ordre, couleur, taille et icônes harmonisés dans les formulaires et les modales. Les « Annuler » qui faisaient un retour deviennent « Retour ».</p>
<p class="changelog-entry-links"><a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/439">Issue #439</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/440">PR #440</a></p>
</div>

<div class="changelog-entry" markdown="1">
<p class="changelog-entry-title"><span class="changelog-tag changelog-tag-feature">Feature</span><span class="changelog-tag changelog-tag-bug">Bug</span>En-tête et titres des pages de l'admin</p>
<p class="changelog-entry-desc">En-tête commun à toutes les pages, onglet du navigateur identique au titre, libellés français corrigés et sidebar entièrement en français (Tableau de bord, Contenus, Outils, Tokens API...).</p>
<p class="changelog-entry-links"><a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/424">Issue #424</a> <a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/425">Issue #425</a> <a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/426">Issue #426</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/428">PR #428</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/429">PR #429</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/430">PR #430</a></p>
</div>

<div class="changelog-entry" markdown="1">
<p class="changelog-entry-title"><span class="changelog-tag changelog-tag-feature">Feature</span>Renforcement du compte et des notifications</p>
<p class="changelog-entry-desc">Jetons CSRF sur les actions du compte et des notifications, « tout marquer comme lu » passe en POST.</p>
<p class="changelog-entry-links"><a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/422">Issue #422</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/423">PR #423</a></p>
</div>

<div class="changelog-entry" markdown="1">
<p class="changelog-entry-title"><span class="changelog-tag changelog-tag-feature">Feature</span><span class="changelog-tag changelog-tag-bug">Bug</span>Nettoyage du front de l'admin</p>
<p class="changelog-entry-desc">Retrait complet de Bootstrap et des dépendances inutilisées, tous les composants en TypeScript, textes traduits, toasts et pagination uniques, erreurs AJAX affichées, couleurs du thème en classes Tailwind. L'option de confirmation avant de quitter un formulaire fonctionne de nouveau.</p>
<p class="changelog-entry-links"><a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/402">Issue #402</a> <a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/403">Issue #403</a> <a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/404">Issue #404</a> <a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/405">Issue #405</a> <a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/406">Issue #406</a> <a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/407">Issue #407</a> <a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/408">Issue #408</a> <a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/409">Issue #409</a> <a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/410">Issue #410</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/412">PR #412</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/413">PR #413</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/414">PR #414</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/415">PR #415</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/416">PR #416</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/417">PR #417</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/418">PR #418</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/419">PR #419</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/420">PR #420</a></p>
</div>

<div class="changelog-entry" markdown="1">
<p class="changelog-entry-title"><span class="changelog-tag changelog-tag-feature">Feature</span>Réorganisation du code</p>
<p class="changelog-entry-desc">Les dossiers <code>src/Utils</code> et <code>assets/utils</code> disparaissent : classes rangées par rôle (services, enums, factories, populates, formateurs, composables) et classes de traduction dans <code>src/Translate</code>.</p>
<p class="changelog-entry-links"><a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/377">Issue #377</a> <a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/379">Issue #379</a> <a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/389">Issue #389</a> <a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/391">Issue #391</a> <a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/393">Issue #393</a> <a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/395">Issue #395</a> <a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/397">Issue #397</a> <a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/399">Issue #399</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/380">PR #380</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/390">PR #390</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/392">PR #392</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/394">PR #394</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/396">PR #396</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/398">PR #398</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/400">PR #400</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/401">PR #401</a></p>
</div>

<div class="changelog-entry" markdown="1">
<p class="changelog-entry-title"><span class="changelog-tag changelog-tag-feature">Feature</span>Compatibilité PostgreSQL</p>
<p class="changelog-entry-desc">PostgreSQL est pris en charge au même titre que MySQL / MariaDB : migrations séparées par SGBD et SGBD déduit de <code>DATABASE_URL</code>, sans modification de code.</p>
<p class="changelog-entry-links"><a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/382">Issue #382</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/388">PR #388</a></p>
</div>

<div class="changelog-entry" markdown="1">
<p class="changelog-entry-title"><span class="changelog-tag changelog-tag-bug">Bug</span>Audit des pages</p>
<p class="changelog-entry-desc">Droits par propriétaire, URL unique par langue, landing page protégée, historique limité à 50 entrées, valeurs validées à la sauvegarde, repli de langue sur les slugs de l'API.</p>
<p class="changelog-entry-links"><a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/372">Issue #372</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/387">PR #387</a></p>
</div>

<div class="changelog-entry" markdown="1">
<p class="changelog-entry-title"><span class="changelog-tag changelog-tag-bug">Bug</span>Audit des FAQ</p>
<p class="changelog-entry-desc">Droits par propriétaire, suppression refusée pour une FAQ utilisée dans une page, identifiants des catégories et des questions conservés à la sauvegarde.</p>
<p class="changelog-entry-links"><a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/371">Issue #371</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/386">PR #386</a></p>
</div>

<div class="changelog-entry" markdown="1">
<p class="changelog-entry-title"><span class="changelog-tag changelog-tag-bug">Bug</span>Audit des commentaires</p>
<p class="changelog-entry-desc">Contenus des visiteurs échappés dans l'admin, texte stocké brut, statut validé, auteur et e-mail en lecture seule. L'API masque l'auteur des commentaires en attente.</p>
<p class="changelog-entry-links"><a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/370">Issue #370</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/385">PR #385</a></p>
</div>

<div class="changelog-entry" markdown="1">
<p class="changelog-entry-title"><span class="changelog-tag changelog-tag-bug">Bug</span>Audit des tags</p>
<p class="changelog-entry-desc">Auteur du tag et droits par propriétaire, suppression refusée pour un tag utilisé, libellé unique par langue, colonne Auteur et filtre « Moi / Tous » dans la liste.</p>
<p class="changelog-entry-links"><a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/369">Issue #369</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/381">PR #381</a></p>
</div>

<div class="changelog-entry" markdown="1">
<p class="changelog-entry-title"><span class="changelog-tag changelog-tag-bug">Bug</span>Audit des menus</p>
<p class="changelog-entry-desc">Supprimer un menu ne supprime plus les pages liées, menu par défaut protégé, liens externes validés, droits par propriétaire, éléments mis à jour par identifiant.</p>
<p class="changelog-entry-links"><a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/368">Issue #368</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/378">PR #378</a></p>
</div>

<div class="changelog-entry" markdown="1">
<p class="changelog-entry-title"><span class="changelog-tag changelog-tag-feature">Feature</span>Socle commun des contenus</p>
<p class="changelog-entry-desc">Droits sur les contenus (<code>ContentVoter</code>), protections communes des contrôleurs et remplissage des entités limité à une liste de champs autorisés.</p>
<p class="changelog-entry-links"><a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/367">Issue #367</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/376">PR #376</a></p>
</div>

<div class="changelog-entry" markdown="1">
<p class="changelog-entry-title"><span class="changelog-tag changelog-tag-feature">Feature</span>Templates d'issue et de PR</p>
<p class="changelog-entry-desc">Formulaires d'issue, template de PR, labels, workflows de contrôle et politique de sécurité sur GitHub.</p>
<p class="changelog-entry-links"><a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/365">Issue #365</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/366">PR #366</a></p>
</div>

## Octobre 2026

<div class="changelog-entry" markdown="1">
<p class="changelog-entry-title"><span class="changelog-tag changelog-tag-bug">Bug</span>Audit du tableau de bord</p>
<p class="changelog-entry-desc">Droits contrôlés côté serveur sur les blocs et les statistiques (statistiques réservées aux contributeurs, aide à la configuration réservée aux super-administrateurs), « Pages les plus vues » limité aux pages publiées ayant au moins une vue, liens de l'aide toujours affichés, libellé « Vues des pages », gestion des pages non traduites et des statuts inconnus.</p>
<p class="changelog-entry-links"><a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/363">Issue #363</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/364">PR #364</a></p>
</div>

<div class="changelog-entry" markdown="1">
<p class="changelog-entry-title"><span class="changelog-tag changelog-tag-feature">Feature</span><span class="changelog-tag changelog-tag-bug">Bug</span>Audit du front</p>
<p class="changelog-entry-desc">Thème Natheo-Horizon passé en TypeScript et nettoyé, nouvelle pagination, filtre IP de l'API désactivé par défaut, et nouvelle option système contenant le jeton API utilisé par le front (généré à l'installation avec le droit d'écriture, nécessaire aux commentaires).</p>
<p class="changelog-entry-links"><a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/218">Issue #218</a> <a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/361">Issue #361</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/362">PR #362</a></p>
</div>

<div class="changelog-entry" markdown="1">
<p class="changelog-entry-title"><span class="changelog-tag changelog-tag-bug">Bug</span>Audit de l'API</p>
<p class="changelog-entry-desc">Codes HTTP revus (400, 404, 405, 429) et messages d'erreur détaillés, nouvelle option « Ouvrir l'API ? », droit d'écriture du jeton exigé pour ajouter et modérer un commentaire, limitation du nombre de tentatives (authentification utilisateur, ajout de commentaire), slash final optionnel et pages non visibles filtrées.</p>
<p class="changelog-entry-links"><a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/359">Issue #359</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/360">PR #360</a></p>
</div>

<div class="changelog-entry" markdown="1">
<p class="changelog-entry-title"><span class="changelog-tag changelog-tag-bug">Bug</span>Audit de l'éditeur Markdown</p>
<p class="changelog-entry-desc">Liens internes vers les pages convertis uniquement en cible de lien, infobulles traduites, fenêtres propres à chaque éditeur d'une même page, mise en forme sur plusieurs lignes, annulation (Ctrl+Z), rendu Markdown unifié côté administration, passage en TypeScript.</p>
<p class="changelog-entry-links"><a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/357">Issue #357</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/358">PR #358</a></p>
</div>

<div class="changelog-entry" markdown="1">
<p class="changelog-entry-title"><span class="changelog-tag changelog-tag-bug">Bug</span>Audit de la gestion des jetons API</p>
<p class="changelog-entry-desc">Jetons stockés sous forme hachée en base (valeur affichée une seule fois à la création), corrections de l'écran de gestion et passage en TypeScript.</p>
<p class="changelog-entry-links"><a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/355">Issue #355</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/356">PR #356</a></p>
</div>

<div class="changelog-entry" markdown="1">
<p class="changelog-entry-title"><span class="changelog-tag changelog-tag-bug">Bug</span>Audit de la gestion des logs</p>
<p class="changelog-entry-desc">Corrections remontées par l'audit et passage en TypeScript.</p>
<p class="changelog-entry-links"><a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/353">Issue #353</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/354">PR #354</a></p>
</div>

<div class="changelog-entry" markdown="1">
<p class="changelog-entry-title"><span class="changelog-tag changelog-tag-bug">Bug</span>Audit de la gestion des emails</p>
<p class="changelog-entry-desc">Nouveau design des emails, correction de dépréciations et passage en TypeScript.</p>
<p class="changelog-entry-links"><a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/351">Issue #351</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/352">PR #352</a></p>
</div>

<div class="changelog-entry" markdown="1">
<p class="changelog-entry-title"><span class="changelog-tag changelog-tag-bug">Bug</span>Audit de la gestion des traductions</p>
<p class="changelog-entry-desc">Suppression des langues codées en dur et passage en TypeScript.</p>
<p class="changelog-entry-links"><a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/349">Issue #349</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/350">PR #350</a></p>
</div>

## Septembre 2026

<div class="changelog-entry" markdown="1">
<p class="changelog-entry-title"><span class="changelog-tag changelog-tag-bug">Bug</span>Audit de la sidebar</p>
<p class="changelog-entry-desc">Corrections remontées par l'audit de la gestion de la sidebar.</p>
<p class="changelog-entry-links"><a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/346">Issue #346</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/347">PR #347</a></p>
</div>

<div class="changelog-entry" markdown="1">
<p class="changelog-entry-title"><span class="changelog-tag changelog-tag-bug">Bug</span>Audit des options système</p>
<p class="changelog-entry-desc">Suppression de configurations inutilisées, renforcement de la sécurité et passage en TypeScript.</p>
<p class="changelog-entry-links"><a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/344">Issue #344</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/345">PR #345</a></p>
</div>

<div class="changelog-entry" markdown="1">
<p class="changelog-entry-title"><span class="changelog-tag changelog-tag-bug">Bug</span>Audit de la gestion des utilisateurs</p>
<p class="changelog-entry-desc">Correction d'une faille de sécurité et passage en TypeScript.</p>
<p class="changelog-entry-links"><a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/342">Issue #342</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/343">PR #343</a></p>
</div>

<div class="changelog-entry" markdown="1">
<p class="changelog-entry-title"><span class="changelog-tag changelog-tag-bug">Bug</span>Audit du gestionnaire de base de données</p>
<p class="changelog-entry-desc">Correction de failles de sécurité et des sauvegardes SQL qui ne pouvaient pas être restaurées, nouvelle notification, passage en TypeScript.</p>
<p class="changelog-entry-links"><a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/340">Issue #340</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/341">PR #341</a></p>
</div>

<div class="changelog-entry" markdown="1">
<p class="changelog-entry-title"><span class="changelog-tag changelog-tag-bug">Bug</span>Corrections des options avancées</p>
<p class="changelog-entry-desc">Corrections diverses et passage en TypeScript.</p>
<p class="changelog-entry-links"><a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/337">Issue #337</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/338">PR #338</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/339">PR #339</a></p>
</div>

<div class="changelog-entry" markdown="1">
<p class="changelog-entry-title"><span class="changelog-tag changelog-tag-bug">Bug</span>Corrections de la recherche globale</p>
<p class="changelog-entry-desc">Correction d'un ensemble de bugs, dont l'injection de caractères spéciaux, et passage en TypeScript.</p>
<p class="changelog-entry-links"><a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/335">Issue #335</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/336">PR #336</a></p>
</div>

<div class="changelog-entry" markdown="1">
<p class="changelog-entry-title"><span class="changelog-tag changelog-tag-bug">Bug</span>Corrections sur les pages</p>
<p class="changelog-entry-desc">Correction de la recherche de page et suppression de code inutilisé.</p>
<p class="changelog-entry-links"><a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/333">Issue #333</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/334">PR #334</a></p>
</div>

<div class="changelog-entry" markdown="1">
<p class="changelog-entry-title"><span class="changelog-tag changelog-tag-feature">Feature</span><span class="changelog-tag changelog-tag-bug">Bug</span>Nettoyage du code de la médiathèque</p>
<p class="changelog-entry-desc">Correction d'une faille de sécurité, contrôle des extensions de fichiers et corrections diverses.</p>
<p class="changelog-entry-links"><a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/330">Issue #330</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/331">PR #331</a></p>
</div>

<div class="changelog-entry" markdown="1">
<p class="changelog-entry-title"><span class="changelog-tag changelog-tag-feature">Feature</span>Nouveaux blocs sur le tableau de bord</p>
<p class="changelog-entry-desc">Ajout des blocs « Dernières pages » et « Pages les plus vues » (migration du code associé en TypeScript).</p>
<p class="changelog-entry-links"><a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/326">Issue #326</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/329">PR #329</a></p>
</div>

<div class="changelog-entry" markdown="1">
<p class="changelog-entry-title"><span class="changelog-tag changelog-tag-feature">Feature</span>Statistiques sur le tableau de bord</p>
<p class="changelog-entry-desc">Ajout de statistiques (nombre de pages, de commentaires, de vues, d'appels API) sur le tableau de bord, chargées en AJAX pour ne pas ralentir l'affichage.</p>
<p class="changelog-entry-links"><a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/249">Issue #249</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/325">PR #325</a></p>
</div>

## Août 2026

<div class="changelog-entry" markdown="1">
<p class="changelog-entry-title"><span class="changelog-tag changelog-tag-feature">Feature</span><span class="changelog-tag changelog-tag-bug">Bug</span>Refonte du module d'installation</p>
<p class="changelog-entry-desc">Refonte graphique du module d'installation et passage du JavaScript en TypeScript, blocage du lancement en environnement de production, prise en compte des migrations lors de l'installation, et correction de la prise en compte du schéma SQL par les migrations.</p>
<p class="changelog-entry-links"><a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/181">Issue #181</a> <a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/307">Issue #307</a> <a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/321">Issue #321</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/319">PR #319</a></p>
</div>

<div class="changelog-entry" markdown="1">
<p class="changelog-entry-title"><span class="changelog-tag changelog-tag-bug">Bug</span>Correction du Gestionnaire SQL</p>
<p class="changelog-entry-desc">Plusieurs erreurs JavaScript rendaient le Gestionnaire SQL inutilisable.</p>
<p class="changelog-entry-links"><a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/320">Issue #320</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/324">PR #324</a></p>
</div>

<div class="changelog-entry" markdown="1">
<p class="changelog-entry-title"><span class="changelog-tag changelog-tag-bug">Bug</span>Nettoyage de l'ancienne gestion des pages</p>
<p class="changelog-entry-desc">Suppression des anciens dossiers Vue (<code>Page_old</code>) laissés après la refonte de la gestion des pages.</p>
<p class="changelog-entry-links"><a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/312">Issue #312</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/313">PR #313</a></p>
</div>

## Juillet 2026

<div class="changelog-entry" markdown="1">
<p class="changelog-entry-title"><span class="changelog-tag changelog-tag-feature">Feature</span>Refonte de la gestion des pages</p>
<p class="changelog-entry-desc">Refonte graphique et technique de la gestion des pages (passage en TypeScript et Tailwind 4).</p>
<p class="changelog-entry-links"><a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/289">Issue #289</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/306">PR #306</a></p>
</div>

<div class="changelog-entry" markdown="1">
<p class="changelog-entry-title"><span class="changelog-tag changelog-tag-bug">Bug</span>Nettoyage à l'installation du CMS</p>
<p class="changelog-entry-desc">Les données temporaires générées par le CMS (<code>var/pageHistory</code>, <code>public/dump</code>) n'étaient pas purgées lors de l'installation.</p>
<p class="changelog-entry-links"><a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/216">Issue #216</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/305">PR #305</a></p>
</div>

<div class="changelog-entry" markdown="1">
<p class="changelog-entry-title"><span class="changelog-tag changelog-tag-bug">Bug</span>Harmonisation du bloc d'aide à la configuration</p>
<p class="changelog-entry-desc">Normalisation de l'en-tête du bloc « aide à la configuration », jusque-là différent des autres blocs du back-office.</p>
<p class="changelog-entry-links"><a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/303">Issue #303</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/304">PR #304</a></p>
</div>

<div class="changelog-entry" markdown="1">
<p class="changelog-entry-title"><span class="changelog-tag changelog-tag-feature">Feature</span>Passage des clés d'options système en enum</p>
<p class="changelog-entry-desc">Conversion des constantes des options système en enum PHP.</p>
<p class="changelog-entry-links"><a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/90">Issue #90</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/302">PR #302</a></p>
</div>

<div class="changelog-entry" markdown="1">
<p class="changelog-entry-title"><span class="changelog-tag changelog-tag-bug">Bug</span>Correction d'une erreur JS à la création d'un menu</p>
<p class="changelog-entry-desc">Correction d'une erreur Vue (prop <code>id</code> invalide) sur la page de création d'un menu.</p>
<p class="changelog-entry-links"><a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/300">Issue #300</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/301">PR #301</a></p>
</div>

<div class="changelog-entry" markdown="1">
<p class="changelog-entry-title"><span class="changelog-tag changelog-tag-bug">Bug</span>Harmonisation des interfaces du back-office</p>
<p class="changelog-entry-desc">Harmonisation des titres de blocs entre les différentes pages d'édition du back-office.</p>
<p class="changelog-entry-links"><a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/279">Issue #279</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/297">PR #297</a></p>
</div>

## Juin 2026

<div class="changelog-entry" markdown="1">
<p class="changelog-entry-title"><span class="changelog-tag changelog-tag-bug">Bug</span>Correction de plusieurs avertissements JS</p>
<p class="changelog-entry-desc">Corrections de divers avertissements JavaScript (éditeur d'email, listing des jetons API, gestionnaire de base de données).</p>
<p class="changelog-entry-links"><a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/295">Issue #295</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/296">PR #296</a></p>
</div>

<div class="changelog-entry" markdown="1">
<p class="changelog-entry-title"><span class="changelog-tag changelog-tag-feature">Feature</span>Nouvelle commande console pour les tests unitaires</p>
<p class="changelog-entry-desc">Ajout de la commande <code>natheo:install-test-unit</code> pour créer ou mettre à jour la base de données de test.</p>
<p class="changelog-entry-links"><a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/266">Issue #266</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/283">PR #283</a></p>
</div>

<div class="changelog-entry" markdown="1">
<p class="changelog-entry-title"><span class="changelog-tag changelog-tag-feature">Feature</span><span class="changelog-tag changelog-tag-bug">Bug</span>Refonte de la gestion des menus</p>
<p class="changelog-entry-desc">Refonte graphique de la gestion des menus avec le nouveau design du back-office, conversion des positions de menu en enum, nettoyage du code obsolète, et corrections de l'ordre des éléments d'un menu et de la réinitialisation des menus par défaut.</p>
<p class="changelog-entry-links"><a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/126">Issue #126</a> <a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/247">Issue #247</a> <a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/251">Issue #251</a> <a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/277">Issue #277</a> <a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/280">Issue #280</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/278">PR #278</a></p>
</div>

<div class="changelog-entry" markdown="1">
<p class="changelog-entry-title"><span class="changelog-tag changelog-tag-bug">Bug</span>Contrôle des identifiants en mode édition</p>
<p class="changelog-entry-desc">En mode édition, un identifiant inexistant n'était pas détecté ; un message d'erreur est maintenant affiché (comme c'était déjà le cas pour les FAQ).</p>
<p class="changelog-entry-links"><a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/281">Issue #281</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/285">PR #285</a></p>
</div>

## Mai 2026

<div class="changelog-entry" markdown="1">
<p class="changelog-entry-title"><span class="changelog-tag changelog-tag-feature">Feature</span>CLI d'installation du CMS sur un serveur</p>
<p class="changelog-entry-desc">Travaux autour d'un outil en ligne de commande pour installer facilement le CMS sur un serveur (choix de la version du CMS, gestion des versions de PHP/Vue/Composer...).</p>
<p class="changelog-entry-links"><a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/187">Issue #187</a></p>
</div>

<div class="changelog-entry" markdown="1">
<p class="changelog-entry-title"><span class="changelog-tag changelog-tag-bug">Bug</span>Respect des droits sur le tableau de bord</p>
<p class="changelog-entry-desc">Les blocs « derniers commentaires » et « pages les plus vues » du tableau de bord ne s'affichent maintenant que pour les utilisateurs ayant les droits nécessaires (rôle contributeur ou plus).</p>
<p class="changelog-entry-links"><a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/184">Issue #184</a> <a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/275">Issue #275</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/276">PR #276</a></p>
</div>

<div class="changelog-entry" markdown="1">
<p class="changelog-entry-title"><span class="changelog-tag changelog-tag-bug">Bug</span>Masquage du bouton « Nouveau » selon les droits</p>
<p class="changelog-entry-desc">Le bouton « Nouveau + » et les liens qu'il propose sont maintenant masqués pour les utilisateurs qui n'ont pas les droits d'accès correspondants.</p>
<p class="changelog-entry-links"><a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/194">Issue #194</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/274">PR #274</a></p>
</div>

<div class="changelog-entry" markdown="1">
<p class="changelog-entry-title"><span class="changelog-tag changelog-tag-bug">Bug</span>Correction d'une regex de configuration</p>
<p class="changelog-entry-desc">Correction de l'expression régulière utilisée pour retrouver une clé dans le fichier de configuration.</p>
<p class="changelog-entry-links"><a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/272">Issue #272</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/273">PR #273</a></p>
</div>

<div class="changelog-entry" markdown="1">
<p class="changelog-entry-title"><span class="changelog-tag changelog-tag-bug">Bug</span>Correction de la purge des notifications</p>
<p class="changelog-entry-desc">Correction de la requête MariaDB utilisée pour purger les anciennes notifications déjà lues.</p>
<p class="changelog-entry-links"><a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/270">Issue #270</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/271">PR #271</a></p>
</div>

## Avril 2026

<div class="changelog-entry" markdown="1">
<p class="changelog-entry-title"><span class="changelog-tag changelog-tag-feature">Feature</span>Refonte de la gestion des commentaires</p>
<p class="changelog-entry-desc">Refonte de la gestion des commentaires avec le nouveau design du back-office, et conversion de leurs statuts en enum PHP.</p>
<p class="changelog-entry-links"><a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/75">Issue #75</a> <a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/241">Issue #241</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/245">PR #245</a></p>
</div>

<div class="changelog-entry" markdown="1">
<p class="changelog-entry-title"><span class="changelog-tag changelog-tag-feature">Feature</span>Retour des API via des DTO</p>
<p class="changelog-entry-desc">Remplacement des entités Doctrine par des DTO en retour des API, pour améliorer les performances.</p>
<p class="changelog-entry-links"><a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/78">Issue #78</a></p>
</div>

<div class="changelog-entry" markdown="1">
<p class="changelog-entry-title"><span class="changelog-tag changelog-tag-bug">Bug</span>Correction des icônes de la sidebar</p>
<p class="changelog-entry-desc">Les icônes n'apparaissaient plus dans le listing de la sidebar d'administration.</p>
<p class="changelog-entry-links"><a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/242">Issue #242</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/244">PR #244</a></p>
</div>

<div class="changelog-entry" markdown="1">
<p class="changelog-entry-title"><span class="changelog-tag changelog-tag-feature">Feature</span>Refonte de la médiathèque</p>
<p class="changelog-entry-desc">Refonte de la médiathèque avec les nouveaux standards du CMS, et suppression des méthodes dépréciées associées.</p>
<p class="changelog-entry-links"><a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/234">Issue #234</a> <a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/235">Issue #235</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/237">PR #237</a></p>
</div>

## Mars 2026

<div class="changelog-entry" markdown="1">
<p class="changelog-entry-title"><span class="changelog-tag changelog-tag-feature">Feature</span><span class="changelog-tag changelog-tag-bug">Bug</span>Refonte de la FAQ</p>
<p class="changelog-entry-desc">Refonte de la FAQ avec le nouveau design du back-office : correction de l'affichage des FAQ désactivées côté front, de la génération des liens internes et des liens custom, et nettoyage du code devenu obsolète.</p>
<p class="changelog-entry-links"><a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/226">Issue #226</a> <a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/227">Issue #227</a> <a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/228">Issue #228</a> <a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/229">Issue #229</a> <a class="changelog-badge changelog-badge-issue" href="https://github.com/counteraccro/natheo/issues/230">Issue #230</a> <a class="changelog-badge changelog-badge-pr" href="https://github.com/counteraccro/natheo/pull/231">PR #231</a></p>
</div>
