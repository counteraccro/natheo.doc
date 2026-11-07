---
title: "Menus"
nav_icon: "M5 7h14M5 12h14M5 17h14"
parent: "Guide d'administration"
has_children: true
nav_order: 8
---

Les menus définissent la navigation de votre site : en-tête, pied de page, ou
menus latéraux gauche/droite. Chaque position peut avoir un menu **par
défaut** (voir [Ajouter / Éditer un menu](ajouter_editer.md#le-menu-par-défaut)).

Cette page est accessible aux contributeurs, administrateurs et
super-administrateurs, à l'URL `/admin/{locale}/menu/`.

![Listing des menus](files/listing.png)

Le tableau ([composant Grid](../../../Architecture/composants/grid.md)) liste
tous les menus existants, avec leur position/type combinés, leur statut
« affichage par défaut », leurs dates de création et de modification, et leur
auteur. Vous pouvez trier chaque colonne triable en cliquant sur son en-tête,
et retrouver rapidement un menu grâce à la barre de recherche (sur le nom).

Si une position (haut de page, pied de page ou menu de gauche) n'a **aucun**
menu par défaut, ou en a **plusieurs**, une alerte s'affiche en haut de page
pour le signaler — voir [Le menu par défaut](ajouter_editer.md#le-menu-par-défaut).
Cette alerte ne se rafraîchit qu'au chargement suivant de la page. La position
*À droite* n'est volontairement pas concernée par cette vérification : elle
n'a pas besoin d'un menu par défaut.

Pour chaque menu, les actions suivantes sont disponibles depuis la ligne :

| Action | Effet |
|---|---|
| 👁️ **Activer / Désactiver** | Un menu désactivé n'apparaît plus sur le site, mais rien n'est supprimé ; une confirmation est demandée uniquement pour la désactivation. N'apparaît pas pour le menu par défaut |
| 🗑️ **Supprimer** | Retire définitivement le menu (irréversible) — n'apparaît que si l'option système *Autoriser la suppression des données* est activée, et jamais pour le menu par défaut |
| ✏️ **Éditer** | Ouvre le menu en édition |
| 🟩 **Définir par défaut** | N'apparaît que si le menu est actif et n'est pas déjà par défaut ; demande confirmation, puis retire automatiquement le statut par défaut de tout autre menu de la **même position** |

Une icône ☰ à côté du numéro `#` d'un menu indique qu'il s'agit du menu par
défaut de sa position.

Quelques règles protègent le site :

- Le **menu par défaut** ne peut être ni désactivé ni supprimé : définissez
  d'abord un autre menu par défaut pour cette position.
- Un menu **utilisé par au moins une page** ne peut pas être supprimé : le
  message d'erreur indique le nombre de pages concernées. Supprimer un menu
  ne supprime donc jamais de page.
- Un **Contributeur** ne dispose de ces actions que sur ses propres menus (voir
  [les droits sur les contenus](../../Systeme/roles.md#les-droits-sur-les-contenus)).

Et en haut de page, le bouton **Nouveau menu** ouvre le formulaire de
création.

## Voir aussi
- [Ajouter / Éditer un menu](ajouter_editer.md)
- [Référence technique](technique.md) *(tables, services, routes)*
