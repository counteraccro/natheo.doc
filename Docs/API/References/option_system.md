---
title: "Options système (API)"
parent: "API"
nav_order: 11
---

Renvoie les options système utiles au front, toutes ensemble ou une seule à partir de sa clé. Seule une **liste
blanche** d'options est exposée ; les autres (emails, sécurité, réglages de l'administration…) ne sont jamais
accessibles par l'API.

Les valeurs se modifient dans le back-office, voir [Options système](../../GuideAdmin/Systeme/options.md).

| Clé | Contenu |
|---|---|
| `OS_SITE_NAME` | Nom du site |
| `OS_ADRESSE_SITE` | URL du site |
| `OS_THEME_FRONT_SITE` | Thème du front |
| `OS_OPEN_COMMENT` | `1` si les commentaires sont ouverts sur le site, `0` sinon |
| `OS_FRONT_SCRIPT_TOP` | Script à insérer dans la balise `<head>` |
| `OS_FRONT_SCRIPT_START_BODY` | Script à insérer juste après l'ouverture de `<body>` |
| `OS_FRONT_SCRIPT_END_BODY` | Script à insérer juste avant la fermeture de `<body>` |
| `OS_FRONT_FOOTER_TEXTE` | Texte du pied de page |
| `OS_FRONT_FOOTER_SOCIAL_FACEBOOK_URL`, `..._INSTAGRAM_URL`, `..._GITHUB_URL`, `..._TIKTOK_URL`, `..._LINKEDIN_URL`, `..._YOUTUBE_URL`, `..._X_URL` | Liens vers les réseaux sociaux (vide si non renseigné) |
| `OS_FRONT_ROBOT_NO_INDEX` | `1` pour demander aux moteurs de recherche de ne pas indexer le site |
| `OS_FRONT_ROBOT_NO_FOLLOW` | `1` pour demander aux moteurs de recherche de ne pas suivre les liens |

Toutes les valeurs sont renvoyées sous forme de **texte**, booléens compris (`"0"` / `"1"`).

**Requêtes CURL**
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

*Pour la liste des options*
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
    "OS_ADRESSE_SITE": "http://dev.natheo:8888",
    "OS_THEME_FRONT_SITE": "natheo_horizon",
    "OS_FRONT_FOOTER_TEXTE": "NatheoCMS est un CMS headless développé avec Symfony pour vous offrir le meilleur compromis entre un site clé en main et la possibilité de le façonner à votre envie",
    "OS_FRONT_FOOTER_SOCIAL_FACEBOOK_URL": "",
    "OS_FRONT_FOOTER_SOCIAL_INSTAGRAM_URL": "",
    "OS_FRONT_FOOTER_SOCIAL_GITHUB_URL": "https://github.com/counteraccro/natheo",
    "OS_FRONT_FOOTER_SOCIAL_TIKTOK_URL": "",
    "OS_FRONT_FOOTER_SOCIAL_LINKEDIN_URL": "https://www.linkedin.com/in/aymeric-gourdon-7b1a6264/",
    "OS_FRONT_FOOTER_SOCIAL_YOUTUBE_URL": "",
    "OS_FRONT_FOOTER_SOCIAL_X_URL": "https://x.com/NatheoCms",
    "OS_FRONT_ROBOT_NO_INDEX": "0",
    "OS_FRONT_ROBOT_NO_FOLLOW": "0"
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

**Réponse 404**

*Si la clé n'existe pas ou ne fait pas partie de la liste blanche (ex. `OS_MAIL_FROM`)*
````json
{
  "code_http": 404,
  "message": "Ressource non disponible",
  "errors": [
    "Option system non disponible"
  ]
}
````

> 📝 Ni `OS_OPEN_SITE` (site ouvert au public) ni `OS_OPEN_API` (API ouverte) ne font partie de la liste. Quand
> l'API est fermée, c'est l'API entière qui répond `403` « API fermée ».

Les erreurs communes (jeton invalide, API fermée) sont décrites dans la [présentation de l'API](../index.md#format-des-réponses).

## Voir aussi
- [Options système](../../GuideAdmin/Systeme/options.md)
