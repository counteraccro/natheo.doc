---
title: "Listing pages par tag"
parent: "API"
nav_order: 10
---

Renvoie la liste paginée des pages **publiées** et **actives** portant un tag, de la plus récemment modifiée à la plus
ancienne.

Paramètres attendus (query string) :

| nom    | type    | obligatoire | valeur par défaut | commentaire                                |
|--------|---------|-------------|-------------------|--------------------------------------------|
| tag    | String  | OUI         |                   | Texte recherché dans le libellé des tags   |
| locale | String  | NON         | fr                | Langue des titres et slugs renvoyés        |
| page   | Integer | NON         | 1                 | Page de résultats                          |
| limit  | Integer | NON         | 25                | Nombre de pages par page de résultats (1 à 100) |

> ⚠️ La recherche n'est **pas exacte** : `tag` est cherché comme une sous-chaîne du libellé, sans tenir compte de la
> casse, dans **toutes les langues** du tag. Ainsi `tag=d` renvoie toutes les pages dont un tag contient la lettre
> « d » (`Démo`, `Documentation`…). Selon la configuration de la base, les accents peuvent aussi être ignorés
> (`demo` trouve `Démo` sur l'instance de démonstration). Les caractères `%` et `_` sont cherchés tels quels (ce ne
> sont pas des jokers).

> 📝 Le `User-Token` est vérifié s'il est envoyé, mais il est **sans effet** : les brouillons ne sont jamais
> listés.

**Requêtes CURL**
`````shell
curl --request GET \
--url '[url-de-mon-site]/api/v1/page/tag?tag=demo' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer [mon-token]'
`````

`````shell
curl --request GET \
--url '[url-de-mon-site]/api/v1/page/tag?tag=demo&locale=es&page=2&limit=5' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer [mon-token]'
`````

**Réponse 200**

url : `[url-de-mon-site]/api/v1/page/tag?tag=demo&limit=1`
````json
{
  "code_http": 200,
  "message": "success",
  "data": {
    "limit": 1,
    "current_page": 1,
    "pages": [
      {
        "title": "Mon blog",
        "slug": "blog",
        "author": "user.demo@mail.fr",
        "created": 1791061090,
        "update": 1791061090
      }
    ],
    "rows": 12
  }
}
````

| Champ | Description |
|---|---|
| `pages` | Pages de la page de résultats : titre et slug dans la `locale`, auteur, dates de création et de modification (timestamp Unix) |
| `limit` / `current_page` | Pagination appliquée |
| `rows` | Nombre **total** de pages trouvées |

**Réponse 404**

*Si aucune page ne correspond, ou si la page de résultats demandée est vide*
````json
{
  "code_http": 404,
  "message": "Ressource non disponible",
  "errors": [
    "Le tag recherché n'existe pas"
  ]
}
````

> 📝 Contrairement au [listing par catégorie](listing_pages_category.md), une page de résultats vide (par
> exemple `page=99`) renvoie cette erreur `404` au lieu d'une liste vide.

**Réponse 400**

*Si `tag` est absent ou vide*
````json
{
  "code_http": 400,
  "message": "Requête invalide",
  "errors": [
    "Cette valeur ne doit pas être vide."
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

Les erreurs communes (jeton invalide, API fermée, locale invalide, `limit` hors limites) sont décrites dans la
[présentation de l'API](../index.md#format-des-réponses).

## Voir aussi
- [Listing pages par catégorie](listing_pages_category.md)
- [Gestion des tags](../../GuideAdmin/Contenu/Tags/listing.md)
