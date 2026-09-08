---
title: "Modèle de données"
parent: "Architecture"
nav_order: 9
---

Vue d'ensemble des tables de la base de données et des relations entre elles,
générée à partir des entités Doctrine (`src/Entity/`). Chaque domaine
fonctionnel du CMS possède son propre jeu de tables, quasi systématiquement
accompagné d'une table `*_translation` pour le contenu multilingue (voir
[Traductions & multilingue](traductions_i18n.md)).

Les diagrammes ci-dessous utilisent la notation entité-association standard :
`||--o{` pour une relation 1—N, `}o--o{` pour une relation N—N (table de
jointure), et une entité qui pointe sur elle-même pour une arborescence
(parent / enfants).

<script src="https://cdnjs.cloudflare.com/ajax/libs/mermaid/10.9.1/mermaid.min.js"></script>
<script>mermaid.initialize({ startOnLoad: true, theme: 'neutral' });</script>

## Pages

La table `page` est le cœur du contenu du site : chaque page a un auteur, des
traductions, des blocs de contenu (eux-mêmes traduits et ordonnables), des
métadonnées SEO et des statistiques de consultation.

<pre class="mermaid">
erDiagram
    USER ||--o{ PAGE : "auteur"
    PAGE ||--o{ PAGE_TRANSLATION : "traductions"
    PAGE ||--o{ PAGE_CONTENT : "blocs de contenu"
    PAGE_CONTENT ||--o{ PAGE_CONTENT_TRANSLATION : "traductions"
    PAGE ||--o{ PAGE_META : "métadonnées SEO"
    PAGE_META ||--o{ PAGE_META_TRANSLATION : "traductions"
    PAGE ||--o{ PAGE_STATISTIQUE : "statistiques"
</pre>

`page.render` et `page.category` sont des entiers référencés dans les
[références globales](references_globales.md) (mode d'affichage, type de
page...). `page.disabled`, `page.landing_page` et `page.is_open_comment`
pilotent respectivement la visibilité, la page d'accueil et l'ouverture des
commentaires.

## Tags, menus & commentaires

Ces trois domaines gravitent autour de `page`, avec deux tables de jointure
N—N (`tag` ↔ `page` et `menu` ↔ `page`).

<pre class="mermaid">
erDiagram
    PAGE }o--o{ TAG : "page_tag"
    TAG ||--o{ TAG_TRANSLATION : "traductions"

    USER ||--o{ MENU : "auteur"
    PAGE }o--o{ MENU : "menu_page"
    MENU ||--o{ MENU_ELEMENT : "éléments"
    MENU_ELEMENT ||--o{ MENU_ELEMENT : "enfants"
    MENU_ELEMENT }o--o| PAGE : "cible (optionnel)"
    MENU_ELEMENT ||--o{ MENU_ELEMENT_TRANSLATION : "traductions"

    PAGE ||--o{ COMMENT : "commentaires"
    USER ||--o{ COMMENT : "modérateur (optionnel)"
</pre>

`menu_element` est une arborescence auto-référencée (colonnes `parent_id`,
triée par `column_position`/`row_position`) : c'est elle qui construit les
menus déroulants à plusieurs niveaux. Un élément de menu peut pointer vers une
`page` (lien interne) ou rester une simple entrée de navigation.

## FAQ

<pre class="mermaid">
erDiagram
    USER ||--o{ FAQ : "auteur"
    FAQ ||--o{ FAQ_TRANSLATION : "traductions"
    FAQ ||--o{ FAQ_CATEGORY : "catégories"
    FAQ_CATEGORY ||--o{ FAQ_CATEGORY_TRANSLATION : "traductions"
    FAQ_CATEGORY ||--o{ FAQ_QUESTION : "questions"
    FAQ_QUESTION ||--o{ FAQ_QUESTION_TRANSLATION : "traductions"
    FAQ ||--o{ FAQ_STATISTIQUE : "statistiques"
</pre>

## Médiathèque

<pre class="mermaid">
erDiagram
    MEDIA_FOLDER ||--o{ MEDIA_FOLDER : "sous-dossiers"
    MEDIA_FOLDER ||--o{ MEDIA : "médias"
    USER ||--o{ MEDIA : "propriétaire"
</pre>

## Système

<pre class="mermaid">
erDiagram
    USER ||--o{ OPTION_USER : "préférences"
    USER ||--o{ USER_DATA : "données complémentaires"
    USER ||--o{ NOTIFICATION : "notifications"
    USER ||--o{ SQL_MANAGER : "requêtes exécutées"

    SIDEBAR_ELEMENT ||--o{ SIDEBAR_ELEMENT : "enfants"

    MAIL ||--o{ MAIL_TRANSLATION : "traductions"
</pre>

`option_system` et `api_token` sont des tables indépendantes (clé/valeur pour
la première, jetons d'accès API pour la seconde) : elles ne référencent aucune
autre table.

> 🚧 Cette page liste les relations structurelles ; le détail de chaque
> colonne (types, contraintes, index) pourra être ajouté plus tard si besoin,
> par exemple via `php bin/console doctrine:mapping:info` ou un export du
> schéma.
