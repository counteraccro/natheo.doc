---
title: "Find menu"
parent: "API"
nav_order: 3
---

Renvoie un menu formaté, désigné soit par son **id**, soit par le **slug d'une page** à laquelle il est rattaché
(avec sa position). Seuls les menus **actifs** sont renvoyés, et seuls leurs éléments actifs y figurent.

Pour récupérer d'un coup tous les menus d'une page, préférez [Find page](find_page.md), qui les renvoie avec la page
et complète avec les menus par défaut.

Paramètres attendus (query string) :

| nom        | type    | obligatoire | valeur par défaut | commentaire                                              |
|------------|---------|-------------|-------------------|----------------------------------------------------------|
| id         | Integer | OUI*        |                   | Id du menu. *Obligatoire si `page_slug` est absent       |
| page_slug  | String  | OUI*        |                   | Slug d'une page. *Obligatoire si `id` est absent         |
| position   | Integer | NON         | 1                 | Position du menu recherché, avec `page_slug` seulement : `1` en-tête, `2` droite, `3` pied de page, `4` gauche |
| locale     | String  | NON         | fr                | Langue des libellés et des slugs renvoyés                |

`id` et `page_slug` sont exclusifs : il en faut exactement un.

Avec `page_slug`, l'API cherche, parmi les menus **rattachés** à la page, celui de la `position` demandée. Les
menus par défaut ne sont pas pris en compte ici : pour obtenir les menus d'une page complétés par les menus par
défaut, utilisez [Find page](find_page.md).

> 📝 Le `User-Token` est vérifié s'il est envoyé (un token invalide renvoie `403` « Utilisateur non trouvé »), mais
> il **ne change pas le résultat** : un menu désactivé n'est jamais renvoyé, même à un utilisateur connecté.

**Requêtes CURL**
`````shell
curl --request GET \
--url '[url-de-mon-site]/api/v1/menu/find?id=1&locale=fr' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer [mon-token]'
`````

`````shell
curl --request GET \
--url '[url-de-mon-site]/api/v1/menu/find?page_slug=blog&position=2' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer [mon-token]'
`````

**Réponse 200**

url : `[url-de-mon-site]/api/v1/menu/find?id=1`
````json
{
  "code_http": 200,
  "message": "success",
  "data": {
    "id": 1,
    "position": "HEADER",
    "type": 2,
    "elements": [
      {
        "id": 1,
        "target": "_self",
        "label": "Index",
        "url": "",
        "slug": "home",
        "category": 1,
        "columnPosition": 1,
        "rowPosition": 1
      },
      {
        "id": 2,
        "target": "_self",
        "label": "Mon blog",
        "url": "",
        "slug": "blog",
        "category": 4,
        "columnPosition": 1,
        "rowPosition": 2
      }
    ]
  }
}
````

### Champs renvoyés

| Champ | Description |
|---|---|
| `id` | Id du menu |
| `position` | Position du menu, en mot-clé : `HEADER`, `RIGHT`, `FOOTER`, `LEFT` ([références](../../Architecture/references_globales.md#position-dun-menu)) |
| `type` | Type d'affichage du menu ([références](../../Architecture/references_globales.md#type-de-menu)) |
| `elements` | Éléments de premier niveau du menu |
| `elements[].id` | Id de l'élément |
| `elements[].target` | `_self` ou `_blank` ([références](../../Architecture/references_globales.md#cible-dun-lien-de-menu)) |
| `elements[].label` | Libellé dans la `locale` demandée |
| `elements[].url` | Lien externe. Vide si l'élément pointe vers une page du CMS |
| `elements[].slug` | Slug de la page cible dans la `locale` demandée. Vide pour un lien externe |
| `elements[].category` | Id de la catégorie de la page cible ([références](../../Architecture/references_globales.md#catégorie-de-page)). Chaîne vide pour un lien externe |
| `elements[].columnPosition` / `rowPosition` | Colonne et ligne de l'élément dans le menu |
| `elements[].elements` | Sous-éléments, même structure (présent seulement si l'élément a des enfants) |

**Réponse 400**

*Si `id` et `page_slug` sont présents ensemble*
````json
{
  "code_http": 400,
  "message": "Requête invalide",
  "errors": [
    "Le paramètre id et page_slug ne peuvent pas être mis ensemble"
  ]
}
````

*Si ni `id` ni `page_slug` ne sont présents*
````json
{
  "code_http": 400,
  "message": "Requête invalide",
  "errors": [
    "Le paramètre id ou page_slug doit être présent"
  ]
}
````

*Si `position` ne vaut pas 1, 2, 3 ou 4*
````json
{
  "code_http": 400,
  "message": "Requête invalide",
  "errors": [
    "Choisi une position entre 1 (haut) - 2 (droite) - 3 (bas) - 4 (gauche)"
  ]
}
````

**Réponse 404**

*Si le menu n'existe pas, est désactivé, ou si aucun menu de cette position n'est rattaché à la page*
````json
{
  "code_http": 404,
  "message": "Ressource non disponible",
  "errors": [
    "Menu non disponible"
  ]
}
````

**Réponse 403**

*Si le `User-Token` est présent mais invalide ou expiré*
````json
{
  "code_http": 403,
  "message": "Ressource non accessible",
  "errors": [
    "Utilisateur non trouvé"
  ]
}
````

Les erreurs communes (jeton invalide, API fermée, locale invalide) sont décrites dans la
[présentation de l'API](../index.md#format-des-réponses).

## Voir aussi
- [Find page](find_page.md)
- [Gestion des menus](../../GuideAdmin/Contenu/Menus/listing.md)
- [Références globales](../../Architecture/references_globales.md)
