---
title: "Traductions & multilingue"
parent: "Architecture"
nav_order: 4
---

Nathéo est multilingue à deux endroits bien distincts, qui ne se gèrent pas
du tout de la même façon dans le code : **le contenu** que l'on rédige dans
le CMS (une page, un tag, une question de FAQ...) et **les textes fixes**
de l'interface elle-même (les libellés de boutons, les messages d'aide...).

## Traduire le contenu

Chaque entité de contenu qui peut être traduite (`Page`, `Tag`, `Menu`,
`Faq`...) est en réalité composée de deux entités Doctrine : l'entité
« support » (qui porte les données communes à toutes les langues, comme la
couleur d'un tag) et une entité `*Translation` associée, qui porte les
champs traduisibles (le libellé, dans le cas d'un tag) — une ligne par
langue.

```php
// Tag.php — l'entité support
#[ORM\OneToMany(mappedBy: 'tag', targetEntity: TagTranslation::class, cascade: ['persist'], orphanRemoval: true)]
private Collection $tagTranslations;
```

```php
// TagTranslation.php — une traduction du tag, pour une langue donnée
#[ORM\Column(length: 10)]
private ?string $locale = null;

#[ORM\Column(length: 255)]
private ?string $label = null;
```

Ce même principe se retrouve, avec ses propres champs à chaque fois, sur
[`PageTranslation`](https://github.com/counteraccro/natheo/blob/master/src/Entity/Admin/Content/Page/PageTranslation.php),
[`MenuElementTranslation`](https://github.com/counteraccro/natheo/blob/master/src/Entity/Admin/Content/Menu/MenuElementTranslation.php),
[`FaqTranslation`](https://github.com/counteraccro/natheo/blob/master/src/Entity/Admin/Content/Faq/FaqTranslation.php)
ou encore [`MailTranslation`](https://github.com/counteraccro/natheo/blob/master/src/Entity/Admin/System/MailTranslation.php)
pour les modèles d'e-mails. Il n'existe pas d'interface ou de classe commune
qui factorise ce fonctionnement : chaque domaine a sa propre paire
entité/entité de traduction, mais toutes suivent la même logique.

Ces traductions se modifient directement depuis le formulaire du contenu
concerné (par exemple, [ajouter ou modifier un tag](../GuideAdmin/Contenu/Tags/ajouter_editer.md)
propose un champ par langue).

## Traduire l'interface

Les textes fixes du CMS (ceux qui ne dépendent d'aucun contenu rédigé) ne
sont pas en base de données, mais dans des fichiers YAML, un par zone
fonctionnelle et par langue, rangés dans `translations/` :

```
translations/tag+intl-icu.fr.yaml
translations/tag+intl-icu.en.yaml
translations/tag+intl-icu.es.yaml
```

Le suffixe `+intl-icu` indique au traducteur Symfony d'utiliser le format
ICU MessageFormat, qui gère proprement des règles comme le pluriel selon la
langue. Le nom qui précède (`tag`, `menu`, `dashboard`...) est le
**domaine** : chaque appel à `trans()` dans le code précise explicitement de
quel domaine il tire sa traduction, ce qui évite qu'un même mot-clé entre
deux zones fonctionnelles ne se marche dessus.

```php
$this->translator->trans('tag.form.submit.create', domain: 'tag');
```

### Nommage des clés

Une clé suit le format `<domaine>.<écran>.<élément>` :

- le nom du domaine (le nom du fichier) vient en premier ;
- tout est en minuscules, avec des points entre les niveaux et des tirets bas dans
  un nom (`grid.search.placeholder_table`) ;
- les niveaux sont en anglais ;
- une clé n'est jamais aussi le préfixe d'une autre : `dashboard.index.title`, et
  pas `dashboard.index` en plus de `dashboard.index.description`.

Les libellés courants (Annuler, Supprimer, Oui, Non...) ont une clé par domaine,
jamais une clé partagée : comme les traductions se modifient depuis le CMS, chaque
bouton doit pouvoir changer seul. Les domaines `security` et `validators` font
exception : ce sont des catalogues Symfony dont les clés sont les messages anglais.

### De Symfony jusqu'au composant Vue

Twig peut appeler `trans()` directement dans un template, mais côté Vue les
composants n'ont pas accès au traducteur Symfony : chaque zone fonctionnelle
a donc une petite classe `XxxTranslate` (héritant de
[`AppTranslate`](https://github.com/counteraccro/natheo/blob/master/src/Translate/AppTranslate.php))
qui traduit à l'avance tous les textes dont son composant aura besoin, et
les regroupe dans un tableau simple :

```php
// TagTranslate.php
'formInputColorHelp' => $this->translator->trans('tag.form.input.color.help', domain: 'tag'),
'btnSubmitCreate' => $this->translator->trans('tag.form.submit.create', domain: 'tag'),
```

Ce tableau est ensuite transmis tel quel au composant via la prop
`translate`, décrite dans [Architecture frontend](frontend.md#comment-un-bloc-vue-arrive-dans-une-page).

### Modifier ces textes sans toucher au code

Un écran dédié du back-office permet à un super-administrateur de parcourir
ces fichiers par langue puis par domaine, et d'en modifier les valeurs
directement — voir [Gestion des traductions](../GuideAdmin/Systeme/traductions.md)
dans le Guide d'administration.

## Langues prises en charge

Les langues actives sur le site sont définies une seule fois, dans
`config/services.yaml` :

```yaml
app.supported_locales: 'fr|en|es'
```

Le français reste la langue par défaut de l'application (`default_locale`
et langue de repli si une traduction venait à manquer dans une autre
langue), configuré dans `config/packages/translation.yaml`.

## Ajouter de nouvelles clés de traduction

Quand un développeur ajoute un nouvel appel à `trans()` dans le code, la
clé n'existe pas encore dans les fichiers YAML. La commande suivante
parcourt tout le code source et complète les fichiers des trois langues
avec les clés manquantes (sans écraser les traductions déjà saisies) :

```bash
php bin/console natheo:translations:update
```

Elle enchaîne simplement `translation:extract --force --format=yaml` pour
chacune des trois langues — voir [`UpdateTranslationsCommand`](https://github.com/counteraccro/natheo/blob/master/src/Command/UpdateTranslationsCommand.php)
et [Commandes Symfony](commandes.md).

L'interface est entièrement traduite dans les trois langues. Deux tests protègent
cet état :

| Test | Ce qu'il vérifie |
|---|---|
| [`TranslationKeyConventionTest`](https://github.com/counteraccro/natheo/blob/master/tests/Translation/TranslationKeyConventionTest.php) | Format et préfixe des clés, mêmes clés en français, anglais et espagnol |
| [`TranslationCompletenessTest`](https://github.com/counteraccro/natheo/blob/master/tests/Translation/TranslationCompletenessTest.php) | Aucune valeur laissée avec un marqueur `__`, mêmes paramètres ICU que le français |

Une clé ajoutée doit donc être traduite dans les trois langues avant de lancer les
tests.

## Voir aussi
- [Architecture frontend](frontend.md) *(la prop `translate` passée aux composants Vue)*
- [Gestion des traductions](../GuideAdmin/Systeme/traductions.md) *(éditer les textes de l'interface depuis le back-office)*
- [Commandes Symfony](commandes.md)
