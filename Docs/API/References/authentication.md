---
title: "Authentification"
parent: "API"
nav_order: 1
---

Permet de vérifier qu'un jeton API est accepté (jeton valide, IP autorisée, API ouverte). Si c'est le cas, renvoie
les rôles portés par le jeton.

C'est l'endpoint à appeler pour tester la configuration d'un front avant d'interroger les autres.

**Requête CURL**
`````shell
curl --request GET \
--url '[url-de-mon-site]/api/v1/authentication' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer [mon-token]'
`````

**Réponse 200**

*Jeton de rôle « Lecture »*
````json
{
  "code_http": 200,
  "message": "success",
  "data": {
    "roles": [
      "ROLE_READ_API",
      "ROLE_USER"
    ]
  }
}
````

`roles` contient le rôle du jeton suivi de tous les rôles dont il hérite. Pour un jeton « Lecture + Écriture » :
`["ROLE_WRITE_API", "ROLE_READ_API", "ROLE_USER"]`. Voir [Rôles des jetons](../index.md#rôles-des-jetons).

**Réponse 401**

*Si aucun en-tête `Authorization: Bearer ...` n'est envoyé*
````json
{
  "code_http": 401,
  "message": "Accès non autorisé",
  "errors": [
    "Full authentication is required to access this resource."
  ]
}
````

Les erreurs communes (jeton invalide, API fermée) sont décrites dans la [présentation de l'API](../index.md#format-des-réponses).

## Voir aussi
- [Jetons API](../../GuideAdmin/Systeme/ApiToken/jetons_api.md)
- [Authentification utilisateur](user_authentication.md)
