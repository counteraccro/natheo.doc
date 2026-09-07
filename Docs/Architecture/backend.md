---
title: "Architecture backend"
parent: "Architecture"
nav_order: 2
---

> 🚧 **Nouvelle page à écrire.** Elle documentera le découpage en couches du backend :
>
> * Controllers minces, logique dans des classes `Service` par couche
>   (`AppAdminService`, `AppApiService`, `AppFrontService`)
> * Le pattern `#[AutowireLocator(self::HANDLERS)]` pour l'injection différée des
>   collaborateurs d'un service
> * Le découpage par domaine sous `src/` (`Admin/Content/...`, `Admin/System/...`,
>   `Admin/Tools/...`, dupliqué entre `Controller/`, `Service/`, `Repository/`,
>   `Entity/`, `Enum/`)
> * Les classes `Utils/Translate/<Domaine>/<Domaine>Translate.php` qui centralisent
>   la construction des tableaux de traduction passés à Twig/Vue
> * Le mécanisme de [surcharge des controllers](surcharge_controllers.md)
> * L'écoute des événements d'activité en base ([EventListener](composants/index.md))

*(à compléter)*
