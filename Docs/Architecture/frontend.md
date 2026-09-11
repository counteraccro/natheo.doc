---
title: "Architecture frontend"
parent: "Architecture"
nav_order: 3
---

La partie visuelle de Nathéo repose sur deux briques qui travaillent
ensemble : **Twig**, qui génère la structure de chaque page côté serveur, et
**Vue**, qui prend le relais pour tout ce qui doit réagir à l'utilisateur
(formulaires, tableaux, éditeur de contenu...). Ce n'est donc pas une
application 100% Vue : la page reste une page Twig classique, dans laquelle
un ou plusieurs petits « blocs Vue » viennent s'insérer, un peu comme des
widgets interactifs posés sur une page normale.

## Comment un bloc Vue arrive dans une page

Dans un template Twig, la fonction `vue_component()` insère un bloc Vue à
un endroit précis, en lui donnant les informations dont il a besoin pour
fonctionner :

```twig
{% raw %}<div {{ vue_component('Admin/Content/TagForm', {
    'url' : path('admin_tag_save'),
    'translate' : translate,
    'pTag' : tag
}) }}></div>{% endraw %}
```

Ces informations passées au bloc s'appellent des « props ». Sur les
composants récents, elles sont regroupées en trois familles, toujours
nommées de la même façon pour rester prévisibles :

- **`urls`** : les liens vers le serveur dont le bloc aura besoin (par
  exemple pour enregistrer un formulaire) ;
- **`translate`** : les textes de l'interface, déjà traduits dans la langue
  de l'utilisateur ;
- **`datas`** : les données déjà connues au moment d'afficher la page (une
  fiche à éditer, une liste d'options...).

Quelques composants plus anciens du projet (comme le formulaire de tag)
transmettent encore ces informations une par une plutôt que regroupées en
trois blocs. C'est une façon de faire héritée du début du projet : les
nouveaux composants doivent suivre le découpage en trois familles ci-dessus.

## Où se trouve le code

Les fichiers `.vue` sont répartis en deux familles de dossiers, selon leur
rôle :

- **`assets/vue/controllers/`** : un fichier par bloc réellement posé sur
  une page (celui appelé par `vue_component()`). Son emplacement dans ce
  dossier suit celui de l'écran correspondant côté administration.
- **`assets/vue/Components/`** : des briques plus petites, réutilisées par
  plusieurs blocs (une fenêtre modale, une notification, un bouton...). Elles
  ne sont jamais posées directement sur une page, seulement importées par un
  autre composant.

Dans ce second dossier, un sous-dossier `Skeleton/` regroupe les
« squelettes de chargement » : ces silhouettes grisées qui s'affichent
brièvement avant que les vraies données arrivent, comme sur beaucoup de
sites et d'applications modernes.

## Du JavaScript classique... et du TypeScript

Le projet a évolué au fil du temps, et deux façons d'écrire un composant
Vue coexistent aujourd'hui :

- les composants les plus anciens sont écrits en JavaScript classique,
  sans vérification particulière sur la forme des données reçues ;
- les composants récents sont écrits en **TypeScript**, une version du
  JavaScript qui décrit précisément la forme attendue de chaque donnée.
  L'intérêt : une erreur (une donnée manquante, un mauvais type) est
  détectée immédiatement à l'écriture du code, plutôt que découverte plus
  tard par un utilisateur.

Pour cela, chaque composant récent a un petit fichier voisin qui décrit ses
données, rangé dans `assets/ts/` :

```ts
// assets/ts/Dashboard/Dashboard.type.ts — la « fiche descriptive » des données du composant Dashboard
export interface DashboardUrls {
  dashboard_last_comments: BlockLastCommentUrls;
  // ...
}
```

Tout nouveau composant doit être écrit dans ce style TypeScript, sur le
modèle des composants récents (`Dashboard`, `Menu`, l'éditeur Markdown...),
plutôt que reproduire l'ancien style.

## Une différence de fonctionnement entre l'admin et le site public

Côté **administration**, chaque écran pose un ou plusieurs blocs Vue qui
reçoivent leurs données directement au chargement de la page, déjà prêtes
à l'emploi.

Côté **site public**, le fonctionnement est différent : la page n'affiche
qu'un seul bloc Vue, qui prend en charge l'intégralité de l'écran (menu,
contenu, pied de page...). Ce bloc affiche d'abord un squelette de
chargement, puis va lui-même chercher le contenu de la page en coulisses,
en interrogeant l'API du site (les mêmes routes JSON que celles utilisées
par une application mobile, par exemple). Une fois la réponse arrivée, le
squelette disparaît et la page réelle s'affiche. Ce détour permet de
garder le site public totalement découplé de l'administration : il ne
consomme que l'API, comme le ferait n'importe quel client externe.

## La partie « fabrication » : Vite

Comme toute application moderne écrite en Vue/TypeScript, le code du
frontend doit être transformé avant d'être utilisable par un navigateur.
Cette transformation est prise en charge par **Vite**, un outil de build :

| Commande | Ce qu'elle fait |
|---|---|
| `yarn dev` | Lance un serveur de développement avec rechargement instantané des modifications |
| `yarn build` | Vérifie les types TypeScript puis génère les fichiers finaux (mis en production) |

Les fichiers générés sont ensuite servis automatiquement dans les pages
Twig, sans configuration manuelle des URLs.

Voir [Commandes](commandes.md) pour la liste complète des commandes utiles,
et [Composants](composants/index.md) pour le détail de composants
transverses comme le [tableau Grid générique](composants/grid.md) ou
[l'éditeur Markdown](composants/editeur_markdown.md).
