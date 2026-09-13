---
title: "Références globales"
parent: "Architecture"
nav_order: 5
---

Certains champs renvoyés par l'[API publique](../API/index.md) (ou attendus
en paramètre) sont des valeurs numériques ou des mots-clés plutôt que du
texte libre : le rendu d'une page, le type d'un menu, la position d'un
bloc... Cette page fait le lien entre ces valeurs et l'**enum PHP** qui les
définit réellement dans le code, pour que la correspondance ne repose pas
sur une liste recopiée à la main (et donc potentiellement désynchronisée du
code).

## Rendu de la page

Défini par la clé `render` dans la réponse de [Find page](../API/References/find_page.md).
Enum [`PageRender`](https://github.com/counteraccro/natheo/blob/master/src/Enum/Admin/Content/Page/PageRender.php) (`int`) :

| Valeur | Cas PHP | Rendu |
|---|---|---|
| 1 | `ONE_BLOCK` | 1 bloc |
| 2 | `TWO_BLOCK` | 2 blocs côte à côte |
| 3 | `THREE_BLOCK` | 3 blocs côte à côte |
| 4 | `TWO_BLOCK_BOTTOM` | 2 blocs, l'un en dessous de l'autre |
| 5 | `THREE_BLOCK_BOTTOM` | 3 blocs, l'un en dessous de l'autre |
| 6 | `ONE_TWO_BLOCK` | 1 bloc au-dessus, 2 blocs côte à côte en dessous |
| 7 | `TWO_ONE_BLOCK` | 2 blocs côte à côte au-dessus, 1 bloc en dessous |
| 8 | `TWO_TWO_BLOCK` | 2 blocs côte à côte au-dessus, 2 blocs côte à côte en dessous |

## Type de contenu de page

Défini par la clé `type` de chaque élément du bloc `contents`, dans la même
réponse [Find page](../API/References/find_page.md).
Enum [`PageContentType`](https://github.com/counteraccro/natheo/blob/master/src/Enum/Admin/Content/Page/PageContentType.php) (`int`) :

| Valeur | Cas PHP | Type de contenu |
|---|---|---|
| 1 | `TEXT` | Texte (éditeur Markdown) |
| 2 | `FAQ` | Bloc FAQ |
| 3 | `LISTING` | Listing (d'articles, de pages...) |

## Catégorie de page

Enum [`PageCategory`](https://github.com/counteraccro/natheo/blob/master/src/Enum/Admin/Content/Page/PageCategory.php) (`int`) :

| Valeur | Cas PHP | Catégorie |
|---|---|---|
| 1 | `PAGE` | Page |
| 2 | `ARTICLE` | Article |
| 3 | `PROJET` | Projet |
| 4 | `BLOG` | Blog |
| 5 | `EVENEMENT` | Évènement |
| 6 | `NEWS` | News |
| 7 | `EVOLUTION` | Évolution |
| 8 | `DOCUMENTATION` | Documentation |
| 9 | `FAQ` | FAQ |

> 💡 Contrairement aux autres valeurs de cette page, le paramètre `category`
> de [Listing des pages par catégorie](../API/References/listing_pages_category.md)
> ne prend pas l'identifiant numérique ci-dessus, mais le **libellé traduit**
> de la catégorie (ex. `blog`), comparé sans tenir compte de la casse au
> texte du domaine de traduction `page` (clé `page.category.*`).

## Statistiques de page

Défini dans le bloc `statistiques` de [Find page](../API/References/find_page.md).
Enum [`PageStatistics`](https://github.com/counteraccro/natheo/blob/master/src/Enum/Admin/Content/Page/PageStatistics.php) (`string`) :

| Valeur | Cas PHP | Statistique |
|---|---|---|
| `PAGE_NB_VISITEUR` | `NB_VISITEUR` | Nombre de visiteurs uniques |
| `PAGE_NB_READ` | `NB_READ` | Nombre de lectures |

## Position d'un menu

Défini par la clé `position` dans [Find page](../API/References/find_page.md)
(bloc `menus`) et [Find menu](../API/References/find_menu.md).
Enum [`MenuPosition`](https://github.com/counteraccro/natheo/blob/master/src/Enum/Admin/Content/Menu/MenuPosition.php) (`int`) :

| Valeur | Cas PHP | Position |
|---|---|---|
| 1 | `POSITION_HEADER` | En-tête |
| 2 | `POSITION_RIGHT` | Droite |
| 3 | `POSITION_FOOTER` | Pied de page |
| 4 | `POSITION_LEFT` | Gauche |

> 💡 Dans les réponses de l'API, ce champ n'apparaît pas sous forme
> numérique mais sous forme de mot-clé (`"HEADER"`, `"LEFT"`...) : c'est le
> nom que renvoie `MenuPosition::getStringByPosition()`.

## Type de menu

Défini par la clé `type` dans [Find page](../API/References/find_page.md)
(bloc `menus`) et [Find menu](../API/References/find_menu.md).
Enum [`MenuType`](https://github.com/counteraccro/natheo/blob/master/src/Enum/Admin/Content/Menu/MenuType.php) (`int`) :

| Valeur | Cas PHP | Type de menu |
|---|---|---|
| 1 | `HEADER_SIDE_BAR` | Header, side-bar |
| 2 | `HEADER_MENU_DEROULANT` | Header, menu déroulant |
| 3 | `HEADER_MENU_DEROULANT_BIG_MENU` | Header, menu déroulant, big menu |
| 4 | `HEADER_MENU_DEROULANT_BIG_MENU_2_COLONNES` | Header, big menu 2 colonnes |
| 5 | `HEADER_MENU_DEROULANT_BIG_MENU_3_COLONNES` | Header, big menu 3 colonnes |
| 6 | `HEADER_MENU_DEROULANT_BIG_MENU_4_COLONNES` | Header, big menu 4 colonnes |
| 11 | `LEFT_RIGHT_SIDE_BAR` | Gauche/droite, side-bar |
| 12 | `LEFT_RIGHT_SIDE_BAR_ACCORDEON` | Gauche/droite, side-bar accordéon |
| 16 | `FOOTER_COLONNES` | Footer, 4 colonnes |
| 17 | `FOOTER_1_ROW_RIGHT` | Footer, 1 ligne à droite |
| 18 | `FOOTER_1_ROW_CENTER` | Footer, 1 ligne centrée |

> 💡 Les valeurs ne se suivent pas : chaque position de menu réserve une
> plage de valeurs (1-6 pour le header, 11-12 pour gauche/droite, 16-18 pour
> le footer), pour pouvoir ajouter de nouveaux types sans jamais faire
> chevaucher les plages entre positions.

## Cible d'un lien de menu

Défini par la clé `target` de chaque élément d'un menu, dans
[Find page](../API/References/find_page.md) et [Find menu](../API/References/find_menu.md).
Enum [`MenuLinkTarget`](https://github.com/counteraccro/natheo/blob/master/src/Enum/Admin/Content/Menu/MenuLinkTarget.php) (`string`) :

| Valeur | Cas PHP | Comportement |
|---|---|---|
| `_blank` | `LINK_TARGET_BLANK` | Ouvre le lien dans un nouvel onglet |
| `_self` | `LINK_TARGET_SELF` | Ouvre le lien dans l'onglet courant |

## Voir aussi
- [Find page](../API/References/find_page.md)
- [Find menu](../API/References/find_menu.md)
- [Listing des pages par catégorie](../API/References/listing_pages_category.md)
