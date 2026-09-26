---
title: "Commandes Symfony"
parent: "Architecture"
nav_order: 6
---

Pense-bête des commandes utiles au quotidien sur le projet. La plupart sont
des commandes Symfony standards (`bin/console ...`), auxquelles s'ajoutent
quelques commandes maison, préfixées `natheo:`, qui enchaînent plusieurs
étapes répétitives en une seule.

## Installation

| Commande | Ce qu'elle fait |
|---|---|
| `php bin/console natheo:install` | Crée la base, les tables, charge les fixtures de démo et vide le cache — en une seule commande interactive. Refuse de s'exécuter en environnement `prod`. |
| `php bin/console natheo:install-bdd-test` | Prépare (ou remet à zéro) la base dédiée aux tests automatisés. Se relance elle-même avec `APP_ENV=test` si besoin, pas la peine de le préciser à la main. |

Voir [Installation en mode développeur](../Demarrage/installation_dev.md) pour
le détail de la procédure complète.

## Base de données, étape par étape

Utile si `natheo:install` échoue en cours de route, ou pour ne rejouer
qu'une étape précise :

| Commande | Ce qu'elle fait |
|---|---|
| `php bin/console doctrine:database:create` | Crée la base de données (vide) |
| `php bin/console doctrine:migrations:sync-metadata-storage` | Prépare la table de suivi des migrations (à faire une seule fois, sur une base neuve) |
| `php bin/console doctrine:migrations:migrate --no-interaction --allow-no-migration` | Crée/met à jour les tables en rejouant les migrations |
| `php bin/console doctrine:database:drop --force` | Supprime entièrement la base de données |

Voir aussi [Base de données](../Demarrage/base_de_donnees.md).

## Fixtures

`php bin/console doctrine:fixtures:load --append --no-interaction` charge
l'ensemble du jeu de données de démonstration (`--append` évite de vider la
base avant, ce que fait la commande par défaut).

Pour ne charger qu'une partie des données, chaque jeu de fixtures est
rattaché à un ou plusieurs groupes (`tag`, `content`, `system`, `mail`...) :

```bash
php bin/console doctrine:fixtures:load --group=tag
```

Voir la [liste complète des groupes existants](composants/index.md#groupe-de-fixtures-existant),
et [Référence technique des Tags](../GuideAdmin/Contenu/Tags/technique.md) pour
un exemple concret de fichier de fixtures.

## Traductions

`php bin/console natheo:translations:update` extrait du code les nouvelles
clés de traduction et complète les fichiers YAML des 3 langues gérées (fr,
en, es), sans écraser les traductions déjà saisies — voir
[Traductions & multilingue](traductions_i18n.md).

## Tâches asynchrones (Messenger)

`php bin/console messenger:consume async -vv` doit tourner en tâche de fond
pour que les messages mis en file d'attente soient réellement traités :
l'envoi des e-mails, mais aussi l'export d'un dump SQL depuis le
gestionnaire de base de données du back-office. Sans ce worker actif, ces
actions restent en attente dans la file sans jamais s'exécuter.

## Tests unitaires

`php bin/phpunit` lance l'ensemble de la suite de tests — voir
[Tests unitaires](tests.md) pour cibler un fichier ou une méthode précise.

## Cache

`php bin/console cache:clear` vide le cache applicatif (à faire après toute
modification qui ne passe pas par l'interface, comme un fichier de
traduction édité directement sur le disque).

## Frontend (Vite)

| Commande | Ce qu'elle fait |
|---|---|
| `yarn dev` | Lance Vite en mode développement, avec rechargement instantané des fichiers Vue/TS modifiés |
| `yarn build` | Vérifie les types TypeScript (`vue-tsc`) puis génère les fichiers de production |
| `yarn type-check` | Vérifie uniquement les types TypeScript, sans générer les fichiers (utile en CI) |
| `yarn preview` | Sert localement les fichiers déjà générés par `yarn build`, pour vérifier le résultat de production |

Voir [Architecture frontend](frontend.md) pour le fonctionnement de la
compilation Vite.
