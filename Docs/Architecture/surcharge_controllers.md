---
title: "Surcharge des controllers"
parent: "Architecture"
nav_order: 8
---

Personnaliser le comportement d'un écran du CMS demande normalement de
modifier le controller qui le gère — mais un fichier modifié directement
dans `src/Controller/` sera écrasé (ou entrera en conflit) à la prochaine
mise à jour du CMS. Nathéo propose donc un mécanisme de **surcharge** :
rediriger une route existante vers votre propre controller, qui vit dans un
dossier que les mises à jour du CMS ne touchent jamais.

## Comment ça marche

[`OverwriteListener`](https://github.com/counteraccro/natheo/blob/master/src/EventListener/OverwriteListener.php)
est un listener Symfony qui s'exécute juste avant chaque controller (événement
`kernel.controller`, détecté automatiquement par Symfony car sa méthode
`__invoke()` attend un `ControllerEvent`). À chaque requête, il compare le
controller et la route qui s'apprêtent à s'exécuter à la liste définie dans
`config/cms/overwrite.yaml`. En cas de correspondance, il ne modifie pas le
comportement en place : il **redirige** la requête vers la nouvelle route
que vous avez déclarée, en reportant au passage les paramètres nécessaires.

> 💡 Concrètement, l'URL affichée dans le navigateur change : c'est une
> vraie redirection HTTP vers votre propre écran, pas un remplacement
> silencieux du code exécuté.

## Mettre en place une surcharge

### 1. Créer le controller de remplacement

Il doit **obligatoirement** vivre dans `src/Overwrite/Controller/`, avec sa
propre déclaration de route — comme n'importe quel autre controller admin.
Depuis ce dossier, vous avez accès à l'ensemble des services, enums et
repositories du CMS.

```php
// src/Overwrite/Controller/Admin/Tools/DemoOverwriteController.php
namespace App\Overwrite\Controller\Admin\Tools;

use App\Controller\Admin\AppAdminController;
use App\Enum\Admin\Global\Breadcrumb;
use App\Service\Admin\System\OptionSystemService;
use Symfony\Component\HttpFoundation\Response;
use Symfony\Component\Routing\Attribute\Route;
use Symfony\Component\Security\Http\Attribute\IsGranted;

#[Route('/admin/{_locale}/overwrite', name: 'admin_overwrite_', requirements: ['_locale' => '%app.supported_locales%'])]
#[IsGranted('ROLE_USER')]
class DemoOverwriteController extends AppAdminController
{
    #[IsGranted('ROLE_SUPER_ADMIN')]
    #[Route('/action-surcharge/{id}', name: 'page_demo')]
    public function pageDemo(OptionSystemService $optionSystemService, $id): Response
    {
        $breadcrumb = [
            Breadcrumb::DOMAIN->value => 'demo_overwrite',
            Breadcrumb::BREADCRUMB->value => [
                'demo_overwrite.page_title_h1' => '#',
            ],
        ];

        return $this->render('overwrite/admin/tools/page_demo.html.twig', ['breadcrumb' => $breadcrumb, 'id' => $id]);
    }
}
```

### 2. Créer la vue associée

Si votre controller fait un `render()`, la vue Twig doit être placée dans
`templates/overwrite/` (ici, `templates/overwrite/admin/tools/page_demo.html.twig`)
— pour la même raison que le controller : ce dossier reste intact d'une
mise à jour à l'autre.

### 3. Déclarer la surcharge

Reste à indiquer au CMS quelle route rediriger vers votre nouvelle route,
dans `config/cms/overwrite.yaml` :

```yaml
overwrite:
    - controller: App\Controller\Admin\DashboardController  # Controller d'origine
      route: admin_dashboard_page_demo                      # Route d'origine à intercepter
      overwrite:
          route: admin_overwrite_page_demo                  # Route qui prend le relais
          parameters:                                       # Si la route d'origine a des paramètres
              param: id                                      # ancien nom du paramètre : nouveau nom
```

| Clé | Rôle |
|---|---|
| `controller` | Classe complète du controller à intercepter |
| `route` | Nom de la route d'origine à intercepter |
| `overwrite.route` | Nom de la nouvelle route, qui prend le relais |
| `overwrite.parameters` | Optionnel — reporte un paramètre de l'URL d'origine vers la nouvelle route, en cas de nom différent (`ancien: nouveau`) |

## Voir une surcharge en action

Le CMS embarque cet exemple précis à titre de démonstration : la page
d'origine reste consultable sur `/admin/{locale}/dashboard/page-demo/{param}`
(route `admin_dashboard_page_demo`), pendant que
`config/cms/overwrite.yaml` la redirige vers
`/admin/{locale}/overwrite/action-surcharge/{id}` (route `admin_overwrite_page_demo`,
gérée par `DemoOverwriteController`) — les deux mènent au même guide
interactif, directement dans le back-office.
