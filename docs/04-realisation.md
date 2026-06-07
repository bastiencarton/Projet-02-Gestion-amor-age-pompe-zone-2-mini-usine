---

layout: default
title: Réalisation
nav_order: 5
---

# Réalisation

Cette partie présente la réalisation concrète du système d’autoamorçage de pompe. Elle décrit les éléments installés, leur rôle dans le montage, ainsi que la manière dont ils ont été intégrés à l’installation existante.

Le projet a été réalisé à partir d’une installation déjà présente sur la mini-usine. Cette installation comportait une pompe principale **P1**, utilisée pour alimenter la zone depuis la cuve principale. Notre travail a consisté à ajouter un système complémentaire permettant de maintenir l’amorçage de cette pompe.

---

## Installation existante

L’installation de départ était composée d’une cuve principale, d’une conduite d’aspiration, d’une pompe principale **P1** et d’une conduite de refoulement vers l’installation.

Le problème principal venait du risque de désamorçage de la pompe. Lorsqu’une pompe se désamorce, elle n’arrive plus correctement à aspirer l’eau. Cela peut empêcher le bon fonctionnement de l’installation et nécessiter une intervention manuelle.

L’objectif de notre réalisation était donc d’ajouter un système permettant de conserver une réserve d’eau disponible afin de faciliter le maintien de l’amorçage de la pompe principale.

---

## Principe de la solution ajoutée

La solution retenue repose sur l’ajout d’un système complémentaire autour de l’installation existante.

Les principaux éléments ajoutés sont :

* un réservoir secondaire **R1** ;
* une pompe auxiliaire auto-amorçante **P2** ;
* un capteur de niveau **C1** ;
* un clapet anti-retour **C2** ;
* une carte **ESP32** ;
* un relais de commande ;
* une alimentation adaptée à la partie commande.

Le réservoir **R1** sert de réserve d’eau d’amorçage. La pompe auxiliaire **P2** permet de remplir ce réservoir lorsque son niveau est insuffisant. Le capteur **C1** surveille le niveau d’eau dans le réservoir. Le clapet anti-retour **C2** permet d’éviter que l’eau ne reparte dans le mauvais sens lorsque la pompe s’arrête.

L’ensemble permet de conserver une réserve d’eau disponible pour maintenir la pompe principale **P1** amorcée.

---

## Réalisation hydraulique

La première partie de la réalisation concerne l’ajout des éléments hydrauliques.

Nous avons intégré un réservoir secondaire **R1** à proximité de l’installation. Ce réservoir permet de stocker une quantité d’eau dédiée à l’amorçage de la pompe principale.

Une pompe auxiliaire **P2** a été ajoutée afin de remplir ce réservoir. Son rôle n’est pas d’alimenter directement toute l’installation, mais uniquement d’assurer le remplissage du réservoir d’amorçage.

Un clapet anti-retour **C2** a également été ajouté sur la conduite d’aspiration. Il permet d’empêcher l’eau de repartir vers la cuve principale lorsque la pompe est arrêtée. Ce composant participe au maintien de l’eau dans la conduite et limite le risque de désamorçage.

Ajouter ici une image du schéma hydraulique :

![Schéma hydraulique](assets/images/schema-hydraulique.png)

Ajouter ici une photo du montage hydraulique :

![Montage hydraulique](assets/images/montage-hydraulique.jpg)

---

## Installation du capteur de niveau C1

Le capteur de niveau **C1** a été installé dans le réservoir secondaire **R1**.

Son rôle est de détecter si le niveau d’eau est suffisant. Le capteur fonctionne comme un contact électrique : selon la position du flotteur, le contact est ouvert ou fermé.

Ce type de capteur est adapté à notre besoin, car il n’est pas nécessaire de mesurer précisément la hauteur d’eau. Nous avons seulement besoin de savoir si le niveau est suffisant ou non pour commander la pompe auxiliaire.

Le capteur est relié à une entrée numérique de l’ESP32. L’état lu par la carte permet ensuite au programme de décider si la pompe auxiliaire doit être activée ou arrêtée.

Ajouter ici une photo du capteur de niveau :

![Capteur de niveau C1](assets/images/capteur-niveau-c1.jpg)

---

## Réalisation électrique

