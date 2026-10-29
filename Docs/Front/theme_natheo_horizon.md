---
title: "Le thème Natheo-Horizon (technique)"
parent: "Front"
nav_order: 1
---

Fonctionnement interne du thème public fourni avec Nathéo CMS, et pistes pour en créer un autre ou le remplacer par
un front externe. Pour ce que voit le visiteur, voir [Front](index.md).

## Chaîne de rendu

```
Navigateur ──GET /fr/article/a-propos──▶ IndexController::index()
                                           │  vérifications (installation, site ouvert)
                                           │  jeton du front, jeton utilisateur, URLs de l'API, traductions
                                           ▼
                            templates/front/natheo_horizon/index.html.twig
                              (balises <meta> SEO + conteneur Vue vide)
                                           ▼
                     Vue : Front/NatheoHorizon/Structure.vue (dans le navigateur)
                                           │  Authorization: Bearer <jeton du front>
                                           │  User-Token: <jeton utilisateur> (si connecté)
                                           ▼
                 API : /options-systems, /page/find, /page/content, /comment/...
```

1. Symfony reçoit l'URL et vérifie que le CMS est installé et que le site est ouvert.
2. Il rend le template Twig du thème avec trois jeux de données : `datas` (slug, langue, jeton du front, jeton
   utilisateur, catégories, chemins), `urls` (routes de l'API et liens utiles) et `translate` (textes du thème).
3. Le composant Vue `Structure` appelle en parallèle l'API des options système et `/page/find` pour la page
   demandée, puis affiche l'en-tête, le contenu et le pied de page.
4. Chaque bloc de contenu charge ses propres données via `/page/content` ; les commentaires passent par
   `/comment/page`, `/comment` (ajout) et `/comment/moderate/{id}` (modération).

Côté serveur, **aucun contenu de page n'est lu en base** en dehors des balises `<meta>` SEO : tout le reste vient de
l'API.

## Les données transmises au thème

| Clé `datas` | Contenu |
|---|---|
| `slug` | Slug demandé dans l'URL (`null` pour l'accueil) |
| `locale` | Langue de l'URL (`fr`, `en`, `es`) |
| `apiToken` | Valeur de l'option `OS_FRONT_API_TOKEN`, envoyée en `Authorization: Bearer` |
| `userToken` | Jeton utilisateur généré pour la personne connectée à l'administration (vide sinon), envoyé dans l'en-tête `User-Token` |
| `userInfo` | Login, avatar et droit de modération (`canModerate`, rôle contributeur ou plus) de l'utilisateur connecté, `null` sinon |
| `pageCategories` | Correspondance id de catégorie → slug d'URL, pour construire les liens |
| `pathAvatar`, `basePath` | Chemins publics des avatars et de l'application |

