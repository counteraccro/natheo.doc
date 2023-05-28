# Notifications

[Index](../../../../index.md) > [Documentation fonctionnelle](../../index.md) > [Administration](../index.md) > Notifications

*Notifications envoyés depuis le CMS*

## Informations générales

Nom entité : **notification**  
Nom de la table en bdd : **natheo.notification**  
*Table de stockage des notifications des utilisateurs*

| Nom        | Type          | Null | Valeur par défaut  |
|------------|---------------|------|--------------------|
| id         | 	Int(11)      | 	Non | 	Aucune            |
| user_id    | 	Int(11)      | 	non | 	Aucune            |
| title      | 	Varchar(255) | 	non | 	Aucune            |
| content    | 	Text         | 	Non | 	Aucune            |
| level      | 	int(1)       | 	Non | 	Aucune            |
| created_at | 	datetime     | 	Non | 	CURRENT_TIMESTAMP |

### Règles de gestions globales
- Un User peut posséder n notification
- Une notification ne peut avoir qu'un seul user
- Le champ created_at est mis à la date du jour à la création d'une option

## Ajouter une nouvelle notification
[Voir ajout nouvelle notification](../../../Techniques/composants/notification.md)

## Liste des options disponibles

| Clé                         | 	Description                                                                    |
|-----------------------------|---------------------------------------------------------------------------------|
| NOTIFICATION_WELCOME        | 	Notification envoyé à la personne à qui on à créé le compte                    |
| NOTIFICATION_SELF_DISABLED  | 	Notification envoyé aux super admin lorsqu'un utilisateur désactive son compte |
| NOTIFICATION_SELF_DELETE    | 	Notification envoyé aux super admin lorsqu'un utilisateur supprime son compte  |
| NOTIFICATION_SELF_ANONYMOUS | 	Notification envoyé aux super admin lorsqu'un utilisateur anonymise son compte |