La réalisation électrique a consisté à alimenter correctement le système et à organiser la partie commande.

Nous avons tiré une alimentation depuis l’armoire principale en **220 V monophasé**. Cette alimentation arrive dans un coffret dédié à notre système.

Dans ce coffret, nous avons installé les différents éléments nécessaires au fonctionnement de la commande :

* un transformateur 220 V vers 24 V ;
* une alimentation adaptée à la carte ESP32 ;
* la carte ESP32 ;
* une carte relais ;
* des borniers de raccordement ;
* les connexions vers le capteur de niveau ;
* les connexions vers la pompe auxiliaire.

L’objectif était de séparer la partie puissance et la partie commande. La partie puissance concerne l’alimentation de la pompe, tandis que la partie commande concerne l’ESP32, le capteur de niveau et le relais.

Cette séparation est importante, car l’ESP32 ne peut pas commander directement une pompe. Elle ne peut fournir qu’un faible courant sur ses sorties. Le relais permet donc de faire le lien entre la carte de commande et l’alimentation de la pompe.

Ajouter ici une photo du coffret électrique :

![Coffret électrique](assets/images/coffret-electrique.jpg)

---

## Commande de la pompe par relais

La pompe auxiliaire **P2** est commandée à l’aide d’un relais.

Le relais fonctionne comme un interrupteur commandé électriquement. L’ESP32 envoie un signal de commande au relais, puis le relais autorise ou coupe l’alimentation de la pompe.

Dans notre montage, nous avons utilisé le contact **normalement ouvert** du relais. Cela signifie que, lorsque le relais n’est pas activé, la pompe n’est pas alimentée. Elle ne démarre que lorsque l’ESP32 active le relais.

Ce choix est plus sécurisant, car en cas d’arrêt de la commande ou de problème sur l’ESP32, la pompe reste coupée.

---

## Branchement du capteur sur l’ESP32

Le capteur de niveau est branché sur une entrée numérique de l’ESP32.

L’entrée est configurée avec l’instruction `INPUT_PULLUP`, ce qui permet d’utiliser la résistance de tirage interne de la carte. Cela permet d’obtenir une lecture stable de l’état du capteur.

Extrait simplifié du code utilisé pour lire le capteur :

```cpp
const int PIN_FLOTTEUR = 4;

void setup() {
  pinMode(PIN_FLOTTEUR, INPUT_PULLUP);
}

void loop() {
  int etat = digitalRead(PIN_FLOTTEUR);
}
```

Cette information est ensuite utilisée dans le programme pour commander la pompe auxiliaire.

---

## Programmation de l’ESP32

La carte ESP32 a été programmée avec l’environnement Arduino.

Le programme permet principalement de :

* lire l’état du capteur de niveau ;
* commander le relais de la pompe auxiliaire ;
* transmettre certaines informations à l’automate.

La logique générale du programme est la suivante :

```cpp
if (niveauDetecte == false) {
  // Le niveau est insuffisant
  // La pompe auxiliaire est activée
}
else {
  // Le niveau est suffisant
  // La pompe auxiliaire est arrêtée
}
```

Le programme a été volontairement conçu de manière simple afin de faciliter les essais, les corrections et les futures améliorations.

---

## Communication avec l’automate

Le système doit également communiquer avec l’automate de l’installation.

Cette communication permet de transmettre des informations utiles, comme l’état du capteur de niveau ou l’état de fonctionnement de la pompe auxiliaire.

L’objectif est d’intégrer notre système dans l’installation existante sans remplacer complètement l’automate. L’ESP32 gère la partie ajoutée liée à l’autoamorçage, tandis que l’automate conserve son rôle de supervision.

---

## Bilan de la réalisation

Cette réalisation nous a permis de mettre en place une solution complète autour de l’installation existante.

Nous avons travaillé sur plusieurs aspects :

* l’ajout des éléments hydrauliques ;
* la mise en place du réservoir secondaire ;
* l’installation du capteur de niveau ;
* le câblage électrique ;
* la commande de la pompe par relais ;
* la programmation de l’ESP32 ;
* l’intégration avec l’automate.

Cette étape correspond donc à la construction du système. Les essais permettant de vérifier son bon fonctionnement seront présentés dans la partie suivante, consacrée à la validation.
