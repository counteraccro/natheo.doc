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


## Ajouter une nouvelle notification