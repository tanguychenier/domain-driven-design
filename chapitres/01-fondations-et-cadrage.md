[↑ Sommaire](../README.md#table-des-matières) · [DDD stratégique : découper le domaine →](02-ddd-strategique-decouper-le-domaine.md)

# 1. Fondations et cadrage

## Introduction

> **Que veut dire « DDD » ?** DDD est l'abréviation de *Domain-Driven Design*, en français « conception pilotée par le domaine ». Le mot *domaine* désigne le métier que le logiciel sert (la banque, la vente en ligne, la santé...). « Pilotée par le domaine » signifie que l'on construit le logiciel en partant des règles du métier, et non de la technique. Imaginez un architecte qui dessine une maison en partant des besoins de la famille qui va y vivre (combien d'enfants, qui cuisine, qui reçoit), plutôt qu'en partant du catalogue de briques du marchand de matériaux. Le DDD applique cette idée au logiciel : on écoute d'abord le métier, le code vient après.

Le *Domain-Driven Design* (DDD) a été formalisé par Eric Evans dans le livre [*Domain-Driven Design: Tackling Complexity in the Heart of Software*](https://www.amazon.fr/Domain-Driven-Design-Tackling-Complexity/dp/0321125215) (2003). Son idée centrale : placer la **complexité métier** au centre de la conception logicielle. Plutôt que de partir d'une base de données ou d'un cadre logiciel, on modélise les concepts, les règles et les processus du métier ; le code en découle.

> **Que veut dire « CRUD » ?** CRUD est l'acronyme anglais de *Create, Read, Update, Delete* (créer, lire, modifier, supprimer), les quatre opérations de base sur des données. Une application « CRUD pur », c'est une application qui ne fait quasiment que ces quatre choses sur des tables, sans règle compliquée. Comparez un carnet d'adresses (on ajoute, on lit, on change, on efface un contact : c'est du CRUD) à un logiciel de calcul d'impôts (qui applique des centaines de règles : ce n'est plus du CRUD).

> **Que veut dire « base de données » ?** Une base de données est l'endroit où le logiciel range durablement ses informations (les comptes, les commandes...) pour les retrouver plus tard, un peu comme un classeur géant rangé par tiroirs. Un *cadre logiciel* (en anglais *framework*) est une boîte à outils toute prête qui impose une façon de coder pour aller plus vite (Symfony, Django, Spring en sont des exemples).

Cette approche prend tout son intérêt dès qu'un projet dépasse le simple CRUD : règles métier nombreuses, plusieurs équipes, plusieurs sous-domaines. Elle apporte trois bénéfices principaux : un vocabulaire partagé entre métier et technique, une frontière claire entre sous-domaines, et un découplage du domaine vis-à-vis de l'infrastructure (le code métier ne dépend pas des détails techniques de stockage ou de réseau).

### Domaine, sous-domaines, *core domain*

- **Domaine** : le secteur d'activité que le logiciel sert (assurance, e-commerce, santé).
- **Sous-domaine** : une zone fonctionnelle du domaine. Trois natures :
  - *Core domain* (domaine cœur) : l'avantage compétitif, là où l'on doit investir.
  - *Supporting subdomain* (sous-domaine de soutien) : nécessaire mais non différenciant ; on peut le développer simplement.
  - *Generic subdomain* (sous-domaine générique) : résolu par n'importe quel acteur du marché (authentification, facturation standard) ; à acheter ou intégrer.

> **Que veut dire « avantage compétitif » ?** C'est ce qui fait qu'une entreprise gagne face à ses concurrents : la chose qu'elle fait mieux ou différemment des autres. Pour un restaurant, ce peut être une recette secrète. En logiciel, c'est la règle métier qui distingue l'entreprise, par exemple la formule de calcul de prix d'un assureur. On investit le plus d'efforts là.

- **Espace problème vs espace solution** : les sous-domaines décrivent le *problème* à résoudre ; les *bounded contexts* (frontières de modèle, définies plus loin) décrivent la *solution* logicielle. La correspondance entre les deux n'est pas forcément une pour une (un problème peut donner naissance à plusieurs morceaux de solution, et inversement).

[🔝 Retour en haut de page](#table-des-matières)

## Quand DDD ne vaut pas le coût

> **Le critère de complexité d'Evans.** Eric Evans lui-même, dans la préface de la *DDD Reference* (2015), rappelle que *« DDD est destiné aux domaines complexes, pas à toute application »*. La discipline a un **coût d'entrée** non négligeable (langage ubiquitaire à entretenir, ateliers métier, modélisation tactique, isolation de l'infrastructure) ; elle ne se rentabilise que là où la **complexité métier** dépasse la complexité technique.

> **Que veut dire « invariant » ?** Un invariant est une règle qui doit *toujours* rester vraie, quoi qu'il arrive. Exemple : « le solde d'un compte courant ne peut pas descendre sous le découvert autorisé ». Pensez à une règle de jeu de société qu'on ne peut jamais enfreindre : tant qu'elle tient, la partie reste valide.

### Cas où DDD est probablement surdimensionné

- **CRUD pur** : un panneau d'administration où chaque écran modifie directement les champs d'une table. Aucun invariant non trivial, aucun vocabulaire métier subtil. Un *Active Record* ou un générateur d'administration (Filament, Django Admin, Backpack) coûtera dix fois moins.

> **Que veut dire « Active Record » ?** C'est un style de programmation où chaque ligne de table en base de données correspond directement à un objet du code, qui sait se sauvegarder lui-même. Simple et rapide pour du CRUD, mais il mélange données et stockage, ce que le DDD cherche justement à séparer.

- **MVP exploratoire ou code jetable** : tant que le métier n'a pas confirmé qu'il y a un produit viable, figer un modèle riche revient à figer une mauvaise hypothèse. Préférer un prototypage rapide ; reconstruire en DDD une fois le marché validé.

> **Que veut dire « MVP » ?** MVP est l'acronyme de *Minimum Viable Product*, en français « produit minimum viable » : la plus petite version d'un produit qui permet déjà de tester si les clients en veulent. C'est la maquette qui roule, pas la voiture finale.

- **Sous-domaine générique** : authentification, paie standard, envoi de courriels, gestion documentaire générique. Ces zones se résolvent par achat ou intégration de services existants (Auth0, Stripe, stockage de fichiers en nuage) ; investir un modèle riche dessus est un gaspillage.
- **Outils internes à courte durée de vie** : scripts d'import-export de données (ETL), tableaux de bord éphémères, migrations ponctuelles. Le retour sur investissement d'une modélisation soignée n'est jamais atteint.

> **Que veut dire « ETL » ?** ETL est l'acronyme de *Extract, Transform, Load* (extraire, transformer, charger) : un programme qui prend des données d'un endroit, les remet en forme, puis les dépose ailleurs. C'est le tapis roulant qui déplace et nettoie les données d'un système à un autre.

- **Équipe sans expert métier disponible** : sans un connaisseur du métier accessible plusieurs heures par semaine, le langage ubiquitaire devient un vœu pieux. Mieux vaut un code honnêtement technique qu'un DDD de façade.

### Cas où DDD se justifie pleinement

- **Domaine cœur** d'une activité où la règle métier *est* l'avantage compétitif (tarification d'assurance, calcul d'un score de risque, répartition logistique, calcul réglementaire).
- **Vocabulaire métier riche et évolutif** : les experts emploient des dizaines de termes précis et distinguent des nuances que le code doit refléter sans perte.
- **Plusieurs équipes** : plusieurs équipes travaillent en même temps sur des morceaux du système et doivent communiquer sans casser leurs modèles respectifs.
- **Long terme** : application appelée à vivre dix ans ou plus, avec renouvellement des équipes. L'investissement dans un modèle explicite se rembourse en dette technique évitée.

> **Que veut dire « dette technique » ?** C'est l'accumulation de raccourcis et de mauvaises décisions dans le code, qui rendent chaque évolution future plus lente et plus risquée. Comme une dette d'argent : on gagne du temps maintenant, mais on paie des « intérêts » plus tard sous forme de bugs et de lenteur.

### Heuristique de décision

| Signal | Verdict |
|--------|---------|
| Le métier dit *« c'est juste un formulaire avec une liste »* | Pas DDD. CRUD assumé. |
| L'expert métier vous corrige sur 3 termes par réunion | DDD pertinent : langage ubiquitaire à formaliser. |
| Aucun invariant ne vaut plus qu'une validation de champ | Pas DDD tactique. Validation suffit. |
| Plusieurs règles s'entremêlent et changent ensemble | DDD tactique : agrégats, services domaine. |
| Une équipe, un service, un déploiement | Stratégique léger ; tactique au cas par cas. |
| Plusieurs équipes, contrats inter-services à stabiliser | Stratégique critique : contextes, ACL, OHS. |

> **Que veut dire « ACL » et « OHS » ?** Ce sont deux façons de relier des morceaux de système, détaillées plus loin. *ACL* (*Anti-Corruption Layer*, couche anti-corruption) est un traducteur qui empêche le vocabulaire d'un système voisin de polluer le vôtre. *OHS* (*Open Host Service*, service hôte ouvert) est un système qui publie un mode d'emploi stable que plusieurs autres peuvent utiliser.

> **Garde-fou.** L'erreur la plus commune n'est pas de *ne pas faire* de DDD : c'est d'en faire **partout de la même manière**. Distinguer cœur, soutien et générique permet d'investir le bon niveau d'effort au bon endroit, et de **ne pas surconcevoir** ce qui n'a aucune valeur différenciante.

[🔝 Retour en haut de page](#table-des-matières)

## Glossaire express

Voici un référentiel court des termes du vocabulaire DDD. Chaque concept est détaillé et illustré dans les sections suivantes.

| Terme | Définition |
|-------|------------|
| **Domaine** | Le secteur d'activité métier servi par le logiciel. |
| **Sous-domaine** | Une zone fonctionnelle du domaine (core, supporting, generic). |
| **Bounded Context** | Frontière explicite dans laquelle un modèle et un langage sont cohérents. |
| **Langage ubiquitaire** | Vocabulaire unique partagé par métier et technique au sein d'un contexte. |
| **Entité** | Objet défini par une **identité stable** dans le temps, indépendamment de ses attributs. |
| **Objet-valeur** (*Value Object*) | Objet **immuable** défini uniquement par ses attributs, sans identité propre. |
| **Agrégat** | Cluster d'entités et d'objets-valeurs traité comme une **unité de cohérence transactionnelle**. |
| **Racine d'agrégat** (*Aggregate Root*) | Entité unique servant de point d'entrée à l'agrégat, garante de ses invariants. |
| **Repository** | Abstraction de persistance offrant l'illusion d'une collection en mémoire d'agrégats. |
| **Factory** | Composant chargé de la **construction cohérente** d'un agrégat ou d'un objet-valeur complexe. |
| **Domain Service** | Logique métier sans foyer naturel dans une entité ou un objet-valeur ; vit dans la couche domaine. |
| **Application Service** | Orchestrateur de cas d'usage ; ouvre une transaction, appelle le domaine, persiste, publie. **Sans logique métier**. |
| **Module** | Regroupement nommé d'éléments du modèle, équivalent d'un *package* aligné sur le langage ubiquitaire. |
| **Context Map** | Cartographie explicite des relations entre Bounded Contexts. |
| **Anti-Corruption Layer** | Couche de traduction entre deux contextes, protégeant le modèle local. |
| **CQRS** | *Command Query Responsibility Segregation* : modèles distincts pour l'écriture et la lecture. |
| **Event Sourcing** | Persistance d'un agrégat sous forme de la **séquence d'événements** qui l'ont fait advenir. |
| **Eventual Consistency** | Cohérence asynchrone entre agrégats ou contextes ; toléré pour gagner en disponibilité. |
| **Saga / Process Manager** | Coordinateur d'un workflow métier long traversant plusieurs agrégats. |
| **Domain Event** | Fait métier passé, immuable, émis à l'intérieur d'un contexte. |
| **Integration Event** | Événement publié vers d'autres contextes ou services, sous contrat stable. |
| **Projection** | Logique qui consomme des événements pour construire un *read model*. |
| **Read Model** | Vue dénormalisée optimisée pour la lecture (côté queries). |
| **Write Model** | Modèle riche optimisé pour la cohérence et l'application des invariants (côté commands). |

[🔝 Retour en haut de page](#table-des-matières)

---

[↑ Sommaire](../README.md#table-des-matières) · [DDD stratégique : découper le domaine →](02-ddd-strategique-decouper-le-domaine.md)
