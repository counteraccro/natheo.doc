---
title: "Modération d'un commentaire (API)"
parent: "API"
nav_order: 8
---

Change le statut d'un commentaire et enregistre éventuellement un motif de modération. L'utilisateur identifié par
le `User-Token` est enregistré comme **modérateur** du commentaire.

C'est l'équivalent API de la [modération depuis le back-office](../../GuideAdmin/Contenu/Commentaires/moderation.md).

Deux conditions d'accès s'ajoutent :
- le jeton API doit avoir le rôle **Lecture + Écriture** (`ROLE_WRITE_API`) ou **Admin** (voir
  [Rôles des jetons](../index.md#rôles-des-jetons)) ;
- l'utilisateur du `User-Token` doit avoir au moins le rôle **Contributeur**.

Paramètres attendus :

| nom                | type    | obligatoire | valeur par défaut | commentaire                                                   |
|--------------------|---------|-------------|-------------------|---------------------------------------------------------------|
| id                 | Integer | OUI         |                   | Id du commentaire, **dans l'URL** (`/comment/moderate/{id}`)  |
| status             | Integer | OUI         |                   | Corps JSON. `1` en attente, `2` validé, `3` modéré (nombre ou chaîne numérique, `3` ou `"3"`) |
| moderation_comment | String  | NON         | vide              | Corps JSON. Motif de la modération, 10 000 caractères maximum, balises HTML retirées |
| User-Token         | String  | OUI         |                   | **En-tête** HTTP. Token de l'utilisateur modérateur           |

Le token utilisateur s'obtient avec [Authentification utilisateur](user_authentication.md). Il peut aussi être passé
dans le corps JSON sous la clé `user_token` ; l'en-tête est prioritaire s'il est présent.

> 📝 **Point relevé dans le code** : le motif et le modérateur sont enregistrés **quel que soit le statut**
> choisi : valider un commentaire avec un `moderation_comment` conserve ce motif, et l'omettre efface le motif
> précédent. Le back-office, lui, efface le motif dès que le statut n'est pas « Modéré ».

**Requête CURL**
`````shell
curl --request PUT \
--url '[url-de-mon-site]/api/v1/comment/moderate/4' \
--header 'Accept: application/json' \
--header 'Content-Type: application/json' \
--header 'User-Token: [user-token]' \
--header 'Authorization: Bearer [mon-token-ecriture]' \
--data-raw '{
    "status": 3,
    "moderation_comment": "Propos hors sujet"
}'
`````

**Réponse 202**
````json
{
  "code_http": 202,
  "message": "success",
  "data": [
    "Commentaire modéré"
  ]
}
````

Le message « Commentaire modéré » est renvoyé quel que soit le nouveau statut, y compris pour une validation.

**Réponse 400**

*Si `status` est absent, n'est pas un nombre ou ne vaut pas 1, 2 ou 3*
````json
{
  "code_http": 400,
  "message": "Requête invalide",
  "errors": [
    "Le status du commentaire est invalide"
  ]
}
````

*Si le corps de la requête est vide ou n'est pas du JSON valide*
````json
{
  "code_http": 400,
  "message": "Requête invalide",
  "errors": [
    "Le corps de la requête doit être un JSON valide"
  ]
}
````

**Réponse 403**

*Si le jeton API n'a pas le rôle `ROLE_WRITE_API`*
````json
{
  "code_http": 403,
  "message": "Ressource non accessible",
  "errors": [
    "Les droits du token API ne permettent pas d'accéder à cette ressource"
  ]
}
````

*Si le `User-Token` est absent, invalide ou expiré*
````json
{
  "code_http": 403,
  "message": "Ressource non accessible",
  "errors": [
    "Utilisateur non trouvé"
  ]
}
````

*Si l'utilisateur du `User-Token` n'est pas au moins Contributeur*
````json
{
  "code_http": 403,
  "message": "Ressource non accessible",
  "errors": [
    "Vous n'avez pas les droits pour modérer ce commentaire"
  ]
}
````

> 📝 Ce dernier cas ne se produit pas en pratique aujourd'hui : un compte qui n'a que le rôle Utilisateur ne peut
> pas [obtenir de token](user_authentication.md). Le contrôle protège contre un changement de rôle survenu après la
> connexion.

**Réponse 404**

*Si le commentaire n'existe pas*
````json
{
  "code_http": 404,
  "message": "Ressource non disponible",
  "errors": [
    "Commentaire non disponible"
  ]
}
````

Les erreurs communes (jeton invalide, API fermée) sont décrites dans la [présentation de l'API](../index.md#format-des-réponses).

## Voir aussi
- [Liste des commentaires d'une page](comment_by_page.md)
- [Authentification utilisateur](user_authentication.md)
- [Modération depuis le back-office](../../GuideAdmin/Contenu/Commentaires/moderation.md)
