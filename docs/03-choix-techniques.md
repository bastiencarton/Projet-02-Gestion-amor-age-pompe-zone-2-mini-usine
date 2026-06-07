---
layout: default
title: Choix techniques
nav_order: 4
---

# Choix techniques

Cette partie présente les principaux choix techniques réalisés pour le projet d’autoamorçage de pompe.  
L’objectif était de proposer une solution fonctionnelle, simple à comprendre, fiable et adaptable à l’installation existante.

## Vue d’ensemble de la solution retenue

Le système repose sur une installation existante, complétée par plusieurs éléments ajoutés afin d’assurer l’amorçage de la pompe principale.

L’installation existante comprend une pompe principale, appelée **P1**, qui permet d’alimenter l’installation depuis la cuve principale.

La solution ajoutée comprend :

- un réservoir secondaire **R1** ;
- une pompe auxiliaire auto-amorçante **P2** ;
- un capteur de niveau **C1** ;
- un clapet anti-retour **C2** ;
- une carte **ESP32** ;
- un relais de commande ;
- une alimentation adaptée à la partie commande.

L’idée générale est de remplir le réservoir secondaire afin de disposer d’une réserve d’eau permettant de maintenir la pompe principale amorcée.

---

## Choix du réservoir secondaire R1

Le réservoir secondaire **R1** a été ajouté pour servir de réserve d’eau d’amorçage.

Ce choix permet de ne pas dépendre uniquement de l’état de la conduite d’aspiration. Le réservoir conserve une quantité d’eau disponible pour aider au maintien de l’amorçage de la pompe principale.

Ce réservoir rend le système plus fiable, car il permet d’avoir une réserve locale directement proche de la pompe.

Le réservoir R1 présente donc plusieurs intérêts :

- conserver une réserve d’eau disponible ;
- faciliter le maintien de l’amorçage ;
- limiter les risques de désamorçage de la pompe principale ;
- rendre le fonctionnement plus autonome.

---

## Choix de la pompe auxiliaire P2

Une pompe auxiliaire **P2** a été ajoutée afin de remplir le réservoir secondaire R1.

Le choix d’une pompe auto-amorçante est important, car elle est capable de commencer à aspirer l’eau sans nécessiter un amorçage aussi contraignant qu’une pompe classique.

La pompe P2 n’a pas pour rôle d’alimenter directement toute l’installation. Son rôle est uniquement de remplir le réservoir secondaire lorsque le niveau d’eau est insuffisant.

Ce choix permet de séparer les fonctions :

- **P1** assure le fonctionnement principal de l’installation ;
- **P2** assure le remplissage du réservoir d’amorçage ;
- **R1** assure la réserve d’eau nécessaire au maintien de l’amorçage.

Cette séparation rend le système plus clair et plus facile à contrôler.

---

## Choix du capteur de niveau C1

Le capteur de niveau **C1** permet de détecter si le réservoir R1 contient suffisamment d’eau.

Nous avons choisi un capteur de niveau de type contact, car il est simple à utiliser avec une carte ESP32. Son état peut être lu comme une entrée logique : soit le niveau est détecté, soit il ne l’est pas.

Ce choix est adapté au projet, car nous n’avons pas besoin de mesurer précisément la hauteur d’eau. Nous avons seulement besoin de savoir si le niveau est suffisant pour autoriser ou non certaines actions.

Le capteur C1 permet donc :

- de surveiller le niveau dans le réservoir R1 ;
- d’éviter le fonctionnement inutile de la pompe auxiliaire ;
- de sécuriser le système ;
- d’envoyer une information simple à traiter par le programme.

---

## Choix du clapet anti-retour C2

Un clapet anti-retour **C2** a été ajouté sur la conduite d’aspiration.

Son rôle est d’empêcher l’eau de repartir dans le mauvais sens lorsque la pompe s’arrête. Ce composant est essentiel pour conserver l’eau dans la conduite et limiter les risques de désamorçage.

Ce choix est important car, sans clapet anti-retour, l’eau pourrait redescendre vers la cuve principale après l’arrêt de la pompe. La pompe principale aurait alors plus de difficulté à redémarrer correctement.

Le clapet C2 permet donc :

- de maintenir l’eau dans la conduite ;
- de limiter les pertes d’amorçage ;
- de protéger le fonctionnement de la pompe principale ;
- d’améliorer la fiabilité générale du système.

---

## Choix de la carte ESP32

La carte **ESP32** a été choisie pour piloter la partie commande du système.

Elle permet de lire l’état du capteur de niveau, de commander un relais et de communiquer avec d’autres équipements si nécessaire.

L’ESP32 est intéressante pour ce projet car elle possède plusieurs entrées/sorties numériques, une bonne capacité de programmation et des possibilités de communication.

Dans notre projet, elle permet principalement de :

- lire l’état du capteur de niveau C1 ;
- commander la pompe auxiliaire P2 via un relais ;
- envoyer ou recevoir des informations liées au fonctionnement ;
- faciliter les essais et les modifications du programme.

Ce choix est aussi pratique car la carte est facilement programmable avec l’environnement Arduino.

---

## Choix de la commande par relais

La pompe ne peut pas être commandée directement par l’ESP32, car la carte ne peut fournir qu’un faible courant en sortie.

Nous avons donc utilisé un relais. Le relais joue le rôle d’un interrupteur commandé électriquement.

L’ESP32 commande le relais avec un signal de faible puissance, puis le relais permet d’alimenter ou de couper la pompe.

Ce choix permet de séparer :

- la partie commande, avec l’ESP32 ;
- la partie puissance, avec l’alimentation de la pompe.

Cette séparation est importante pour protéger la carte électronique et rendre l’installation plus sûre.

---

## Choix de l’alimentation

L’installation utilise une alimentation issue du réseau électrique en **220 V monophasé**.

Une conversion est ensuite réalisée vers une tension plus faible, notamment pour alimenter les éléments de commande comme l’ESP32, les relais ou certains capteurs.

Ce choix permet d’avoir une alimentation adaptée à chaque partie du système :

- le 220 V pour la partie puissance ;
- le 24 V pour certains éléments de commande ;
- une tension adaptée à l’ESP32 pour la partie électronique.

L’objectif est d’éviter d’alimenter tous les composants avec la même tension, car ils n’ont pas les mêmes besoins électriques.

---

## Choix de la communication avec l’automate

Le système peut communiquer avec l’automate afin d’échanger des informations sur l’état du système.

Cette communication permet par exemple de transmettre l’état du capteur de niveau ou l’état de fonctionnement de la pompe.

L’intérêt de cette communication est de rendre le système compatible avec l’installation existante et de permettre une supervision depuis l’automate.

Cela permet également de mieux intégrer notre solution dans un environnement industriel.

---

## Bilan des choix techniques

Les choix techniques réalisés ont été guidés par trois objectifs principaux :

- rendre le système fonctionnel ;
- limiter les modifications sur l’installation existante ;
- assurer une solution compréhensible et sécurisée.

Le système retenu repose donc sur une pompe principale existante, complétée par une pompe auxiliaire, un réservoir secondaire, un capteur de niveau et une commande par ESP32.

Cette architecture permet d’obtenir une solution simple, progressive et adaptée à un projet de prototypage en MakerSpace.
