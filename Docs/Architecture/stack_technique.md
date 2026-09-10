---
title: "Stack technique"
parent: "Architecture"
nav_order: 1
---

Vue d'ensemble des technologies utilisées par Nathéo : backend PHP/Symfony,
frontend Vue/Tailwind pour le back-office, thèmes Bootstrap pour le site
public, base de données, asynchrone et outillage de build.

## Backend

| Techno | Version | Rôle |
|---|---|---|
| [PHP](https://www.php.net/) | 8.2+ | Langage backend |
| [Symfony](https://symfony.com/) | 8.1 | Framework principal |
| [Doctrine ORM](https://www.doctrine-project.org/projects/orm.html) | 3.x (DBAL 4, migrations-bundle 3.2) | Mapping objet-relationnel, migrations |
| [Twig](https://twig.symfony.com/) | 2.12 / 3.x | Moteur de templates |
| [pentatrion/vite-bundle](https://github.com/pentatrion/symfony-vite-bundle) | 8.0 | Intégration Vite ↔ Twig (manifest, HMR) |
| [symfony/ux-vue](https://ux.symfony.com/vue) | 2.7 | Montage de composants Vue dans Twig (`vue_component()`) |
| [symfony/ux-twig-component](https://ux.symfony.com/twig-component) | 2.12 | Composants Twig réutilisables |

L'architecture applicative (couches `Controller`/`Service`/`Repository`,
pattern `AutowireLocator`, découpage par domaine) est détaillée dans
[Architecture backend](backend.md).

### Bundles complémentaires

| Bundle | Rôle |
|---|---|
| `symfony/mailer` + `symfony/notifier` | Envoi d'e-mails et de notifications |
| `twig/markdown-extra` | Rendu Markdown dans les templates (éditeur de contenu) |
| `twig/cssinliner-extra`, `twig/inky-extra` | Mise en forme des e-mails (CSS inline, layout responsive Inky) |
| `martin-georgiev/postgresql-for-doctrine` | Fonctions DQL spécifiques PostgreSQL (`CONTAINS`, `TO_JSONB`, ...) |
| `amphp/http-client` | Client HTTP asynchrone (appels externes non bloquants) |
| `phpdocumentor/reflection-docblock`, `phpstan/phpdoc-parser` | Introspection des docblocks (serializer, property-info) |
| `fakerphp/faker` | Génération de données pour les fixtures |
| `league/commonmark` | Parsing Markdown (moteur sous-jacent de `twig/markdown-extra`) |

## Base de données

Nathéo supporte deux SGBD au choix, via `DATABASE_URL` :

| SGBD | Version | Notes |
|---|---|---|
| MySQL / MariaDB | MySQL 8.0+ / MariaDB 10.11+ | Valeur par défaut en `.env` |
| PostgreSQL | 15+ (16 en environnement Docker) | Fonctions Doctrine dédiées via `martin-georgiev/postgresql-for-doctrine` |

L'environnement de développement fourni (`docker-compose.yml`) démarre un
conteneur PostgreSQL. Le détail des tables est dans le
[modèle de données](modele_donnees.md).

## Asynchrone

Les envois d'e-mails et notifications passent par **Symfony Messenger** sur
un transport `async` (Doctrine, table `messenger_messages` par défaut :
`MESSENGER_TRANSPORT_DSN=doctrine://default`) avec une stratégie de retry
(3 tentatives, backoff x2) et un transport `failed` pour les messages en
échec :

```bash
php bin/console messenger:consume async -vv
```

## Frontend back-office (admin)

| Techno | Version | Rôle |
|---|---|---|
| [Vue.js](https://vuejs.org/) | 3.4+ | Composants d'interface, montés en « îlots » dans le Twig admin |
| [TypeScript](https://www.typescriptlang.org/) | 5.9 | Typage du code Vue/TS (`assets/ts/`, `vue-tsc`) |
| [Vite](https://vitejs.dev/) | 8 | Bundler, dev server HMR |
| [vite-plugin-symfony](https://github.com/lhapaipai/vite-plugin-symfony) | 8 | Pont Vite ↔ Symfony (manifest, entrypoints) |
| [Tailwind CSS](https://tailwindcss.com/) | 4.1 | Framework utilitaire CSS de l'admin |
| [Flowbite](https://flowbite.com/) | 3.1 | Composants UI (plugin Tailwind) |
| [Stimulus](https://stimulus.hotwired.dev/) | 3.x (`@symfony/stimulus-bundle`) | Glue JS légère, notamment pour monter les composants Vue |

Autres dépendances JS notables : `axios`/`vue-axios` (appels API),
`sortablejs`/`vue-draggable-plus` (drag & drop), `marked` (rendu Markdown
côté client), `dompurify` (sanitation HTML).

Le détail du mécanisme d'intégration Vue ⇄ Twig (convention des props
`urls`/`translate`/`datas`, arborescence `assets/vue/controllers` vs
`assets/vue/Components`) est dans [Architecture frontend](frontend.md).

## Frontend site public (thèmes)

Le rendu public est piloté par un système de thèmes Twig (le thème fourni
par défaut est `natheoHorizon`, sous `assets/styles/front/`). Le thème par
défaut s'appuie sur :

| Techno | Version | Rôle |
|---|---|---|
| [Bootstrap](https://getbootstrap.com/) | 5.3 | Grille et composants CSS du site public |
| Bootstrap Icons | 1.10 | Iconographie |
| [Popper.js](https://popper.js.org/) | 2.11 | Positionnement des dropdowns/tooltips Bootstrap |

Un thème étant un ensemble de templates/assets indépendant, un autre thème
peut faire des choix différents (ex. ne pas utiliser Bootstrap).

## Tests & qualité

| Outil | Rôle |
|---|---|
| [PHPUnit](https://phpunit.de/) 12 | Tests unitaires et fonctionnels (`tests/`, base `AppWebTestCase`) |
| `vue-tsc` | Vérification de types TypeScript (`yarn type-check`, exécuté aussi au build) |

Voir [Tests](tests.md) pour la structure et les conventions.

## Outillage / build

| Commande | Rôle |
|---|---|
| `composer install` | Dépendances PHP |
| `php bin/console natheo:install` | Install complète en dev (DB + schéma + fixtures) |
| `yarn install` / `yarn dev` / `yarn build` | Dépendances JS, dev server Vite (HMR), build de prod |

Liste complète des commandes utiles : voir [Commandes](commandes.md).
