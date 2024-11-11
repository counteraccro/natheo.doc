## Find menu

[Index](../../../index.md) > [API](../index.md) > Listing pages par tag

Permet de retourner un listing de pages par tag en fonction de différents paramètres

Paramètres attendus :

| nom    | type   | obligatoire | valeur par défaut | commentaire |
|--------|--------|-------------|-------------------|-------------|
| tag    | String | OUI         |                   | Obligatoire |
| locale | String | NON         | fr                |             |
| page   | int    | NON         | 1                 |             |
| limit  | int    | NON         | 20                |             |


### Informations


**Requêtes CURL**
`````shell
curl --request GET \
--url --location '[url-de-mon-site]/api/v1/page/tag?tag=natheo \
--header 'Accept: application/json' \
--header 'User-Token: [user-token' \
--header 'Authorization: Bearer [mon-token]'
`````

`````shell
curl --request GET \
--url --location '[url-de-mon-site/api/v1/page/tag?tag=natheo&locale=es&page=5&limit=1 \
--header 'Accept: application/json' \
--header 'Authorization: Bearer [mon-token]'
`````

**Réponse 200**

Pour un listing de page de type blog
url : [url-de-mon-site]/api/v1/page/tag?tag=natheo
````json
{
  "code_http": 200,
  "message": "success",
  "data": {
    "limit": 25,
    "current_page": 1,
    "pages": [
      {
        "title": "Bienvenue sur NatheoCMS",
        "slug": "bienvenue",
        "author": "user.demo@mail.fr",
        "created": 1731229302,
        "update": 1731229305
      }
    ],
    "rows": 1
  }
}
````

**Réponse 401**

*Si le token n'est pas valide*
````json
{
    "code_http": 401,
    "message": "Accès non autorisé",
    "errors": [
        "Token Invalide"
    ]
}
````

**Réponse 403**


*Si le paramètre locale n'est pas valide*
````json
{
  "code_http": 403,
  "message": "Ressource non accessible",
  "errors": [
    "Choisir une locale entre fr (français) ou es (espagnol) ou en (anglais) "
  ]
}
````

*Si le User-Token est présent mais faux et/ou périmé*
````json
{
    "code_http": 403,
    "message": "Ressource non accessible",
    "errors": [
        "Utilisateur non trouvé"
    ]
}
````

*Si le tag n'existe pas*
````json
{
  "code_http": 403,
  "message": "Ressource non accessible",
  "errors": [
    "Le tag recherché n'existe pas"
  ]
}
````