## Find menu

[Index](../../../index.md) > [API](../index.md) > Find page

Permet de renvoyer une page formatée en fonction de différents paramètres
Si le User-Token est présent dans le header et valide, permet de voir une page en brouillon

Paramètres attendus :

| nom        | type    | obligatoire | valeur par défaut | commentaire                          |
|------------|---------|-------------|-------------------|--------------------------------------|
| id         | Integer | OUI         |                   | Obligatoire si page_slug non présent |
| page_slug  | String  | OUI         |                   | Obligatoire si id non présent        |
| position   | Integer | NON         | 1                 |                                      |
| locale     | String  | NON         | fr                |                                      |

### Informations


**Requêtes CURL**
`````shell
curl --request GET \
--url '[url-de-mon-site]/api/v1/menu/find?page_slug=page&position=2&locale=fr' \
--header 'Accept: application/json' \
--header 'User-Token: [user-token]' \
--header 'Authorization: Bearer [mon-token]'
`````

`````shell
curl --request GET \
--url '[url-de-mon-site]/api/v1/menu/find?id=1' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer [mon-token]'
`````

**Réponse 200**
````json

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

*Si le paramètre id et page_slug est présent*
````json
{
  "code_http": 403,
  "message": "Ressource non accessible",
  "errors": [
    "Le paramètre id et page_slug ne peuvent pas être mis ensemble"
  ]
}
````

*Si le paramètre id ou page_slug est absent*
````json
{
  "code_http": 403,
  "message": "Ressource non accessible",
  "errors": [
    "Le paramètre id ou page_slug doit être présent"
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

*Si le menu n'existe pas*
````json
{
    "code_http": 403,
    "message": "Ressource non accessible",
    "errors": [
        "Menu non disponible"
    ]
}
````