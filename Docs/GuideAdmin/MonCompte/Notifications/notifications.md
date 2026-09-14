---
title: "Notifications"
nav_icon: "M14.857 17.082a23.848 23.848 0 0 0 5.454-1.31A8.967 8.967 0 0 1 18 9.75V9A6 6 0 0 0 6 9v.75a8.967 8.967 0 0 1-2.312 6.022c1.733.64 3.56 1.085 5.455 1.31m5.714 0a24.255 24.255 0 0 1-5.714 0m5.714 0a3 3 0 1 1-5.714 0"
parent: "Guide d'administration"
has_children: true
nav_order: 2
---

Le CMS peut vous prévenir de certains événements survenant sur le site (nouveau
commentaire, actions sur les comptes, sauvegarde disponible…) via un centre de
notifications personnel, propre à chaque utilisateur connecté.

## Accès

Une icône en forme de cloche, dans l'en-tête de l'administration, ouvre le
centre de notifications. Un point rouge s'affiche dessus dès qu'il existe au
moins une notification non lue.

Cette icône — et plus généralement toute la fonctionnalité — n'apparaît que si
l'option **Notifications** est activée dans les [options système](../../Systeme/options.md).
Si elle est désactivée, aucune notification n'est générée et l'icône disparaît
de l'en-tête.

Le centre de notifications est accessible à tous les utilisateurs connectés, à
l'URL `/admin/{locale}/notification/`.

## Le centre de notifications

En haut de la page, trois compteurs résument la situation : le nombre de
notifications **non lues**, celles reçues **aujourd'hui**, et le **total**.

Juste en dessous, un message rappelle que les notifications déjà lues sont
automatiquement supprimées au bout d'un certain nombre de jours (réglable
également depuis les [options système](../../Systeme/options.md)) — cette
purge s'exécute discrètement à chaque ouverture de la page.

Les notifications sont ensuite réparties dans des onglets :

- **Toutes** — l'ensemble de vos notifications
- **Non lu** — uniquement celles pas encore lues, avec leur nombre en badge
- un onglet par catégorie (**Commentaire**, **Administration**, **Dump**…)

Pour chaque notification affichée, vous retrouvez :

- un titre et un message, qui peuvent contenir un lien (par exemple **Voir le
  commentaire** pour une notification liée à un commentaire)
- la date d'émission, sous forme relative (« il y a 3 heures »)
- un point de couleur et un fond légèrement teinté tant qu'elle n'est pas lue
- un lien **Marquer comme lu**, qui disparaît une fois la notification lue
- un bouton 🗑️ pour la supprimer individuellement (une confirmation est
  demandée)

## Actions groupées

Dans l'onglet **Toutes**, une case à cocher sur chaque ligne (et une case
« tout cocher » dans l'en-tête) permet de sélectionner plusieurs notifications
à la fois. Une fois une sélection faite, des boutons apparaissent pour :

- les marquer comme **lues** (ou **non lues**, si toute la sélection est déjà
  lue)
- les **supprimer** — une confirmation rappelle le nombre total de
  notifications concernées, et combien étaient non lues

Le bouton **Marquer tout comme lu**, en haut de page, passe en une fois toutes
les notifications non lues de l'utilisateur en lu ; il est désactivé s'il n'y
a rien à lire.

## Quels événements génèrent une notification ?

| Événement | Destinataire |
|---|---|
| Bienvenue, à la création d'un compte | Le nouvel utilisateur |
| Un utilisateur désactive lui-même son compte | Les super-administrateurs |
| Un utilisateur supprime ou anonymise lui-même son compte | Les super-administrateurs |
| Un nouveau commentaire est déposé sur une page | L'auteur (propriétaire) de la page |
| Un dump SQL demandé est disponible | L'utilisateur ayant lancé la sauvegarde |
| Fin de l'installation du CMS | Le compte fondateur |

> 💡 Cette liste dépend uniquement du code du CMS : elle n'est pas
> paramétrable depuis l'administration, en dehors de l'activation générale de
> la fonctionnalité.

## Voir aussi
- [Référence technique](technique.md)
- [Options système](../../Systeme/options.md)
