## Find menu

[Index](../../../index.md) > [API](../index.md) > Find menu

Permet de renvoyer un menu formaté en fonction de différents paramètres
Si le User-Token est présent dans le header et valide, renvoi le menu même si celui-ci est désactivé

Pour plus d'information sur les références globales, [cliquez ici](../../Techniques/Références_globales.md)


Paramètres attendus :

| nom        | type    | obligatoire | valeur par défaut | commentaire                          |
|------------|---------|-------------|-------------------|--------------------------------------|
| id         | Integer | OUI         |                   | Obligatoire si page_slug non présent |
| page_slug  | String  | OUI         |                   | Obligatoire si id non présent        |
| position   | Integer | NON         | 1                 |                                      |
| locale     | String  | NON         | fr                |                                      |

### Informations
Les types de menus existants :

| Type | Correspondance                             |
|------|--------------------------------------------|
| 1    | Header side bar                            |
| 2    | Header menu déroulant                      |
| 3    | Header menu déroulant, big menu            |
| 4    | Header menu déroulant, big menu 2 colonnes |
| 5    | Header menu déroulant, big menu 3 colonnes |
| 6    | Header menu déroulant, big menu 4 colonnes |
| 11   | Left - Right, side-bar                     |
| 12   | Left - Right, droite side-bar accordéon    |
| 16   | Footer 4 colonnes                          |
| 17   | Footer 1 ligne à droite                    |
| 18   | Footer 1 ligne centrée                     |

Les types de menus sont défini par le champ ```type```

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
{
  "code_http": 200,
  "message": "success",
  "data": {
    "position": "LEFT",
    "type": 12,
    "elements": [
      {
        "target": "_blank",
        "label": "Pages",
        "url": "#",
        "elements": [
          {
            "target": "_blank",
            "label": "Les nouveautées",
            "url": "",
            "slug": "new-in-natheo-cms"
          },
          {
            "target": "_blank",
            "label": "Démonstration",
            "url": "",
            "slug" : "demo-page"
          }
        ]
      }
    ]
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