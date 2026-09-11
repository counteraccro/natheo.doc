---
title: "Architecture backend"
parent: "Architecture"
nav_order: 2
---

Le backend est découpé en 3 couches applicatives parallèles (Admin, Api,
Front), chacune avec ses propres classes de base. Dans chaque couche, les
controllers restent minces : la logique vit dans des classes `Service`, qui
récupèrent leurs dépendances via un pattern d'injection différée commun à
tout le projet.

## Les 3 couches

| Couche | Controller de base | Service de base | Usage |
|---|---|---|---|
| Admin | [`AppAdminController`](https://github.com/counteraccro/natheo/blob/master/src/Controller/Admin/AppAdminController.php) | [`AppAdminService`](https://github.com/counteraccro/natheo/blob/master/src/Service/Admin/AppAdminService.php) → `AppAdminHandlerService` | Back-office (`src/Controller/Admin/...`) |
| Api | [`AppApiController`](https://github.com/counteraccro/natheo/blob/master/src/Controller/Api/v1/AppApiController.php) → `AppApiHandlerController` | `AppApiService` → `AppApiHandlerService` | API JSON v1 (`src/Controller/Api/v1/...`) |
| Front | [`AppFrontController`](https://github.com/counteraccro/natheo/blob/master/src/Controller/Front/AppFrontController.php) | `AppFrontService` → `AppFrontHandlerService` | Site public (`src/Controller/Front/...`) |

Chaque `App*Service` hérite d'un `App*HandlerService` propre à sa couche, qui
porte uniquement la liste des dépendances disponibles (voir plus bas) et
n'expose que des méthodes `getXxx()` vers le conteneur — les 3 couches ne
partagent pas de handler commun, chacune redéclare ce dont elle a besoin. Un
service concret (ex. `TagService`) n'étend que `AppAdminService` et n'a
jamais à déclarer ses propres dépendances : tout transite par le handler de
sa couche. En dehors de ces 3 couches, quelques services transverses
(`LoggerService`, `SecurityService`, `DateService`) étendent directement
[`AppService`](https://github.com/counteraccro/natheo/blob/master/src/Service/AppService.php),
une base plus légère avec un socle de dépendances réduit (entity manager,
translator, security, request stack).

## Injection différée : `AutowireLocator`

Plutôt que d'injecter individuellement chaque dépendance dans le constructeur
de chaque service, chaque `App*HandlerService` déclare une seule fois la
liste de tout ce qui peut être utilisé dans sa couche, via l'attribut
`#[AutowireLocator(self::HANDLERS)]` sur un unique paramètre
`ContainerInterface $handlers` :

```php
class AppAdminHandlerService
{
    public const array HANDLERS = [
        'logger' => LoggerInterface::class,
        'entityManager' => EntityManagerInterface::class,
        'translator' => TranslatorInterface::class,
        'router' => UrlGeneratorInterface::class,
        'security' => Security::class,
        'optionSystemService' => OptionSystemService::class,
        'gridService' => GridService::class,
        'pageService' => PageService::class,
        // ...
    ];

    public function __construct(#[AutowireLocator(self::HANDLERS)] protected ContainerInterface $handlers) {}

    protected function getPageService(): PageService
    {
        return $this->handlers->get('pageService');
    }
    // un getXxx() par entrée de HANDLERS
}
```

Une classe métier (`TagService extends AppAdminService`) appelle simplement
`$this->getTranslator()`, `$this->getGridService()`, etc. — ces accesseurs
sont hérités, elle n'a ni constructeur ni propriétés à déclarer pour ça.

**Pour ajouter une nouvelle dépendance à un service admin**, il faut
l'ajouter à `AppAdminHandlerService::HANDLERS` (et créer le `getXxx()`
correspondant) plutôt que de l'injecter directement — c'est cette liste
centrale qu'il faut regarder avant d'ajouter un collaborateur. `AppAdminController`
utilise le même principe pour ses propres dépendances transverses
(`OptionUserService`, `LoggerInterface`, ...), avec sa propre liste inline
(pas de constante `HANDLERS` partagée avec les services).

## Découpage par domaine sous `src/`

Chaque domaine fonctionnel est dupliqué à l'identique dans plusieurs arbres
parallèles sous `src/` : `Controller/`, `Service/`, `Repository/`,
`Entity/`, `Utils/Translate/`, généralement sous
`Admin/Content/<Domaine>/…`, `Admin/System/…` ou `Admin/Tools/…` (+ arbres
`Api/` et `Front/` pour ces couches-là). Exemple avec le domaine `Tag` :

```text
src/Controller/Admin/Content/TagController.php
src/Service/Admin/Content/Tag/TagService.php
src/Repository/Admin/Content/Tag/TagRepository.php
src/Repository/Admin/Content/Tag/TagTranslationRepository.php
src/Entity/Admin/Content/Tag/Tag.php
src/Entity/Admin/Content/Tag/TagTranslation.php
src/Utils/Translate/Content/TagTranslate.php
```

En ajoutant une fonctionnalité à un domaine existant, il faut donc
généralement toucher le sous-dossier correspondant dans plusieurs de ces
arbres en même temps. La liste des services/extensions Twig/fixtures
existants (non exhaustive) est recensée dans [Composants](composants/index.md).

## `Utils/Translate/<Domaine>Translate`

Les classes `Utils/Translate/<Domaine>/<Domaine>Translate.php` (qui étendent
[`AppTranslate`](https://github.com/counteraccro/natheo/blob/master/src/Utils/Translate/AppTranslate.php))
centralisent la construction du tableau de traductions passé en props aux
composants Vue, pour éviter d'appeler le translator directement dans les
controllers :

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

Le controller instancie cette classe et passe son résultat comme prop
`translate` au `vue_component()` (voir [Architecture frontend](frontend.md)).

## Surcharge de controllers

Pour permettre des développements spécifiques sans perdre le travail à
chaque mise à jour du CMS, Nathéo fournit un mécanisme de surcharge d'action
de controller piloté par `config/cms/overwrite.yaml` (controllers dans
`src/Overwrite/Controller/...`, vues dans `templates/overwrite/...`). Détail
complet : [Surcharge des controllers](surcharge_controllers.md).

## Événements

| Listener | Déclenché sur | Rôle |
|---|---|---|
| [`DatabaseActivityListener`](https://github.com/counteraccro/natheo/blob/master/src/EventListener/DatabaseActivityListener.php) | `postPersist` / `postUpdate` / `postRemove` Doctrine | Journalise chaque création/modification/suppression d'entité via `LoggerService` |
| [`OverwriteListener`](https://github.com/counteraccro/natheo/blob/master/src/EventListener/OverwriteListener.php) | `kernel.controller` | Redirige vers l'action de surcharge définie dans `overwrite.yaml` (voir ci-dessus) |
| [`DatabaseTablePrefixListener`](https://github.com/counteraccro/natheo/blob/master/src/EventListener/DatabaseTablePrefixListener.php) | `loadClassMetadata` Doctrine | Ajoute un préfixe de table configurable (ex. isolation multi-instances sur un même schéma) |
| [`LocaleSubscriber`](https://github.com/counteraccro/natheo/blob/master/src/EventSubscriber/LocaleSubscriber.php) | Avant chaque action de controller | Positionne la langue de la requête depuis l'option utilisateur |

Voir aussi [Composants](composants/index.md) pour la liste des services,
extensions Twig et fixtures.
