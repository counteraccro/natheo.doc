---
title: "Liste des commentaires en fonction d'une page"
parent: "API"
nav_order: 6
---

Renvoie la liste paginée des commentaires d'une page, désignée par son id ou par son slug.

Le texte des commentaires **en attente de validation** et **modérés** est masqué, sauf pour un utilisateur
connecté :

| Statut du commentaire | Sans `User-Token` | Avec le `User-Token` d'un Contributeur (ou plus) |
|---|---|---|
| `1` En attente de validation | `comment` = « En attente de validation » | Texte réel |
| `2` Validé | Texte réel | Texte réel |
| `3` Modéré | `comment` = « Commentaire modéré » | Texte réel + clé `moderate` (motif de modération) |

Paramètres attendus (query string) :

| nom        | type    | obligatoire | valeur par défaut | commentaire                                          |
|------------|---------|-------------|-------------------|------------------------------------------------------|
| id         | Integer | OUI*        |                   | Id de la page. *Obligatoire si `page_slug` est absent |
| page_slug  | String  | OUI*        |                   | Slug de la page. *Obligatoire si `id` est absent      |
| locale     | String  | NON         | fr                | Langue du `page_slug` (voir ci-dessous)              |
| page       | Integer | NON         | 1                 | Page de résultats                                    |
| limit      | Integer | NON         | 25                | Nombre de commentaires par page de résultats (1 à 100) |
| order      | String  | NON         | desc              | `asc` ou `desc`                                      |
| order_by   | String  | NON         | createdAt         | `createdAt` ou `id`                                  |

`id` et `page_slug` sont exclusifs : il en faut exactement un. Le token utilisateur s'envoie dans l'**en-tête**
`User-Token`, pas en paramètre.

> 📝 Avec `page_slug`, la `locale` doit être celle du slug : `page_slug=about` ne trouve rien avec la `locale` par
> défaut `fr` (réponse `200` avec une liste vide), il faut ajouter `locale=en`. La `locale` n'a pas d'autre effet :
> les commentaires ne sont pas traduits.

Seuls les commentaires **actifs** d'une page **publiée** et **active** sont renvoyés ; avec le `User-Token` d'un
Contributeur (ou plus), ceux des pages en **brouillon** aussi. Une page inexistante ou non visible ne provoque pas
d'erreur : la réponse est un `200` avec une liste vide.

**Requêtes CURL**
`````shell
curl --request GET \
--url '[url-de-mon-site]/api/v1/comment/page?page_slug=home&limit=10' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer [mon-token]'
`````

`````shell
curl --request GET \
--url '[url-de-mon-site]/api/v1/comment/page?id=1&order=asc&order_by=id' \
--header 'Accept: application/json' \
--header 'User-Token: [user-token]' \
--header 'Authorization: Bearer [mon-token]'
`````

**Réponse 200**

*Sans `User-Token`* — url : `[url-de-mon-site]/api/v1/comment/page?id=1` (textes raccourcis)
````json
{
  "code_http": 200,
  "message": "success",
  "data": {
    "comments": [
      {
        "id": 1,
        "author": "John Doe",
        "status": 1,
        "createdAt": 1791061090,
        "updateAt": 1791061090,
        "comment": "En attente de validation"
      },
      {
        "id": 2,
        "author": "Marc Doe",
        "status": 2,
        "createdAt": 1791061090,
        "updateAt": 1791061090,
        "comment": "Conlegerat ambo deducere indue Dodonaeo si vultu funduntur `pathPage` regit pars..."
      },
      {
        "id": 3,
        "author": "Denis Doe",
        "status": 2,
        "createdAt": 1791061090,
        "updateAt": 1791061090,
        "comment": "Lorem markdownum aevo quam semel, suadent, portasque carmine..."
      },
      {
        "id": 4,
        "author": "Franc Martin",
        "status": 3,
        "createdAt": 1791061090,
        "updateAt": 1791061090,
        "comment": "Commentaire modéré"
      }
    ],
    "current_page": 1,
    "rows": 4,
    "limit": 25
  }
}
````

**Réponse 200**

*Même commentaire modéré, avec le `User-Token` d'un Contributeur*
````json
{
  "id": 4,
  "author": "Franc Martin",
  "status": 3,
  "createdAt": 1791061090,
  "updateAt": 1791061090,
  "comment": "Umbra ortu levius, qua fert, libratum signa. Quam ego exspectat! Corpusque\n**cetera**...",
  "moderate": null
}
````

`moderate` vaut `null` si le commentaire a été modéré sans motif.

| Champ | Description |
|---|---|
| `comments[].id` | Id du commentaire (à passer à [Modérer un commentaire](moderate_comment.md)) |
| `comments[].author` | Nom saisi par l'auteur du commentaire |
| `comments[].status` | `1` en attente, `2` validé, `3` modéré |
| `comments[].createdAt` / `updateAt` | Dates de création et de modification (timestamp Unix, en secondes) |
| `comments[].comment` | Texte du commentaire (Markdown), ou message de remplacement selon le statut |
| `comments[].moderate` | Motif de modération (statut `3`, avec `User-Token` uniquement) |
| `current_page` / `limit` | Pagination appliquée |
| `rows` | Nombre **total** de commentaires de la page |

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

*Si `order_by`, `order`, `page` ou `limit` ne sont pas valides*
````json
{
  "code_http": 400,
  "message": "Requête invalide",
  "errors": [
    "Choisissez un orderBy entre id ou createdAt"
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
- [Ajouter un commentaire](add_comment.md)
- [Modérer un commentaire](moderate_comment.md)
- [Gestion des commentaires](../../GuideAdmin/Contenu/Commentaires/listing.md)
