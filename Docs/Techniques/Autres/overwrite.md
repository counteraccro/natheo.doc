# Surcharge des controllers

[Index](../../index.md) > [Documentation technique](../index.md) > Autres > Surcharge des controllers

Dans certains cas, pour le besoin de développements spécifiques il est nécessaire de devoir modifier du code existant
dans le CMS.

Pour éviter de perdre l'ensemble du travail réalisé dans le cas d'une mise à jour du CMS, NatheoCMS à mis
en place un système de surcharge afin de pouvoir surcharger une action d'un controller.

## Configuration
Chaque action qui doit être surchargé doit être configuré dans le fichier ```config/cms/overwrite.yaml```

```yaml
    overwrite:
      -
        controller: App\Controller\Admin\DashboardController      # Controller à overwrite
        route: admin_dashboard_page_demo                          # Route à overwrite
        overwrite:                                                # Données qui surchargent
          route: admin_overwrite_page_demo                        # Route qui va surcharger l'action
          parameters:                                             # Dans le cas la route possède des paramètres
            param : id                                            # ancien paramètre : nouveau paramètre
```

Pour chaque action qui doit être surchargé il faut défini la configuration suivante:
 * ```controller:```  Le namespace du controller à overwrite
 * ```route:``` La route de l'action à overwrite
 * ```overwrite:```
   * ```route:``` La nouvelle route
   * ```parameters``` Dans le cas ou la route contenait des paramètres vous pouvez les transférés à la nouvelle route 
avec des noms de paramètres différents sous la forme clé : valeur avec comme clé l'ancien paraètre et comme valeur le nom du nouveau paramètre

## Architecture

### Controller
Les controllers créés pour la surcharge doivent **obligatoirement** être dans le dossier ```src/Overwrite/Controller```

L'action surchargé doit être défini comme n'importe quelle action

```php
<?php

/**
 * Controller de test pour la surcharge
 * @author Gourdon Aymeric
 * @version 1.0
 */

namespace App\Overwrite\Controller\Admin\Tools;

use App\Controller\Admin\AppAdminController;
use App\Service\Admin\System\OptionSystemService;
use App\Utils\Breadcrumb;
use Symfony\Component\HttpFoundation\Response;
use Symfony\Component\Routing\Annotation\Route;
use Symfony\Component\Security\Http\Attribute\IsGranted;

#[Route('/admin/{_locale}/overwrite', name: 'admin_overwrite_', requirements: ['_locale' => '%app.supported_locales%'])]
#[IsGranted('ROLE_USER')]
class TestOverwriteController extends AppAdminController
{

    /**
     * Page Démo pour les elements html
     * @param OptionSystemService $optionSystemService
     * @param $id
     * @return Response
     */
    #[IsGranted('ROLE_SUPER_ADMIN')]
    #[Route('/page-demo/{id}', name: 'page_demo')]
    public function pageDemo(OptionSystemService $optionSystemService, $id): Response
    {
        $breadcrumb = [
            Breadcrumb::DOMAIN => 'message',
            Breadcrumb::BREADCRUMB => [
                'pagedemo.element.html' => '#'
            ]
        ];

        return $this->render('overwrite/admin/tools/page_demo.html.twig', ['breadcrumb' => $breadcrumb, 'id' => $id]);
    }
}


```

### les vues
Les vues surchargées doivent être misent dans le dossier **templates/overwrite**