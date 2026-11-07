---
title: "Installation en mode développeur"
parent: "Démarrage"
nav_order: 2
---

Procédure pour installer Nathéo CMS en local pour du développement, sans passer par l'installeur graphique.

### Pré-requis
[Voir les pré-requis](pre_requis.md)

## Installation

### Étape 1 : cloner le dépôt Git

```bash
git clone https://github.com/counteraccro/natheo.git
cd natheo
```

### Étape 2 : installer les dépendances PHP

```bash
composer install
```

### Étape 3 : configuration de l'environnement

Créer une copie du fichier `.env` en `.env.local` (ce dernier n'est jamais versionné) :

```bash
cp .env .env.local
```

Dans `.env.local`, ajuster au minimum :

| Variable | Valeur | Rôle |
|---|---|---|
| `APP_ENV` | `dev` | Environnement de l'application |
| `APP_DEBUG` | `1` | Active le mode debug Symfony |
| `NATHEO_DBNAME` | *(ex: `natheo`)* | Nom de la base de données |
| `NATHEO_DEBUG` | `true` | Active le mode debug applicatif ([voir les options de configuration](configuration_installation.md)) |
| `DATABASE_URL` | *(à adapter)* | DSN Doctrine (login/mot de passe/port de votre SGBD) |

> 💡 Par défaut le CMS est configuré pour MySQL. Pour utiliser PostgreSQL, voir la
> [procédure de changement de base de données](base_de_donnees.md).

### Étape 4 : installation du CMS

```bash
php bin/console natheo:install
```

Cette commande automatise toute l'installation applicative :

1. `doctrine:database:create` — création de la base de données
2. `doctrine:migrations:sync-metadata-storage` puis `doctrine:migrations:migrate` — création du schéma via les [migrations Doctrine](https://symfony.com/doc/current/doctrine.html#migrations-creating-the-database-tables-schema)
3. `doctrine:fixtures:load --append` — chargement des données de démonstration
4. `cache:clear` — vidage du cache

Elle demande une confirmation avant de s'exécuter, et **refuse de tourner si `APP_ENV=prod`** (dans ce cas, passer par [l'installeur graphique](installation_prod.md)). Si une base de données existe déjà pour le projet, elle est supprimée puis recréée après confirmation.

> 🔁 Cette commande est idempotente : vous pouvez la relancer à tout moment (par exemple pour repartir d'un jeu de données propre) — elle réinitialise entièrement la base.

### Étape 5 : génération des assets front

```bash
yarn install
yarn dev
```

`yarn dev` lance Vite en mode watch. Pour un build de production, voir la [commande `yarn build`](#commandes-utiles) ci-dessous.

## Accès au site

Sur votre environnement de développement :

1. Créer un virtual host qui pointe vers le dossier `[chemin-vers-natheo]/public`.
2. Ouvrir `http://[mon-virtual-host]/admin/fr/dashboard/index`.
3. S'authentifier avec le compte de démonstration : `user.demo@mail.fr` / `user.demo@mail.fr`.

## Commandes utiles

| Commande | Effet |
|---|---|
| `php bin/console natheo:install` | Installation/réinitialisation complète du CMS |
| `php bin/console natheo:install-bdd-test` | Crée/recrée la base de données dédiée aux tests (environnement `test`) |
| `php bin/console messenger:consume async -vv` | Traite les tâches asynchrones en file d'attente |
| `php bin/console doctrine:fixtures:load` | Recharge les fixtures |
| `php bin/console translation:extract --force --format=yaml fr\|en\|es` | Génère/extrait les traductions manquantes pour la langue donnée |
| `yarn dev` | Compilation JS/CSS en mode développement (avec watch) |
| `yarn build` | Compilation JS/CSS pour la production (type-check inclus) |
| `yarn type-check` | Vérification TypeScript seule |
| `php bin/phpunit --display-deprecations` | Lancement des tests unitaires |
| `php bin/phpunit --filter=nomDuTest --display-deprecations` | Lancement d'un test unitaire en particulier |

## Voir aussi
- [Installation via l'installeur](installation_prod.md)
- [Options de configuration](configuration_installation.md)
- [Bases de données prises en charge](base_de_donnees.md)
