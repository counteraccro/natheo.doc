## Options Systems

[Index](../../../index.md) > [API](../index.md) > Options Systems

Permet de récupérer une liste ou une option system en particulier

**Requête CURL**
`````shell
curl --request GET \
--url '[url-de-mon-site]/api/v1/options-systems' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer [mon-token]'
`````

`````shell
curl --request GET \
--url '[url-de-mon-site]/api/v1/options-systems/OS_SITE_NAME' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer [mon-token]'
`````

**Réponse 200**

*Pour la liste des options systèmes*
````json
{
  "code_http": 200,
  "message": "success",
  "data": {
    "OS_SITE_NAME": "Nathéo CMS",
    "OS_FRONT_SCRIPT_TOP": "",
    "OS_FRONT_SCRIPT_START_BODY": "",
    "OS_FRONT_SCRIPT_END_BODY": "",
    "OS_OPEN_COMMENT": "1",
    "OS_ADRESSE_SITE": "http://www.value-must-be-change.com"
  }
}
````

**Réponse 200**

*Pour une option particulière*
````json
{
  "code_http": 200,
  "message": "success",
  "data": {
    "key": "OS_SITE_NAME",
    "value": "Nathéo CMS"
  }
}
````

**Réponse 403**

*Si l'option system n'existe pas*
````json
{
  "code_http": 403,
  "message": "Ressource non accessible",
  "errors": [
    "Option system non disponible"
  ]
}
````