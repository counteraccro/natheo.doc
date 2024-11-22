## Installation en mode développeur

[Index](../../index.md) > Installation en mode développeur

Procédure pour installer NatheoCMS sans passer par l'installateur.

### Pré-requis
[Voir les pré-requis](pre-requis.md)

### Installation

#### Étape 1 : cloner le dépôt GIT

```https://github.com/counteraccro/natheo.git```

#### Étape 2 : Installer Symfony

```composer update```

#### Étape 3 : Configuration .env

Créer une copie du fichier ```.env``` en ```.env.local```

* Mettre la valeur ```dev``` à  ```APP_ENV```
* Mettre la valeur ```1``` à ```APP_DEBUG```
* Mettre la valeur ```[nom-de-votre-bdd]``` à ```NATHEO_SCHEMA```

#### Étape 4 : Installation de natheo CMS

Cette commande permet de passer les étapes 5,6,7

```php bin/console natheo:install```

#### Étape 5 : installer la base de données

*A faire si vous souhaitez faire une installation point par point*

```php bin/console doctrine:database:create```

#### Étape 6 : récupération des tables de la base de données

*A faire si vous souhaitez faire une installation point par point*

```php bin/console doctrine:schema:create```

#### Étape 7 : installation des fixtures

*A faire si vous souhaitez faire une installation point par point*

```php bin/console doctrine:fixture:load```

#### Étape 8 : Génération des assets

```yarn encore dev -- watch```

# Accès au site
Sur votre environnement de développement
* Créer un virtual host qui pointe vers le dossier suivant : ```[path-complet-vers-mon-dossier]\www\natheo\public```
* Cliquez sur le lien ```http://[mon-virtual-host]/admin/fr/dashboard/index```
* Authentifier vous avec le login/mot de passe suivant : ```superadmin@natheo.com/superadmin@natheo.com```

# Commande :

Installation générale du CMS : ```php bin/console natheo:install```

Lancer les scripts en async : ```php bin/console messenger:consume async -vv```

Génération des traductions (fr|en|es) : ```php bin/console translation:extract --force --format=yaml en```

Chargement des fixtures : ```php bin/console doctrine:fixtures:load```

Compilations du JS : ```yarn encore dev -- watch```

Lancement des tests unitaires : ```bin/phpunit ```