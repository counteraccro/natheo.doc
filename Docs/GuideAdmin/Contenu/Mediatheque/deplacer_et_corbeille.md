---
title: "Déplacer et mettre à la corbeille"
parent: "La médiathèque"
grand_parent: "Guide d'administration"
nav_order: 3
---

Ce document couvre le déplacement d'un dossier ou d'un média, ainsi que la
corbeille de la [médiathèque](mediatheque.md).

## Déplacer un média ou un dossier

Le lien **Déplacer** du menu **…** ouvre un panneau listant l'arborescence
complète des dossiers de la médiathèque.

![Déplacer un média ou un dossier](files/deplacer.png)

La recherche en haut du panneau filtre la liste par nom de dossier. Cliquer
sur un dossier le sélectionne (ligne surlignée) ; **root** représente la
racine de la médiathèque. Le bouton **Déplacer** ne devient actif qu'une fois
un dossier sélectionné, et valide le déplacement.

Lorsqu'il s'agit d'un dossier, ses propres sous-dossiers ne sont pas proposés
comme destination (impossible de déplacer un dossier dans lui-même ou dans
l'un de ses enfants). Le déplacement met à jour le chemin de l'élément — et,
pour un dossier, de tout son contenu en cascade — en base de données, ainsi
que sur le disque si la création physique des dossiers est activée.

## La corbeille

Mettre un élément à la corbeille (lien **Corbeille** du menu **…** en vue
grille, ou icône 🗑️ directement dans la ligne en vue liste — voir
[la médiathèque](mediatheque.md#actions-sur-un-élément) pour le détail des
deux affichages) ne le supprime pas immédiatement : il reste stocké et
physiquement présent sur le disque, mais disparaît du navigateur de la
médiathèque. Mettre un **dossier** à la corbeille y pousse aussi, en cascade,
tous les dossiers et médias qu'il contient — chacun est individuellement
marqué comme étant dans la corbeille, pas seulement le dossier lui-même.
Cette action n'est proposée que si l'option système *Autoriser la suppression
des données* (`OS_ALLOW_DELETE_DATA`) est activée (voir
[la médiathèque](mediatheque.md#actions-sur-un-élément)).

Le badge sur l'icône 🗑️ de la barre d'outils indique le nombre total
d'éléments actuellement dans la corbeille (dossiers et médias confondus). Y
cliquer ouvre la vue corbeille :

![La corbeille](files/corbeille.png)

> Comme le marquage « à la corbeille » est propagé à chaque dossier et média
> descendant, mettre un dossier contenant plusieurs éléments à la corbeille
> fait grimper le badge d'autant (pas seulement de 1), et la vue corbeille
> liste **chaque élément individuellement** — le dossier trashé et tout son
> contenu apparaissent à plat, côte à côte, pas regroupés sous le dossier.
> **Restaurer** le dossier parent, même depuis cette vue, restaure aussi tous
> ses descendants (la restauration passe par le même mécanisme de cascade que
> la mise à la corbeille). **Supprimer** définitivement le dossier parent
> supprime également tous ses descendants (base de données et fichiers sur le
> disque), qui disparaissent alors de la vue corbeille même s'ils n'ont pas
> été sélectionnés individuellement.

Depuis le menu **…** d'un élément de la corbeille, deux actions sont
proposées :

| Action | Effet |
|---|---|
| ↩️ **Restaurer** | Retire l'élément de la corbeille ; il réapparaît dans son dossier d'origine |
| 🗑️ **Supprimer** | Ouvre une confirmation inline sur la vignette ; une fois confirmée, supprime l'élément **définitivement**, y compris le fichier physique sur le disque |

> Le bouton 🗑️ **Supprimer** n'apparaît dans le menu **…** de la corbeille que
> si l'option système *Autoriser la suppression des données*
> (`OS_ALLOW_DELETE_DATA`) est activée — sinon, seule la **Restauration**
> reste possible depuis cette vue. Le serveur applique en plus sa propre
> garde-fou indépendante de cette option : la route de suppression définitive
> refuse toute tentative sur un élément qui n'est pas déjà passé par la
> corbeille (`trash` à `true`), même en appelant la route directement.

## Voir aussi
- [Ajouter, informer et modifier un média](medias.md)
- [Créer et renommer un dossier](dossiers.md)
- [Référence technique](technique.md)
