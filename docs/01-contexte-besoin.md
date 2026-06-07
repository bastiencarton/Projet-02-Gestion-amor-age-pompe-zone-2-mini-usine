---

title: Contexte et besoin
layout: default
nav_order: 2
------------

# Contexte et besoin

## Contexte général du projet

Le projet s’inscrit dans le cadre de la Mini-Usine d’UniLaSalle Amiens. Cette installation reproduit une ligne de production industrielle dans laquelle des graviers sont transportés, lavés, séchés, chauffés puis conditionnés.

Notre projet concerne plus précisément la **zone de lavage des graviers**. À cette étape, les graviers passent sur un convoyeur et sont nettoyés à l’aide d’un jet d’eau sous pression. Cette pression est fournie par une pompe hydraulique située à proximité du tapis convoyeur. La pompe aspire l’eau depuis une cuve de stockage, puis l’envoie sous pression vers le système de nettoyage.

Cette partie de l’installation est essentielle, car elle permet d’éliminer les impuretés présentes sur les graviers avant les étapes de séchage, de chauffage et de conditionnement.

---

## Fonctionnement actuel

Dans l’installation actuelle, la pompe nécessite une phase d’amorçage avant son démarrage. Cette opération consiste à ajouter manuellement de l’eau dans le corps de pompe afin de permettre la création de l’aspiration.

Concrètement, avant chaque mise en route, un opérateur doit intervenir pour verser de l’eau dans un orifice prévu à cet effet. Cette action permet de remplir la pompe et d’éviter qu’elle ne fonctionne à sec au moment du démarrage.

Ce fonctionnement est simple, mais il dépend entièrement de l’intervention humaine. Il ne correspond donc pas totalement à l’objectif d’automatisation recherché dans une mini-usine.

---

## Problématique

La problématique principale du projet est la suivante :

**Comment automatiser l’amorçage de la pompe de lavage afin de fiabiliser le démarrage de l’installation et de limiter les interventions humaines ?**

Le fonctionnement actuel présente plusieurs limites :

* une intervention humaine est nécessaire avant chaque démarrage ;
* le temps de mise en route de l’installation est augmenté ;
* un oubli d’amorçage peut entraîner un fonctionnement à sec de la pompe ;
* un fonctionnement à sec peut provoquer une usure prématurée ou une détérioration du matériel ;
* le système manque d’autonomie et de fiabilité.

Dans une logique d’amélioration industrielle, il est donc nécessaire de rendre cette étape plus automatique, plus sûre et plus simple à superviser.

---

## Besoin fonctionnel

Le besoin principal est de mettre en place un système capable de rendre la pompe **automatiquement amorçable après un premier amorçage manuel**.

Le système doit permettre de constituer et de maintenir une réserve d’eau suffisante pour assurer les démarrages suivants de la pompe. Il doit également être capable de détecter les situations dans lesquelles le fonctionnement n’est pas possible, par exemple si le niveau d’eau est insuffisant.

L’objectif est de limiter les interventions de l’opérateur tout en garantissant un fonctionnement fiable de la pompe.

---

## Objectifs du système

Le système développé doit permettre de :

* automatiser l’amorçage de la pompe après le premier remplissage manuel ;
* maintenir une réserve d’eau disponible pour les prochains démarrages ;
* détecter un niveau d’eau insuffisant dans la cuve ou dans le système d’amorçage ;
* éviter le fonctionnement à sec de la pompe ;
* transmettre les informations de fonctionnement à l’automate ;
* permettre une supervision depuis l’interface HMI ;
* signaler les défauts ou les conditions empêchant le bon fonctionnement du système.

---

## Contraintes du projet

La solution proposée doit respecter plusieurs contraintes.

Tout d’abord, elle doit pouvoir s’intégrer à l’installation existante de la Mini-Usine. Il ne s’agit pas de remplacer entièrement le système de lavage, mais d’améliorer son fonctionnement en ajoutant une solution d’amorçage automatique.

Ensuite, le système doit communiquer avec l’automate de l’installation. Cette communication doit être réalisée sans fil afin de faciliter l’intégration dans l’environnement existant.

Le projet doit également prévoir une interaction avec l’interface HMI. Depuis cette interface, l’opérateur doit pouvoir superviser le système et effectuer certaines actions manuellement si nécessaire.

Enfin, la conception doit respecter une contrainte budgétaire maximale de **150 €** pour l’ensemble des composants nécessaires à la réalisation de la solution.

---

## Interface de supervision attendue

Une page dédiée au système d’amorçage doit être prévue sur l’interface HMI. Cette page doit permettre à l’opérateur de suivre l’état du système et d’agir en cas de besoin.

L’interface doit notamment permettre de visualiser :

* l’état du système d’amorçage ;
* le niveau de remplissage du réservoir d’amorçage ;
* l’état de fonctionnement de la pompe ;
* les alertes et défauts éventuels.

Elle doit également permettre certaines actions manuelles, comme :

* lancer un cycle d’amorçage ;
* forcer le remplissage du réservoir d’amorçage ;
* démarrer la pompe à distance si les conditions le permettent.

---

## Enjeu du projet

L’enjeu du projet est donc de passer d’un amorçage manuel, dépendant de l’opérateur, à un système plus autonome et plus fiable.

Cette amélioration doit permettre de réduire les risques d’oubli, de protéger la pompe contre un fonctionnement à sec et de rendre l’installation plus proche d’un fonctionnement industriel automatisé.

Le projet ne se limite donc pas à la mise en marche d’une pompe. Il s’agit de concevoir une solution complète intégrant la détection, la commande, la communication, la supervision et la sécurité de fonctionnement.
