## Find menu

[Index](../../../index.md) > [API](../index.md) > Find page

Permet de renvoyer une page formatée en fonction de différents paramètres
Si le User-Token est présent dans le header et valide, permet de voir une page en brouillon

Paramètres attendus :

| nom               | type    | obligatoire | valeur par défaut | commentaire                                                                         |
|-------------------|---------|-------------|-------------------|-------------------------------------------------------------------------------------|
| slug              | String  | OUI         |                   | Obligatoire                                                                         |
| locale            | String  | NON         | fr                |                                                                                     |
| page              | Integer | NON         | 1                 | Dans le cas d'éléments de page de type listing, affiche la page choisi              |
| limit             | Integer | NON         | 25                | Dans le cas d'éléments de page de type listing, affiche le nombre d'éléments choisi |
| show_menus        | boolean | NON         | true              | remonte ou non les menus associés à la page                                         |
| show_tags         | boolean | NON         | true              | remonte ou non les tags associés à la page                                          |
| show_statistiques | boolean | NON         | true              | remonte ou non les statistiques associés à la page                                  |
| menu_position     | array   | NON         | 0                 | remonte uniquement les menus dans les positions demandés                            |


### Informations


**Requêtes CURL**
`````shell
curl --request GET \
--url --location '[url-de-mon-site]/api/v1/page/find?slug=bienvenue' \
--header 'Accept: application/json' \
--header 'User-Token: [user-token' \
--header 'Authorization: Bearer [mon-token]'
`````

`````shell
curl --request GET \
--url --location '[url-de-mon-site/api/v1/page/find?slug=bienvenue&locale=es&page=10&limit=250&show_menu=false&menu_positions=2%2C3%2C4' \
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


*Si le paramètre menu_position n'est pas valide*
````json
{
  "code_http": 403,
  "message": "Ressource non accessible",
  "errors": [
    "Choisi une position entre 0 (tout) - 1 (haut) - 2 (droite) - 3 (bas) - 4 (gauche). Plusieurs choix possible "
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

*Si la page n'existe pas*
````json
{
    "code_http": 403,
    "message": "Ressource non accessible",
    "errors": [
        "Page non disponible"
    ]
}
````