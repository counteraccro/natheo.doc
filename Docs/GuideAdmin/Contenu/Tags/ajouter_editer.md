---
title: "Ajouter / Éditer un tag"
parent: "Tags"
grand_parent: "Guide d'administration"
nav_order: 1
---

Ce formulaire permet de créer un nouveau tag ou de modifier un tag existant,
depuis le [listing des tags](listing.md).

Il est accessible aux contributeurs, administrateurs et super-administrateurs,
aux URLs `/admin/{locale}/tag/add/` (création) et `/admin/{locale}/tag/update/{id}` (édition).

![Création d'un tag](files/creation.png)

Un tag se compose de deux choses toutes simples :

- **Une couleur**, à choisir librement (au format `#rrggbb`) — un lien vous
  propose des exemples si vous manquez d'inspiration
- **Un libellé**, obligatoire dans chacune des langues du site (255 caractères
  maximum). Il doit être unique dans sa langue : un libellé déjà utilisé par un
  autre tag est refusé. Pour aller plus vite, une case à cocher permet de
  dupliquer automatiquement le libellé saisi vers les autres langues

Au fur et à mesure de la saisie, un aperçu vous montre à quoi ressemblera le
tag une fois publié.

En édition, un petit bloc en bas de page rappelle depuis quand le tag existe
et quand il a été modifié pour la dernière fois.

![Édition d'un tag](files/edition.png)

Une fois satisfait, il ne reste plus qu'à valider avec le bouton **Créer le
tag** (ou **Modifier**), ou à cliquer sur **Retour** pour revenir à la page
précédente sans rien changer. En édition, un bouton **Supprimer** est également
disponible si vous en avez le droit — une confirmation vous sera demandée avant
toute suppression, et celle-ci est refusée si le tag est utilisé par au moins
une page.

> 💡 Modifier un tag n'a aucun impact sur le contenu des pages qui lui sont
> associées : seul le tag lui-même change.

## Voir aussi
- [Listing des tags](listing.md)
- [Référence technique](technique.md) *(tables, services, routes)*
