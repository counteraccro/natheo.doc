## Find menu

[Index](../../../index.md) > [API](../index.md) > Find menu

Permet de renvoyer un menu formaté en fonction de différents paramètres
Si le user_token est présent et valide, renvoi le menu même si celui-ci est désactivé

Paramètres attendus :

| nom        | type    | obligatoire | valeur par défaut | commentaire                          |
|------------|---------|-------------|-------------------|--------------------------------------|
| id         | Integer | OUI         |                   | Obligatoire si page_slug non présent |
| page_slug  | String  | OUI         |                   | Obligatoire si id non présent        |
| position   | Integer | NON         | 1                 |                                      |
| locale     | String  | NON         | fr                |                                      |
| user_token | String  | NON         |                   |                                      |

**Requêtes CURL**
`````shell
curl --request GET \
--url '[url-de-mon-site]/api/v1/menu/find?page_slug=page&position=2&locale=fr' \
--header 'Accept: application/json' \
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
{
   
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