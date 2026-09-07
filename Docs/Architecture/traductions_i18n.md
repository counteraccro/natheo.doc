---
title: "Traductions & multilingue"
parent: "Architecture"
nav_order: 4
---

> 🚧 **Nouvelle page à écrire.** Elle distinguera les deux notions de traduction du
> projet :
>
> * **Contenu multilingue** : les entités de contenu (Page, Menu, Faq, Comment, ...)
>   stockent leurs données par langue via des entités enfants `*Translation`
>   (ex. `PageTranslation`, `MenuElementTranslation`)
> * **Traduction de l'interface** : les chaînes d'UI vivent dans
>   `translations/<domaine>+intl-icu.<locale>.yaml` (un domaine par zone
>   fonctionnelle), chargées via le traducteur Symfony avec un `domain:` explicite
>   sur chaque appel `trans()`
> * Langues d'interface supportées : `fr`, `en`, `es`
> * Commande d'extraction : `php bin/console translation:extract --force --format=yaml fr`
>   (voir aussi [Commandes Symfony](commandes.md))

*(à compléter)*
