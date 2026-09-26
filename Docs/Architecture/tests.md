---
title: "Tests unitaires"
parent: "Architecture"
nav_order: 7
---

Nathéo s'appuie sur **PHPUnit**, avec les outils Symfony habituels
(`WebTestCase` pour les tests fonctionnels de controllers) et le bundle
[DAMA DoctrineTestBundle](https://github.com/dmaicher/doctrine-test-bundle),
qui enveloppe chaque test dans une transaction annulée à la fin : les
fixtures chargées restent valables d'un test à l'autre, sans jamais polluer
durablement la base.

## Préparer l'environnement de test

La connexion à la base dédiée aux tests se configure dans `.env.test`
(`DATABASE_URL`, `NATHEO_DBNAME`) — à adapter à votre environnement local.

Une fois cette configuration en place, une seule commande crée (ou remet à
zéro) la base et ses tables :

```bash
php bin/console natheo:install-bdd-test
```

Elle bascule elle-même sur `APP_ENV=test` si besoin — inutile de le préciser
à la main. Voir [Commandes Symfony](commandes.md#installation) pour le
détail de ce qu'elle fait, étape par étape.

## Lancer les tests

```bash
# L'ensemble de la suite
php bin/phpunit

# Un seul fichier de test
php bin/phpunit tests/Service/LoggerServiceTest.php

# Une seule méthode, dans un fichier précis
php bin/phpunit --filter testGetAllFiles tests/Service/LoggerServiceTest.php
```

## Organisation des tests

Le dossier `tests/` reprend la même arborescence que `src/` : le test d'un
controller ou d'un service se trouve au même chemin relatif que la classe
qu'il teste (`tests/Controller/Admin/DashboardControllerTest.php` teste
`src/Controller/Admin/DashboardController.php`, etc.). On y retrouve
notamment `Controller/Admin`, `Controller/Api`, `Service/Admin`,
`Service/Api` et `Utils/...`.

Le dossier `Helper/` sort de ce principe : il regroupe les outils communs à
tous les tests plutôt que les tests d'une classe précise (voir ci-dessous).

## Base commune : `AppWebTestCase`

Les tests fonctionnels (controllers) héritent de
[`AppWebTestCase`](https://github.com/counteraccro/natheo/blob/master/tests/AppWebTestCase.php),
qui prépare tout ce dont un test admin a besoin avant même qu'il commence :
client de test, routeur, traducteur, `EntityManager`, locale forcée sur le
français, et options système par défaut déjà générées.

Elle fournit aussi un raccourci très utilisé pour vérifier qu'un rôle
insuffisant est bien refusé, plutôt que de réécrire la même vérification
dans chaque test :

```php
// Vérifie qu'un utilisateur non connecté (ou avec un rôle insuffisant)
// se voit bien refuser l'accès à une route admin
$this->checkNoAccess('admin_sql_manager_index');
```

## Générer des données de test

Plutôt que de construire des entités à la main dans chaque test, un
ensemble de traits — un par domaine (`UserFixturesTrait`, `TagFixturesTrait`,
`PageFixturesTrait`...), regroupés dans
[`FixturesTrait`](https://github.com/counteraccro/natheo/blob/master/tests/Helper/Fixtures/FixturesTrait.php) —
expose des méthodes prêtes à l'emploi comme `$this->createUser()` ou
`$this->createTag()`. Ces méthodes s'appuient sur
[Faker](https://fakerphp.org/) (configuré en français via
[`FakerTrait`](https://github.com/counteraccro/natheo/blob/master/tests/Helper/FakerTrait.php))
pour générer des données réalistes plutôt que des chaînes de test
génériques.

## État actuel

*Chiffres du 13/09/2026, à obtenir soi-même avec `php bin/phpunit --list-tests` :*

- **77** fichiers de test
- **375** tests répertoriés par PHPUnit
