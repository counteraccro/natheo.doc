---
title: "Migrer de la v1 à la v2"
parent: "API"
nav_order: 0
---

À partir de la **2.1**, Nathéo CMS expose une nouvelle API, la **v2** (`/api/v2`), construite avec
[API Platform](https://api-platform.com/). L'API **v1** (`/api/v1`) reste disponible et continue de fonctionner sans
changement, mais elle est **dépréciée** : elle ne reçoit plus de nouvelle fonctionnalité et sera **supprimée en 3.0**.
Cette page liste ce qui change pour passer de l'une à l'autre.

## Calendrier

| Version | API v1 | API v2 |
|---|---|---|
| 2.0 | seule API disponible | — |
| 2.1 | dépréciée, correctifs de sécurité uniquement | disponible |
| 3.0 | supprimée | seule API disponible |

## Repérer un appel à la v1

Toutes les réponses de la v1, erreurs comprises, portent des en-têtes de dépréciation :

```
Deprecation: @1796083200
Link: <https://counteraccro.github.io/natheo.doc/Docs/API/migration_v2.html>; rel="deprecation"; type="text/html"
```

- `Deprecation` : date de la dépréciation, en timestamp Unix ([RFC 9745](https://www.rfc-editor.org/rfc/rfc9745)).
- `Link` : lien vers cette page.
- `Sunset` : date de suppression ([RFC 8594](https://www.rfc-editor.org/rfc/rfc8594)), ajoutée quand la date de la
  3.0 sera fixée.

Ces valeurs se règlent dans `config/services.yaml` (`app.api_v1.deprecated_at`, `app.api_v1.sunset_at`,
`app.api_v1.migration_guide_url`).

## Documentation de référence

La v2 est décrite par sa documentation OpenAPI, générée à partir du code :

- **Swagger UI** : `[url-de-mon-site]/api/v2/docs`, pour lire et tester chaque opération ;
- **OpenAPI (JSON)** : `[url-de-mon-site]/api/v2/docs.jsonopenapi`.

Elle n'est accessible que lorsque l'option système **Ouvrir l'API ?** est activée (sinon `404`).

## Ce qui ne change pas

- Le **jeton API**, créé dans le back-office, s'envoie toujours dans `Authorization: Bearer [mon-token]`.
- Les **rôles** (`ROLE_READ_API` pour lire, `ROLE_WRITE_API` pour écrire), le **filtrage par IP** et l'option
  **Ouvrir l'API ?** s'appliquent de la même façon.

## Correspondance des endpoints

| v1 (`/api/v1`) | v2 (`/api/v2`) |
|---|---|
| `GET /authentication` (tester un jeton) | `GET /info` |
| `POST /authentication/user` | `POST /user_sessions` |
| `GET /menu/find` | `GET /menus/{id}` ou `GET /menus?page=…&position=…` |
| `GET /page/find?slug=…` | `GET /pages?slug=…` (page d'accueil : `GET /pages?landing=true`), puis `GET /pages/{id}` |
| `GET /page/content` | `GET /page_contents/{id}` |
| `GET /page/category` | `GET /pages?category=…` |
| `GET /page/tag` | `GET /pages?tag=…` |
| `GET /comment/page` | `GET /pages/{pageId}/comments` |
| `POST /comment` | `POST /comments` |
| `PUT /comment/moderate/{id}` | `PATCH /comments/{id}` |
| `GET /options-systems`, `/options-systems/{key}` | `GET /option_systems`, `/option_systems/{key}` |
| `GET /sitemap` | `GET /sitemap` |

Nouveautés sans équivalent v1 : `GET /me` (utilisateur connecté), `GET /user_sessions` et
`DELETE /user_sessions/{id}` (sessions de l'utilisateur), `POST /user_session_refreshes` (renouvellement de la
session), `POST /page_reads` (compter une lecture de page).

Les identifiants et les liens entre ressources sont des **IRI** (`"/api/v2/pages/12"`) : c'est par exemple l'IRI de
la page que l'on envoie dans le champ `page` d'un nouveau commentaire.

## Format des réponses

La v1 enveloppe chaque réponse dans `{code_http, message, data}`. La v2 renvoie la ressource elle-même, en
**JSON-LD** par défaut (`Accept: application/ld+json`) ou en **JSON simple** avec `Accept: application/json` :

*v1*
````json
{
  "code_http": 200,
  "message": "success",
  "data": { "...": "..." }
}
````

*v2, `GET /api/v2/info` en JSON-LD*
````json
{
  "@context": "/api/v2/contexts/Info",
  "@id": "/api/v2/info",
  "@type": "Info",
  "siteName": "Nathéo CMS",
  "natheoVersion": "2.1.0",
  "apiVersion": "v2"
}
````

*La même en `Accept: application/json`*
````json
{
  "siteName": "Nathéo CMS",
  "natheoVersion": "2.1.0",
  "apiVersion": "v2"
}
````

Autres différences :

- noms de champs en **camelCase** (`updatedAt`, `openComment`) ;
- dates au format **ISO 8601** (`2026-11-11T19:29:56+00:00`) ;
- valeurs d'énumération en minuscules (`two_block_bottom`, `header`) ;
- un champ sans valeur est renvoyé à `null`, jamais omis.

## Collections et pagination

Les listes sont des collections Hydra : les éléments sont dans `member`, leur nombre total dans `totalItems`, et les
liens de pagination dans `view` (`first`, `previous`, `next`, `last`).

````json
{
  "@context": "/api/v2/contexts/Page",
  "@id": "/api/v2/pages",
  "@type": "Collection",
  "totalItems": 42,
  "member": [
    { "@id": "/api/v2/pages/12", "@type": "Page", "id": 12, "slug": "accueil", "title": "Accueil" }
  ],
  "view": {
    "@id": "/api/v2/pages?category=blog&page=1",
    "@type": "PartialCollectionView",
    "first": "/api/v2/pages?category=blog&page=1",
    "last": "/api/v2/pages?category=blog&page=3",
    "next": "/api/v2/pages?category=blog&page=2"
  }
}
````

| v1 | v2 |
|---|---|
| `page` | `page` |
| `limit` | `itemsPerPage` (20 par défaut, 100 au maximum) |

## Langue

La langue des contenus se choisit avec le paramètre `locale` (`fr`, `en`, `es`) ou, à défaut, avec l'en-tête
`Accept-Language`. Sans l'un ni l'autre, la langue par défaut du site s'applique. La langue utilisée est renvoyée
dans l'en-tête `Content-Language`, et les messages d'erreur sont traduits dans cette langue.

## Erreurs

Les erreurs suivent le format **`application/problem+json`** ([RFC 9457](https://www.rfc-editor.org/rfc/rfc9457)) :
`title` donne la catégorie de l'erreur, `detail` le message précis.

````json
{
  "title": "Ressource introuvable",
  "detail": "Page non disponible",
  "status": 404,
  "type": "/errors/404"
}
````

Un paramètre ou un corps invalide renvoie un **`422`** (au lieu du `400` de la v1), avec une entrée par erreur dans
`violations` :

````json
{
  "title": "Données invalides",
  "detail": "category: Cette valeur doit être l'un des choix proposés.",
  "status": 422,
  "violations": [
    {
      "propertyPath": "category",
      "message": "Cette valeur doit être l'un des choix proposés."
    }
  ]
}
````

Les réponses `401` portent l'en-tête `WWW-Authenticate: Bearer`.

## Utilisateur connecté

En v1, le token utilisateur s'envoie dans l'en-tête `User-Token`, **en plus** du jeton API. En v2, l'utilisateur
ouvre une **session**, et son jeton d'accès **remplace** le jeton API dans `Authorization` (il porte les rôles du jeton
API qui a ouvert la session).

1. Ouvrir la session avec le jeton API :

   ```shell
   curl --request POST \
   --url '[url-de-mon-site]/api/v2/user_sessions' \
   --header 'Content-Type: application/ld+json' \
   --header 'Authorization: Bearer [mon-token]' \
   --data '{"email": "[email]", "password": "[mot-de-passe]"}'
   ```

   ````json
   {
     "@id": "/api/v2/user_sessions/920",
     "@type": "UserSession",
     "id": 920,
     "accessToken": "jCwyYjfU8n3H...",
     "accessTokenExpiresAt": "2026-11-11T19:44:56+00:00",
     "refreshToken": "wZ1k7AVdLuqR...",
     "refreshTokenExpiresAt": "2026-11-18T19:29:56+00:00"
   }
   ````

2. Appeler l'API avec `Authorization: Bearer [accessToken]`. Le jeton d'accès est valable **15 minutes**.
3. Avant son expiration, le renouveler avec `POST /user_session_refreshes` et `{"refreshToken": "[refreshToken]"}` :
   la réponse contient une nouvelle paire de jetons, l'ancien refresh token n'est plus utilisable. La durée de vie du
   refresh token (1, 7 ou 30 jours) se règle dans les options système.
4. Se déconnecter avec `DELETE /user_sessions/current`.

`GET /me` renvoie l'utilisateur de la session ; un appel fait avec un simple jeton API reçoit un `403`.

## Écritures

- Les corps de requête sont en JSON-LD (`Content-Type: application/ld+json`) ou en JSON (`application/json`).
- Les modifications partielles (`PATCH`, par exemple la modération d'un commentaire) utilisent
  `Content-Type: application/merge-patch+json` et ne contiennent que les champs à modifier.
- Une création renvoie `201` avec la ressource créée.
