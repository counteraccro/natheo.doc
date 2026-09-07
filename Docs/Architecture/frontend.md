---
title: "Architecture frontend"
parent: "Architecture"
nav_order: 3
---

> 🚧 **Nouvelle page à écrire.** Elle documentera comment le frontend Vue s'intègre
> dans les templates Twig :
>
> * Les pages d'admin sont du Twig serveur (`admin/admin_base.html.twig`), dans
>   lesquelles des « îlots » Vue sont montés via `vue_component()`
>   (`symfony/ux-vue`), ex. :
>   ```twig
>   <div {{ vue_component('Admin/Dashboard/Dashboard', { 'urls': urls, 'translate': translate, 'datas': datas }) }}></div>
>   ```
> * Convention des 3 groupes de props : `urls` (routes générées), `translate`
>   (via une classe `AppTranslate`), `datas` (état serveur)
> * `assets/vue/controllers/...` (composants montés) vs `assets/vue/Components/...`
>   (composants réutilisables non montés directement)
> * `assets/ts/` : fichiers de types purs, convention `<Composant>.type.ts`,
>   alias `@/` → `assets/`
> * Build via Vite (`yarn dev`, `yarn build`, `yarn type-check`)

*(à compléter)*
