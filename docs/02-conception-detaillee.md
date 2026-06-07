---

title: "Conception détaillée"
layout: default
nav_order: 3
------------

# Conception détaillée

## Introduction

Cette partie présente le fonctionnement général de la solution retenue pour automatiser l’amorçage de la pompe de lavage de la zone 2 de la mini-usine.

L’objectif est d’expliquer comment le système fonctionne dans son ensemble, depuis la réserve d’eau principale jusqu’à la pompe de lavage. Les choix techniques précis seront détaillés dans la partie suivante.

---

## Principe général du système

La solution retenue repose sur l’ajout d’un système complémentaire à l’installation existante.

L’installation initiale est conservée : la pompe principale continue d’assurer l’alimentation en eau du système de lavage des graviers. Cependant, afin d’éviter un amorçage manuel avant chaque démarrage, un système d’amorçage automatique est ajouté.

Le principe général est le suivant :

* la pompe principale P1 reste la pompe utilisée pour alimenter l’installation de lavage ;
* un réservoir secondaire R1 est ajouté pour stocker une réserve d’eau d’amorçage ;
* une pompe auxiliaire auto-amorçante P2 permet de remplir ce réservoir secondaire ;
* un capteur de niveau C1 surveille la présence d’eau dans le réservoir secondaire ;
* un clapet anti-retour C2 permet de maintenir l’eau dans le circuit et de limiter la perte d’amorçage.

Ainsi, après un premier amorçage manuel, le système doit être capable de conserver une réserve d’eau suffisante pour faciliter les démarrages suivants de la pompe principale.

---

## Séparation entre installation existante et système ajouté

Le système peut être séparé en deux parties.

La première partie correspond à l’installation existante. Elle comprend la cuve principale, la pompe principale P1 et le circuit alimentant l’installation de lavage. Cette partie permet d’envoyer l’eau sous pression vers le système de nettoyage des graviers.

La seconde partie correspond au système ajouté. Elle comprend la pompe auxiliaire P2, le réservoir secondaire R1, le capteur de niveau C1 et le clapet anti-retour C2. Cette partie a pour rôle de gérer l’amorçage et de maintenir une quantité d’eau disponible pour les démarrages suivants.

Cette séparation permet de conserver le fonctionnement de base de la mini-usine tout en ajoutant une fonction d’automatisation autour de la pompe.

---

## Rôle de la pompe principale P1

La pompe principale P1 est l’élément déjà présent sur l’installation. Elle permet d’aspirer l’eau depuis la cuve principale et de l’envoyer vers l’installation de lavage.

Cette pompe est indispensable au fonctionnement de la zone de lavage, car elle fournit la pression nécessaire au nettoyage des graviers.

Le problème rencontré est que cette pompe nécessite un amorçage pour démarrer correctement. Si elle démarre sans eau dans son circuit, elle peut fonctionner à sec, ce qui peut entraîner une perte d’efficacité ou une dégradation du matériel.

Dans notre solution, la pompe P1 conserve son rôle initial. Le système ajouté sert uniquement à améliorer ses conditions de démarrage.

---

## Rôle du réservoir secondaire R1

Le réservoir secondaire R1 sert de réserve d’eau d’amorçage.

Son rôle est de conserver une quantité d’eau disponible afin de permettre à la pompe principale de redémarrer dans de meilleures conditions. Au lieu de dépendre uniquement d’un amorçage manuel, le système dispose d’une réserve dédiée.

Ce réservoir est donc un élément central de la solution. Il permet de passer d’un amorçage entièrement manuel à un fonctionnement plus autonome.

Le niveau d’eau présent dans ce réservoir est surveillé par un capteur afin de s’assurer que la réserve est suffisante.

---

## Rôle de la pompe auxiliaire P2

La pompe auxiliaire P2 est une pompe auto-amorçante ajoutée au système.

Son rôle est de remplir le réservoir secondaire R1 à partir de la cuve principale. Elle permet donc de constituer ou de compléter la réserve d’eau d’amorçage.

Lorsque le niveau d’eau dans le réservoir secondaire devient insuffisant, le système peut commander la pompe auxiliaire afin de remplir à nouveau le réservoir.

Cette pompe ne remplace pas la pompe principale. Elle a un rôle spécifique : assurer le remplissage du réservoir d’amorçage.

---

## Rôle du capteur de niveau C1

Le capteur de niveau C1 est placé dans le réservoir secondaire R1.

Il permet de détecter si le niveau d’eau est suffisant ou non. Cette information est utilisée par la partie commande du système pour autoriser ou non certaines actions.

