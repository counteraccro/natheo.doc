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
