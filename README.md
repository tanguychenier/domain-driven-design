# [Tansoftware](https://www.tansoftware.com) - Domain Driven Design [![fr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/France.png)](README.md)

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE) [![Lang](https://img.shields.io/badge/Lang-Français-005EB8.svg)](#) [![Topic](https://img.shields.io/badge/Topic-DDD-brightgreen.svg)](#) [![Made with Markdown](https://img.shields.io/badge/Made%20with-Markdown-1f425f.svg)](https://www.markdownguide.org/)

## Table des matières

1. [Fondations et cadrage](chapitres/01-fondations-et-cadrage.md)
   - [Introduction](chapitres/01-fondations-et-cadrage.md#introduction)
   - [Quand DDD ne vaut pas le coût](chapitres/01-fondations-et-cadrage.md#quand-ddd-ne-vaut-pas-le-coût)
   - [Glossaire express](chapitres/01-fondations-et-cadrage.md#glossaire-express)
2. [DDD stratégique : découper le domaine](chapitres/02-ddd-strategique-decouper-le-domaine.md)
   - [DDD stratégique vs DDD tactique](chapitres/02-ddd-strategique-decouper-le-domaine.md#ddd-stratégique-vs-ddd-tactique)
   - [Sous-domaines : core, supporting, generic](chapitres/02-ddd-strategique-decouper-le-domaine.md#sous-domaines-core-supporting-generic)
3. [Modélisation et langage ubiquitaire](chapitres/03-modelisation-et-langage-ubiquitaire.md)
   - [Modélisation du domaine](chapitres/03-modelisation-et-langage-ubiquitaire.md#modélisation-du-domaine)
   - [Langage ubiquitaire](chapitres/03-modelisation-et-langage-ubiquitaire.md#langage-ubiquitaire)
4. [Contextes délimités et cartographie](chapitres/04-contextes-delimites-et-cartographie.md)
   - [Bounded Contexts](chapitres/04-contextes-delimites-et-cartographie.md#bounded-contexts)
   - [Bounded Context Canvas](chapitres/04-contextes-delimites-et-cartographie.md#bounded-context-canvas)
   - [Context Map : les patterns de relation](chapitres/04-contextes-delimites-et-cartographie.md#context-map-les-patterns-de-relation)
5. [Briques tactiques : agrégats et services](chapitres/05-briques-tactiques-agregats-et-services.md)
   - [Entités, objets-valeurs et agrégats](chapitres/05-briques-tactiques-agregats-et-services.md#entités-objets-valeurs-et-agrégats)
   - [Règles de conception des agrégats (Vernon)](chapitres/05-briques-tactiques-agregats-et-services.md#règles-de-conception-des-agrégats-vernon)
   - [Frontières d'agrégats : un choix de conception, pas une vérité](chapitres/05-briques-tactiques-agregats-et-services.md#frontières-dagrégats-un-choix-de-conception-pas-une-vérité)
   - [Factories et Modules](chapitres/05-briques-tactiques-agregats-et-services.md#factories-et-modules)
   - [Repositories et Domain Services](chapitres/05-briques-tactiques-agregats-et-services.md#repositories-et-domain-services)
6. [CQRS, événements et fiabilité](chapitres/06-cqrs-evenements-et-fiabilite.md)
   - [Application Services et CQRS](chapitres/06-cqrs-evenements-et-fiabilite.md#application-services-et-cqrs)
   - [CQRS et Event Sourcing : indépendants](chapitres/06-cqrs-evenements-et-fiabilite.md#cqrs-et-event-sourcing-indépendants)
   - [Événements de domaine et Event Sourcing](chapitres/06-cqrs-evenements-et-fiabilite.md#événements-de-domaine-et-event-sourcing)
   - [Outbox Pattern : publication fiable des événements](chapitres/06-cqrs-evenements-et-fiabilite.md#outbox-pattern-publication-fiable-des-événements)
   - [Sagas et Process Managers](chapitres/06-cqrs-evenements-et-fiabilite.md#sagas-et-process-managers)
   - [Cohérence à terme : compensations et garanties](chapitres/06-cqrs-evenements-et-fiabilite.md#cohérence-à-terme-compensations-et-garanties)
   - [Domain Events vs Integration Events](chapitres/06-cqrs-evenements-et-fiabilite.md#domain-events-vs-integration-events)
7. [Patterns d'intégration et approche fonctionnelle](chapitres/07-patterns-dintegration-et-approche-fonctionnelle.md)
   - [Anti-Corruption Layer](chapitres/07-patterns-dintegration-et-approche-fonctionnelle.md#anti-corruption-layer)
   - [Specification Pattern](chapitres/07-patterns-dintegration-et-approche-fonctionnelle.md#specification-pattern)
   - [DDD fonctionnel : modéliser sans objets](chapitres/07-patterns-dintegration-et-approche-fonctionnelle.md#ddd-fonctionnel-modéliser-sans-objets)
8. [Mise en pratique et mises en garde](chapitres/08-mise-en-pratique-et-mises-en-garde.md)
   - [Exemple intégré : e-commerce multi-contextes](chapitres/08-mise-en-pratique-et-mises-en-garde.md#exemple-intégré-e-commerce-multi-contextes)
   - [Pièges classiques](chapitres/08-mise-en-pratique-et-mises-en-garde.md#pièges-classiques)
   - [Le côté obscur : DDD comme jeu de vocabulaire](chapitres/08-mise-en-pratique-et-mises-en-garde.md#le-côté-obscur-ddd-comme-jeu-de-vocabulaire)
   - [DDD et architectures voisines](chapitres/08-mise-en-pratique-et-mises-en-garde.md#ddd-et-architectures-voisines)
   - [Monolithe modulaire vs microservices](chapitres/08-mise-en-pratique-et-mises-en-garde.md#monolithe-modulaire-vs-microservices)

---

## Pour aller plus loin

- *Domain-Driven Design: Tackling Complexity in the Heart of Software*, Eric Evans (le « livre rouge », 2003)
- *Implementing Domain-Driven Design*, Vaughn Vernon (le « livre jaune », 2013)
- *Domain-Driven Design Distilled*, Vaughn Vernon (2016) : synthèse courte et accessible
- *Domain Modeling Made Functional*, Scott Wlaschin (2018) : DDD en F#, *parse don't validate*, types-sommes
- *Patterns, Principles, and Practices of Domain-Driven Design*, Scott Millett et Nick Tune (2015)
- *Learning Domain-Driven Design*, Vlad Khononov (O'Reilly, 2021) : moderne, pragmatique
- *Microservices Patterns*, Chris Richardson (Manning, 2018) : *Saga*, *Outbox*, *CDC*
- *CQRS Documents by Greg Young* : synthèse historique de CQRS et Event Sourcing
- *Building Event-Driven Microservices*, Adam Bellemare (O'Reilly, 2020)
- *Designing Data-Intensive Applications*, Martin Kleppmann (O'Reilly, 2017) : fondations événementielles, cohérence, réplication
- [DDD Reference (PDF)](https://www.domainlanguage.com/wp-content/uploads/2016/05/DDD_Reference_2015-03.pdf) : synthèse officielle d'Eric Evans
- [DDD Crew](https://github.com/ddd-crew) : outils, *Bounded Context Canvas* (Nick Tune), *Aggregate Design Canvas*, *Core Domain Chart*
- [Bounded Context Canvas (template)](https://github.com/ddd-crew/bounded-context-canvas) : Nick Tune
- [EventStorming](https://www.eventstorming.com/) : méthode collaborative d'Alberto Brandolini
- [Modular Monolith Primer](https://martinfowler.com/articles/2024-evaluate-architecture.html) : Martin Fowler et al.
- [Microsoft, CQRS Journey](https://learn.microsoft.com/en-us/previous-versions/msp-n-p/jj554200(v=pandp.10)) : retour d'expérience approfondi sur CQRS et ES

## Licence

Distribué sous licence [MIT](LICENSE).

## Auteur

**Tansoftware - Tanguy Chénier** · [LinkedIn](https://www.linkedin.com/in/tanguy-chenier) · [Tan-Software](https://github.com/Tan-Software) · [Compte personnel (derniers outils)](https://github.com/tanguychenier) · [tansoftware.com](https://www.tansoftware.com)
