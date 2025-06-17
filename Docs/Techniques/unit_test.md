## Les tests unitaires

[Index](../../index.md) > [Documentation technique](index.md) > Tests unitaires

Natheo CMS possède ses propres tests unitaires afin de pouvoir tester un maximum de fonctionnalités de façon unitaire


### Installation environnement de test
Paramétrer l'accès à la base de données dans votre fichier .env.test
```yaml 
NATHEO_SCHEMA="natheo_test" 
DATABASE_URL="mysql://root:@127.0.0.1:3306/natheo_test?serverVersion=8.2.0&charset=utf8"
````

Créer la base données

``php bin/console --env=test doctrine:database:create``

Créer les tables et colonnes

``php bin/console --env=test doctrine:schema:create``

### Commandes pour lancer les tests unitaires
Lancer l'ensemble des tests unitaires : 

`` php bin/phpunit``

Lancer les tests unitaires uniquement sur un fichier

``php bin/phpunit  .\path\de\ma\class.php``

Lancer un test unitaire sur une méthode précise dans un fichier précis

``php bin/phpunit --filter monTest .\path\de\ma\class.php``

### Description des tests
L'ensemble des tests unitaires sont présents dans le dossier tests

* tests
  * Controller : Test des controllers
    * Api : Test des API
  * Helper : Class utilitaires pour générer les données de tests
  * Service : Test des services
  * Utils : Test des class du dossier Utils

### Statistiques

Actuellement, **390** fonctions sont testés avec **2987** tests ce qui représente environs **92%** du backoffice testé
