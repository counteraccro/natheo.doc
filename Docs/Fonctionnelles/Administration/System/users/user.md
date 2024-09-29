# Gestion des utilisateurs

[Index](../../../../../index.md) > [Documentation fonctionnelle](../../../index.md) > [Administration](../../index.md) > Gestion des utilisateurs

*Permet de pouvoir gérer les personnes qui peuvent ce connecter à la partie administration du CMS*

![Listing](../../files/users/listing.png)

## Informations générales
Sidebar : **Système > Utilisateur**  
Droit d'accès : **ROLE_SUPER_ADMIN**

Nom entité : **User**  
Nom de la table en bdd : **natheo.user**

| Nom        | Type          | Null | Valeur par défaut  |
|------------|---------------|------|--------------------|
| id         | 	Int(11)      | 	Non | 	Aucune            |
| email      | 	Varchar(180) | 	Non | 	Aucune            |
| roles      | 	Json         | 	Non | 	Aucune            |
| password   | 	Varchar(255) | 	Non | 	Aucune            |
| login      | 	Varchar(100) | 	Oui | 	NULL              |
| firstname  | 	Varchar(100) | 	Oui | 	NULL              |
| lastname   | 	Varchar(100) | 	Oui | 	NULL              |
| disabled   | 	boolean      | 	Non | 	false             |
| anonymous  | 	boolean      | 	Non | 	false             |
| founder    | 	boolean      | 	Non | 	false             |
| created_at | 	datetime     | 	Non | 	CURRENT_TIMESTAMP |
| update_at  | 	datetime     | 	Oui | 	NULL              |

### Règles de gestions globales table user
- Le champ email est unique en base de donnée
- Le champ created_at est mis à la date du jour à la création d'une option
- Le champ update_at est mis à jour à la date du jour au format [aaaa-mm-jj hh:mm:ss] à chaque modification de la valeur d'une option

Nom entité : **UserData**  
Nom de la table en bdd : **natheo.user_data**

| Nom        | Type          | Null | Valeur par défaut  |
|------------|---------------|------|--------------------|
| id         | 	Int(11)      | 	Non | 	Aucune            |
| user_id    | 	Varchar(255) | 	Non | 	Aucune            |
| key        | 	Json         | 	Non | 	Aucune            |
| value      | 	text         | 	Non | 	Aucune            |
| created_at | 	datetime     | 	Non | 	CURRENT_TIMESTAMP |
| update_at  | 	datetime     | 	Oui | 	NULL              |

### Règles de gestions globales table user_data
- Liaison Many-to-One avec User
    - Une UserData appartient à 1 user
    - Un user peut avoir n userData
- La composition du champ key et user_id est UNIQUE en base de donnée
- Le champ created_at est mis à la date du jour à la création d'une option
- Le champ update_at est mis à jour à la date du jour au format [aaaa-mm-jj hh:mm:ss] à chaque modification de la valeur d'une option

### Liste des clés utilisées pour UserData

| Clé                      | 	Valeur par défaut | 	Description                                                                           |
|--------------------------|--------------------|----------------------------------------------------------------------------------------|
| KEY_RESET_PASSWORD       | 	     Aucune       | 	Clé généré pour identifier la personne avant le changement de mot de passe            |
| KEY_LAST_CONNEXION       | 	     Aucune       | 	Date de dernière connexion au format timestamp                                        |
| KEY_HELP_FIRST_CONNEXION | Aucune             | Généré lors de la création du compte fondateur, lance l'aide à la configuration du cms |
| KEY_TOKEN_CONNEXION | Aucune             | Token généré lors de la connexion via API du user                                      |
| TIME_VALIDATE_TOKEN | Aucune             | Temps de validité du token                                                             |

## Règles de gestions globales du tableau de données
Le tableau de données regroupe l'ensemble des utilisateurs présent sur le site, le trie par défaut ce fait sur l'id du user.

### Liste des actions possibles
Liste des actions possibles sur le listing des utilisateurs

![Listing](../../files/users/aide_listng.png)

#### Nouvel utilisateur
Permet de pouvoir créer un nouvel utilisateur   
Voir [Création d'un utilisateur](new_user.md)

#### Désactiver un utilisateur
Met le champ ``user.disabled à true``, ce qui empêche la personne de pouvoir ce connecter à la partie administration du CMS.   
Une confirmation est nécessaire pour pouvoir désactiver un utilisateur.   
Un utilisateur désactivé apparaitra avec le symbole ![Listing](../../files/users/disabled_eye.png) à côté de son l'id

#### Activer un utilisateur
Met le champ ``user.disabled à false``, ce qui autorise la personne à pouvoir ce connecter à la partie administration du CMS.   
Aucune confirmation n'est demandée pour réaliser cette action

#### Supprimer un utilisateur
Permet de supprimer de façon définitive un utilisateur ainsi que toutes les données associées à celui-ci.
Cette action est définitive et sans possibilité de retour.
Une confirmation est nécessaire pour pouvoir supprimer un utilisateur.   
La suppression d'un utilisateur est possible uniquement si :
* L'option OS_ALLOW_DELETE_DATA doit être a 1
* L'option OS_REPLACE_DELETE_USER doit être a 0

#### Anonymiser un utilisateur
Permet d'anonymiser un utilisateur afin de ne plus pouvoir l'identifier. Cette action permet de conserver les données produites sans conserver les données personnelles de l'utilisateur.   
Cette action est définitive et sans possibilité de retour.
Une confirmation est nécessaire pour pouvoir anonymiser un utilisateur.   
L'anonymisation d'un utilisateur est possible uniquement si :
* L'option OS_ALLOW_DELETE_DATA doit être a 1
* L'option OS_REPLACE_DELETE_USER doit être a 1

#### Modifier un utilisateur
Permet de pouvoir modifier les données personnelles d'un utilisateur   
Voir [Edition d'un utilisateur](edit_user.md)

#### Ce connecter en tant que
Permet de faire une prise de contrôle du compte de l'utilisateur sans utiliser le mot de passe de celui-ci.

## Fixtures
Path du fichier de données : ``src/DataFixtures/data/user_fixtures_data.yaml``  
Nom de la fixture : **UserFixtures**  
Groupe de fixtures : **user, registered**

Commande pour lancer uniquement cette fixture : ``php bin/console doctrine:fixture:load --group=user``

## Exemple de fichier de liste d'utilisateur
Le fichier de config pour générer les users est construit sous la forme suivante :
Ce fichier n'est pas utilisé lors de l'installation du CMS
````yaml
user:
  -
    email: user@natheo.com
    password: user@natheo.com
    login: user-1
    firstname: user-demo
    lastname: the-demo
    roles: ROLE_USER
    disabled: 0
    anonymous: 0
    founder: 0
````