Par exemple, si le niveau d’eau est suffisant, le système peut considérer que les conditions d’amorçage sont correctes. À l’inverse, si le niveau est insuffisant, le système peut déclencher le remplissage du réservoir ou empêcher le démarrage de la pompe principale.

Le capteur de niveau permet donc d’éviter un fonctionnement sans réserve d’eau suffisante.

---

## Rôle du clapet anti-retour C2

Le clapet anti-retour C2 est ajouté sur le circuit hydraulique.

Son rôle est d’empêcher l’eau de repartir vers la cuve principale lorsque la pompe est arrêtée. Il permet donc de maintenir l’eau dans la partie utile du circuit et de limiter la perte d’amorçage.

Ce composant est important, car il contribue à conserver les conditions nécessaires au redémarrage de la pompe principale.

Sans clapet anti-retour, une partie de l’eau pourrait retourner vers la cuve principale, ce qui réduirait l’efficacité du système d’amorçage.

---

## Fonctionnement global du cycle

Le fonctionnement global peut être décrit en plusieurs étapes.

Dans un premier temps, l’installation nécessite un premier amorçage manuel. Cette étape permet de remplir le circuit initialement et de mettre le système dans des conditions de fonctionnement correctes.

Ensuite, le système ajouté prend le relais pour maintenir une réserve d’eau disponible. La pompe auxiliaire P2 peut remplir le réservoir secondaire R1 à partir de la cuve principale.

Le capteur de niveau C1 surveille ensuite le niveau dans le réservoir secondaire. Si le niveau est suffisant, le système considère que la réserve d’amorçage est disponible. Si le niveau est insuffisant, le système peut déclencher un remplissage ou signaler une condition de défaut.

Lors du démarrage suivant, la présence d’eau dans le réservoir secondaire et dans le circuit permet de faciliter l’amorçage de la pompe principale P1. Le clapet anti-retour C2 limite les pertes d’eau dans le circuit et aide à maintenir l’amorçage.

---

## Logique de commande

La logique de commande repose sur une carte ESP32.

L’ESP32 reçoit l’information du capteur de niveau C1. En fonction de cette information, elle peut commander le système, notamment la pompe auxiliaire ou le relais associé.

La logique générale peut être résumée ainsi :

* si le niveau d’eau dans R1 est suffisant, le système est prêt pour le fonctionnement ;
* si le niveau d’eau est insuffisant, le remplissage du réservoir doit être réalisé ;
* si le remplissage n’est pas possible ou si une condition anormale est détectée, une alerte doit être transmise ;
* les informations importantes sont communiquées à l’automate pour permettre la supervision.

Cette logique permet de rendre le système plus autonome tout en conservant une possibilité de contrôle par l’opérateur.

---

## Communication avec l’automate

Le système doit transmettre certaines informations à l’automate de la mini-usine.

Cette communication permet d’intégrer le système d’amorçage à l’environnement existant. L’automate peut ainsi recevoir des informations sur l’état du système et les afficher sur l’interface HMI.

Les informations transmises peuvent concerner :

* l’état du capteur de niveau ;
* l’état du réservoir secondaire ;
* l’état de fonctionnement de la pompe auxiliaire ;
* l’état général du système ;
* les défauts ou alertes éventuels.

Cette communication permet de ne pas avoir un système isolé, mais bien un système intégré à la supervision de la mini-usine.

---

## Supervision par l’interface HMI

L’interface HMI permet à l’opérateur de suivre le fonctionnement du système.

Elle doit afficher les informations principales, comme le niveau du réservoir secondaire, l’état de la pompe et les éventuelles alertes.

Elle peut également permettre à l’opérateur d’effectuer certaines actions manuelles, par exemple lancer un cycle d’amorçage ou forcer le remplissage du réservoir secondaire.

Cette supervision est importante, car elle rend le fonctionnement du système plus lisible et plus facilement exploitable.

---

## Synthèse du fonctionnement

Le fonctionnement retenu repose donc sur une logique simple : conserver l’installation existante, puis ajouter un système capable de maintenir une réserve d’eau d’amorçage.

La pompe principale P1 continue d’alimenter la zone de lavage. La pompe auxiliaire P2 remplit le réservoir secondaire R1. Le capteur de niveau C1 vérifie que la réserve d’eau est suffisante. Le clapet anti-retour C2 permet de limiter la perte d’amorçage.

L’ensemble est commandé par une carte ESP32, qui assure la lecture du capteur, la commande du relais et la communication avec l’automate.

Cette conception permet de répondre à la problématique principale du projet : rendre l’amorçage de la pompe plus autonome, plus fiable et plus facile à superviser.
