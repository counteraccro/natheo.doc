---
title: "Référence technique"
parent: "FAQ"
grand_parent: "Guide d'administration"
nav_order: 2
---

Vue d'ensemble technique du module FAQ : tables, classes backend/frontend et
routes. Pour l'usage côté back-office, voir le [listing](listing.md) et la
[création/édition d'une FAQ](ajouter_editer.md).

## Tables

Une FAQ est composée de 3 niveaux : la FAQ elle-même, ses catégories, et les
questions de chaque catégorie — chacun avec sa propre table de traduction.

### `faq`

| Colonne | Type | Description |
|---|---|---|
| `id` | int, PK | Identifiant |
| `user_id` | int, FK → `user.id`, NOT NULL | Auteur de la FAQ |
| `disabled` | bool | Visibilité de la FAQ (voir le [listing](listing.md)) |
| `created_at` | datetime | Date de création |
| `update_at` | datetime, nullable | Date de dernière modification |

### `faq_translation`

| Colonne | Type | Description |
|---|---|---|
| `id` | int, PK | Identifiant |
| `faq_id` | int, FK → `faq.id`, NOT NULL | FAQ parente |
| `locale` | varchar(10) | Langue (`fr`, `en`, `es`) |
| `title` | text | Titre de la FAQ dans cette langue |

### `faq_category`

| Colonne | Type | Description |
|---|---|---|
| `id` | int, PK | Identifiant |
| `faq_id` | int, FK → `faq.id`, NOT NULL | FAQ parente |
| `disabled` | bool | Visibilité de la catégorie |
| `render_order` | int | Position d'affichage parmi les catégories de la FAQ |

### `faq_category_translation`

| Colonne | Type | Description |
|---|---|---|
| `id` | int, PK | Identifiant |
| `faq_category_id` | int, FK → `faq_category.id`, NOT NULL | Catégorie parente |
| `locale` | varchar(10) | Langue |
| `title` | text | Titre de la catégorie dans cette langue |

### `faq_question`

| Colonne | Type | Description |
|---|---|---|
| `id` | int, PK | Identifiant |
| `faq_category_id` | int, FK → `faq_category.id`, NOT NULL | Catégorie parente |
| `disabled` | bool | Visibilité de la question |
| `render_order` | int | Position d'affichage parmi les questions de la catégorie |

### `faq_question_translation`

| Colonne | Type | Description |
|---|---|---|
| `id` | int, PK | Identifiant |
| `faq_question_id` | int, FK → `faq_question.id`, NOT NULL | Question parente |
| `locale` | varchar(10) | Langue |
| `title` | text | Intitulé de la question dans cette langue |
| `answer` | text | Réponse (Markdown) dans cette langue |

### `faq_statistique`

| Colonne | Type | Description |
|---|---|---|
| `id` | int, PK | Identifiant |
| `faq_id` | int, FK → `faq.id`, NOT NULL | FAQ parente |
| `key` | varchar(255) | Clé de la statistique (`KEY_STAT_NB_CATEGORIES`, `KEY_STAT_NB_QUESTIONS`) |
| `value` | varchar(255) | Valeur, recalculée à chaque sauvegarde de la FAQ |
| `created_at` | datetime | Date de création |
| `update_at` | datetime, nullable | Date de dernière modification |

Ces deux statistiques sont ce qui alimente les colonnes « Nombre de
catégories » et « Nombre de questions » du [listing](listing.md) ; elles sont
recalculées par `FaqController::save()` à chaque sauvegarde plutôt que
comptées à la volée. Voir le
[modèle de données complet](../../../Architecture/modele_donnees.md) pour la
vue d'ensemble de toutes les tables du CMS.

## Backend

| Classe | Rôle |
|---|---|
| [`Faq`](https://github.com/counteraccro/natheo/blob/master/src/Entity/Admin/Content/Faq/Faq.php) | Entité Doctrine (`faq`) |
| [`FaqCategory`](https://github.com/counteraccro/natheo/blob/master/src/Entity/Admin/Content/Faq/FaqCategory.php) | Entité Doctrine (`faq_category`) |
| [`FaqQuestion`](https://github.com/counteraccro/natheo/blob/master/src/Entity/Admin/Content/Faq/FaqQuestion.php) | Entité Doctrine (`faq_question`) |
| [`FaqStatistique`](https://github.com/counteraccro/natheo/blob/master/src/Entity/Admin/Content/Faq/FaqStatistique.php) | Entité Doctrine (`faq_statistique`) |
| [`FaqRepository`](https://github.com/counteraccro/natheo/blob/master/src/Repository/Admin/Content/Faq/FaqRepository.php) | Requêtes : pagination/tri/recherche (`getAllPaginate`), liste `id => titre` (`getListeFaq`, utilisée par l'éditeur de pages, voir plus bas). Une méthode `search()` existe aussi (recherche transverse FAQ/catégories/questions) mais n'est appelée depuis aucune route actuellement |
| [`FaqService`](https://github.com/counteraccro/natheo/blob/master/src/Service/Admin/Content/Faq/FaqService.php) | Logique métier : formatage pour le Grid, mise à jour des statistiques (`updateFaqStatistique`) |
| [`FaqController`](https://github.com/counteraccro/natheo/blob/master/src/Controller/Admin/Content/FaqController.php) | Routes admin (`/admin/{_locale}/faq/...`) |
| [`FaqFactory`](https://github.com/counteraccro/natheo/blob/master/src/Utils/Content/Faq/FaqFactory.php) | Construit une FAQ vide avec sa première catégorie et sa première question, dans toutes les locales du site |
| [`FaqPopulate`](https://github.com/counteraccro/natheo/blob/master/src/Utils/Content/Faq/FaqPopulate.php) | Reconstruit l'arbre catégories/questions à partir du tableau reçu du composant Vue (efface puis recrée les catégories à chaque sauvegarde) |
| [`FaqConst`](https://github.com/counteraccro/natheo/blob/master/src/Utils/Content/Faq/FaqConst.php) | Constantes des actions sur les statistiques (`add`, `sub`, `overwrite`) |
| [`FaqStatistiqueKey`](https://github.com/counteraccro/natheo/blob/master/src/Utils/Content/Faq/FaqStatistiqueKey.php) | Clés des statistiques (`KEY_STAT_NB_CATEGORIES`, `KEY_STAT_NB_QUESTIONS`) |
| [`FaqTranslate`](https://github.com/counteraccro/natheo/blob/master/src/Utils/Translate/Content/FaqTranslate.php) | Construit le tableau de traductions passé aux composants Vue `NewFaq` et `EditFaq` |

Le controller est protégé par `#[IsGranted('ROLE_CONTRIBUTEUR')]` (voir la
hiérarchie des rôles dans `config/packages/security.yaml`).

`FaqPopulate::populate()` ne fait pas un merge fin champ par champ sur les
catégories/questions existantes : à chaque sauvegarde, il **vide puis
reconstruit entièrement** `Faq::$faqCategories` (et, en cascade, leurs
questions) à partir du tableau reçu du front — `orphanRemoval: true` sur ces
relations se charge de supprimer en base les catégories/questions qui ne
sont plus présentes.

### Routes principales

| Route | Méthode | Description |
|---|---|---|
| `admin_faq_index` | GET | Page de listing |
| `admin_faq_load_grid_data` | GET | Données paginées du Grid (ajax) |
| `admin_faq_add` / `admin_faq_update` | GET | Formulaire de création / écran d'édition complet |
| `admin_faq_new_faq` | POST | Crée une FAQ à partir d'un simple titre (ajax) |
| `admin_faq_load_faq` | GET | Charge les données complètes d'une FAQ (ajax) |
| `admin_faq_save` | PUT | Sauvegarde une FAQ (catégories et questions incluses, ajax) |
| `admin_faq_disabled` | PUT | Active/désactive une FAQ (ajax) |
| `admin_faq_delete` | DELETE | Supprime une FAQ (ajax) |

## Fixtures

| | |
|---|---|
| Fichier de données | `src/DataFixtures/data/content/faq/faq_fixtures_data.yaml` |
| Classe de fixture | [`FaqFixtures`](https://github.com/counteraccro/natheo/blob/master/src/DataFixtures/Admin/Content/Faq/FaqFixtures.php) |
| Groupes | `faq`, `content` |

Charger uniquement les fixtures de FAQ :

```bash
php bin/console doctrine:fixtures:load --group=faq
```

Le fichier YAML imbrique les 3 niveaux (une FAQ référence ses catégories, qui
référencent elles-mêmes leurs questions) :

```yaml
faq:
    faq-1:
        user: User
        disabled: false
        faqTranslation:
            fr:
                locale: fr
                title: 'FAQ de démonstration'
        faqStatistique:
            FAQ_STAT_NB_CATEGORIES:
                key: KEY_STAT_NB_CATEGORIES
                value: '2'
        faqCategory:
            category-1:
                disabled: false
                renderOrder: 1
                faqCategoryTranslation:
                    fr:
                        locale: fr
                        title: 'Curvique fronte'
                faqQuestion:
                    category-1-q-1:
                        disabled: false
                        renderOrder: 1
                        faqQuestionTranslation:
                            fr:
                                locale: fr
                                title: 'Pati utque in petuntur'
                                answer: |
                                    # Elusaque facit tibi illa
                                    (contenu Markdown tronqué ici pour l'exemple)
```

### Dépendance croisée : l'éditeur de pages

`FaqRepository::getListeFaq()` ne sert pas qu'au listing : elle alimente
aussi la liste déroulante de sélection d'une FAQ dans l'onglet **Contenu**
du formulaire d'édition d'une page (`PageService::getListeContentByType()`,
type `PageContentType::FAQ`), qui permet d'insérer une FAQ existante comme
[bloc de contenu sur une page du site](../Pages/contenu.md) — au même titre
qu'un bloc de texte (`PageContentType::TEXT`) ou un bloc listing
(`PageContentType::LISTING`).

## Frontend

| Composant | Rôle |
|---|---|
| `Admin/Content/Faq/Faq` (`assets/vue/controllers/Admin/Content/Faq/Faq.vue`) | Point d'entrée, monté via `vue_component()` ; affiche `NewFaq` (création) ou `EditFaq` (édition) selon la présence d'un id |
| `NewFaq` (`assets/vue/Components/Faq/NewFaq.vue`) | Formulaire minimal de création (titre) |
| `EditFaq` (`assets/vue/Components/Faq/EditFaq.vue`) | Écran de construction complet : catégories/questions, glisser-déposer, validation |
| `Admin/GenericGrid` | Composant Grid générique réutilisé pour le listing — voir [Tableau GRID](../../../Architecture/composants/grid.md) |
| `MarkdownEditor` | Éditeur utilisé pour la réponse d'une question — voir [Éditeur Markdown](../../Modules/editeur_markdown.md) |

Le glisser-déposer (catégories entre elles, questions au sein d'une catégorie
ou d'une catégorie à une autre) est géré par
[SortableJS](https://github.com/SortableJS/Sortable), initialisé côté client
dans `EditFaq.vue` (`loadDraggableCategories` / `loadDraggableQuestions`) ;
`renderOrder` est recalculé localement à chaque réordonnancement et n'est
persisté qu'à la sauvegarde de la FAQ.

## Traductions

Toutes les chaînes (labels, messages de confirmation, aide) sont dans le
domaine de traduction `faq` : `translations/faq+intl-icu.{fr,en,es}.yaml`.

## Tests

| Fichier | Contenu |
|---|---|
| [`FaqServiceTest`](https://github.com/counteraccro/natheo/blob/master/tests/Service/Admin/Content/Faq/FaqServiceTest.php) | Tests du service (Grid, statistiques) |
| [`FaqFactoryTest`](https://github.com/counteraccro/natheo/blob/master/tests/Utils/Content/Faq/FaqFactoryTest.php) | Tests de la création d'une FAQ vide |
| [`FaqPopulateTest`](https://github.com/counteraccro/natheo/blob/master/tests/Utils/Content/Faq/FaqPopulateTest.php) | Tests de la reconstruction de l'arbre catégories/questions |
| [`FaqControllerTest`](https://github.com/counteraccro/natheo/blob/master/tests/Controller/Admin/Content/FaqControllerTest.php) | Tests des routes du contrôleur admin |
| `FaqFixturesTrait` (`tests/Helper/Fixtures/Content/FaqFixturesTrait.php`) | Aide de test pour créer des FAQ factices |
