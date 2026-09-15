---
title: "Architecture backend"
parent: "Architecture"
nav_order: 2
---

Le code backend de Nathéo est écrit en PHP avec le framework Symfony.
Cette page explique les grandes règles d'organisation à connaître avant de
modifier ou d'ajouter une fonctionnalité : comment le code est rangé,
comment les différentes classes communiquent entre elles, et quels sont
les mécanismes transverses (surcharge, événements) mis à disposition.

## Trois univers séparés : Admin, Api, Front

Le code backend est divisé en trois univers qui ne se mélangent jamais :

| Univers | À quoi il sert | Où le trouver |
|---|---|---|
| **Admin** | Le back-office, l'interface utilisée pour administrer le site | [`src/Controller/Admin/...`](https://github.com/counteraccro/natheo/blob/master/src/Controller/Admin/AppAdminController.php) |
| **Api** | L'API JSON (v1), utilisée par le site public et par d'éventuelles applications externes | [`src/Controller/Api/v1/...`](https://github.com/counteraccro/natheo/blob/master/src/Controller/Api/v1/AppApiController.php) |
| **Front** | Les pages classiques du site public | [`src/Controller/Front/...`](https://github.com/counteraccro/natheo/blob/master/src/Controller/Front/AppFrontController.php) |

Chacun de ces trois univers a ses propres classes de base (un « Controller »
et un « Service » de départ, dont héritent tous les autres). Un même besoin
métier peut donc exister en plusieurs versions selon l'univers où il est
utilisé, sans jamais partager de code directement entre les trois.

Dans chaque univers, la règle est la même : les **controllers** restent
volontairement simples (ils reçoivent la requête, appellent le bon service,
renvoient la réponse), et toute la vraie logique métier vit dans des
classes **Service**. C'est donc presque toujours dans un `Service` qu'il
faut chercher ou ajouter du comportement, pas dans un `Controller`.

## Comment un service accède à ses dépendances

Un service a souvent besoin d'autres services pour fonctionner (accès à la
base de données, aux traductions, à d'autres services métier...). Plutôt
que de lister ces besoins un par un dans chaque classe, Nathéo utilise un
mécanisme centralisé : chaque univers (Admin, Api, Front) possède **une
seule liste**, qui recense tout ce qui est disponible pour les services de
cet univers :

```php
class AppAdminHandlerService
{
    public const array HANDLERS = [
        'translator' => TranslatorInterface::class,
        'pageService' => PageService::class,
        // ... tout ce qui peut être utilisé par un service admin
    ];
}
```

Un service métier concret (par exemple `TagService`) n'a donc jamais à
déclarer lui-même ses dépendances : il hérite simplement de la classe de
base de son univers, et peut appeler directement `$this->getPageService()`
ou `$this->getTranslator()` — ces méthodes existent déjà, une par entrée de
la liste ci-dessus.

**Concrètement, pour ajouter une nouvelle dépendance à un service admin**,
il faut l'ajouter à cette liste centrale (`AppAdminHandlerService::HANDLERS`)
plutôt que de l'injecter directement dans le service qui en a besoin — c'est
le premier endroit à regarder.

En dehors de ces trois univers, quelques services transverses utilisés
partout (journalisation, sécurité, dates) reposent sur une base plus légère
et continuent d'être injectés de façon classique.

## Un dossier par fonctionnalité

Chaque fonctionnalité du CMS (les tags, les pages, les menus...) est
organisée de la même façon, avec les mêmes noms de sous-dossiers répétés
dans plusieurs arborescences. Voici à quoi ça ressemble pour les tags :

```text
src/Controller/Admin/Content/TagController.php
src/Service/Admin/Content/Tag/TagService.php
src/Repository/Admin/Content/Tag/TagRepository.php
src/Entity/Admin/Content/Tag/Tag.php
src/Utils/Translate/Content/TagTranslate.php
```

Autrement dit : pas un gros dossier « tout le code des tags », mais un
même nom de dossier (`Tag`) répété sous `Controller/`, `Service/`,
`Repository/`, `Entity/`... En pratique, ajouter une fonctionnalité à un
domaine existant demande donc de toucher plusieurs de ces arborescences en
parallèle, chacune à son endroit habituel. La liste des services déjà
existants (non exhaustive) est recensée dans [Composants](composants/index.md).

## Préparer les textes envoyés à l'écran

Quand un écran d'administration doit afficher des textes traduits, le
controller ne demande pas ces traductions une par une : il délègue ce
travail à une classe dédiée, qui prépare d'un coup tout le tableau de
textes nécessaires à l'écran :

```php
class TagTranslate extends AppTranslate
{
    public function getTranslate(): array
    {
        return [
            'formTitleCreate' => $this->translator->trans('tag.form.title.create', domain: 'tag'),
            // ...
        ];
    }
}
```

Ce tableau est ensuite transmis tel quel à la partie visuelle de la page
(voir [Architecture frontend](frontend.md)), qui n'a donc jamais besoin
d'aller chercher elle-même une traduction.

## Adapter le CMS sans perdre son travail à chaque mise à jour

Nathéo est livré comme un produit qu'on met régulièrement à jour. Pour
permettre de personnaliser un comportement précis sans risquer de perdre
cette personnalisation à la prochaine mise à jour, un mécanisme de
« surcharge » permet de remplacer l'action d'un controller par une version
maison, déclarée à part (dans `config/cms/overwrite.yaml`), sans toucher au
fichier d'origine. Le détail complet est expliqué dans
[Surcharge des controllers](surcharge_controllers.md).

## Ce qui se déclenche automatiquement en coulisses

Certaines actions se déclenchent automatiquement, en réaction à un
événement, sans qu'aucun controller n'ait besoin de les appeler
explicitement :

| Ce qui se déclenche | Quand | À quoi ça sert |
|---|---|---|
| Journalisation | À chaque création/modification/suppression d'une donnée | Garder une trace de qui a fait quoi |
| Redirection de surcharge | Avant l'exécution d'un controller | Rediriger vers une version personnalisée si elle existe (voir ci-dessus) |
| Préfixe de table | Au chargement du schéma de base de données | Permettre à plusieurs instances de Nathéo de partager une même base |
| Choix de la langue | Avant chaque action d'un controller | Appliquer la langue préférée de l'utilisateur connecté (ou la langue par défaut du site pour un visiteur anonyme) |

Voir aussi [Composants](composants/index.md) pour la liste complète des
services, extensions et jeux de données de test existants.