Les **jetons sont visibles dans le code source de la page**. Pour le jeton du front, c'est voulu (voir
[le jeton du front](../GuideAdmin/Systeme/options.md#le-jeton-du-front)) ; le jeton utilisateur, lui, n'est présent
que pour la personne connectée.

## Fichiers du thème

| Élément | Emplacement |
|---|---|
| Contrôleur | `App\Controller\Front\IndexController` (routes `front_no_local`, `front_index_2`, `front_sitemap`), parent `AppFrontController` |
| Choix du thème | `App\Service\Front\AppFrontService` : `getPathTemplate()` renvoie `front/{thème}` et `getScriptTags()` l'entrée Vite `front_{thème}`, d'après l'option `OS_THEME_FRONT_SITE` et l'enum `App\Enum\Front\Template` |
| Options lues par le serveur | `App\Service\Front\OptionSystemFrontService` (site ouvert, jeton du front, balise robots) |
| Templates Twig | `templates/front/front_base.html.twig` (squelette HTML, mode sombre appliqué avant affichage) et `templates/front/natheo_horizon/` (`index`, `close`, `sitemap.xml`) |
| Entrée Vite | `front_natheo_horizon` → `assets/app_front.ts` (CSS `assets/styles/app_front_natheo_horizon.css` et `styles/front/natheoHorizon/global.css`) |
| Composant racine | `assets/vue/controllers/Front/NatheoHorizon/Structure.vue` |
| Composants | `assets/vue/Components/Front/NatheoHorizon/` : `Nav/` (en-tête, menus déroulants et méga-menu), `Main/` (menus latéraux, `Content/` avec les blocs texte, FAQ, listing, commentaires, fil d'Ariane, pagination), `Footer/`, `Skeleton.vue` |
| Client API | `assets/utils/Front/FrontApi.ts` (ajoute les en-têtes, extrait `data` de la réponse, convertit les erreurs) |
| Utilitaires | `assets/utils/Front/UtilsFront.ts` (construction des URL, dates, initiales) et constantes `assets/utils/Front/Const/` (types de contenu, mises en page, statuts) |
| Types | `assets/ts/Front/NatheoHorizon/` |
| Traductions | Domaines `front` et `front_error` (`translations/front*+intl-icu.*.yaml`), servies par `App\Utils\Translate\Front\FrontTranslate` |

### Mises en page et blocs

Le champ `render` de la page choisit la grille (`assets/utils/Front/Const/PageRender.ts`) et le type de chaque bloc
choisit le composant (`ContentType.ts`) :

| `render` | Grille | | `type` de bloc | Composant |
|---|---|---|---|---|
| 1 | 1 bloc pleine largeur | | 1 | `ContentText` (Markdown rendu) |
| 2 | 2 blocs côte à côte | | 2 | `ContentFaq` (accordéon) |
| 3 | 3 blocs côte à côte | | 3 | `ContentListing` (pages publiées de la catégorie du bloc, paginées) |
| 4 / 5 | 2 / 3 blocs l'un sous l'autre | | | |
| 6 | 1 bloc puis 2 côte à côte | | | |
| 7 | 2 côte à côte puis 1 bloc | | | |
| 8 | 2 × 2 blocs | | | |

Un `render` inconnu est affiché comme une page à un bloc ; un bloc de type inconnu est ignoré.

### Liens internes

`UtilsFront::getUrl()` construit les liens des menus et listings : URL externe si l'élément en a une, sinon
`{basePath}/{langue}/{slug de catégorie}/{slug}`.

## Créer un autre thème

Le changement de thème passe par l'option **Thème de votre site**. Pour ajouter un thème `mon_theme` au CMS :

1. ajouter un cas à l'enum `App\Enum\Front\Template` (`MON_THEME = 'mon_theme'`) et une entrée à la liste
   `list_value` de `OS_THEME_FRONT_SITE` dans `config/cms/options_system.yaml` ;
2. créer `templates/front/mon_theme/` avec au moins `index.html.twig`, `close.html.twig` et `sitemap.xml.twig`, qui
   reçoivent les mêmes variables que ceux de Natheo-Horizon ;
3. déclarer l'entrée Vite `front_mon_theme` dans `vite.config.ts` (bloc `rolldownOptions.input`).

Si l'un de ces éléments manque, le site public renvoie une erreur 500 (« Le template … n'existe pas » quand le thème
n'est pas dans l'enum, erreur Twig ou Vite sinon). C'est aujourd'hui le cas du thème **FrontStorm**
(`natheo_storm`), déclaré dans l'enum et l'option mais dont les fichiers n'existent pas.

## Utiliser un front externe

Comme Natheo-Horizon ne fait qu'appeler l'API, un front développé à part peut s'y substituer complètement :

- créez-lui un [jeton API](../GuideAdmin/Systeme/ApiToken/nouveau_jeton.md) (rôle écriture si les visiteurs
  peuvent commenter) ;
- appelez les mêmes routes que le thème, dans le même ordre : options système, puis `/page/find`, puis
  `/page/content` pour chaque bloc (voir [API](../API/index.md)) ;
- si votre front appelle l'API depuis un serveur et non depuis le navigateur, lisez les remarques sur l'IP et la
  limite anti-spam dans [Ajouter un commentaire](../API/References/add_comment.md) et sur le filtre IP dans
  [API](../API/index.md) ;
- récupérez les scripts personnalisés (analytics...) dans les options système : contrairement à Natheo-Horizon, un
  front externe peut les injecter.

Fermez alors le site public intégré avec l'option **Ouvrir le site au public ?** : l'API reste accessible tant que
**Ouvrir l'API ?** est activée.

## Voir aussi
- [Front](index.md)
- [API](../API/index.md)
- [Architecture frontend](../Architecture/frontend.md)
- [Surcharge des contrôleurs](../Architecture/surcharge_controllers.md)
