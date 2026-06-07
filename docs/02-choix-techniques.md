---

title: Choix techniques
layout: default
nav_order: 3
------------

# Choix techniques

## Analyse des solutions envisagées

Plusieurs solutions ont été étudiées afin d'assurer l'amorçage automatique de la pompe principale.

### Solution 1 : Amorçage manuel

Cette solution consistait à demander à l'opérateur de remplir manuellement le circuit avant chaque utilisation.

**Avantages :**

* Aucun coût supplémentaire.
* Mise en œuvre immédiate.

**Inconvénients :**

* Forte dépendance à l'opérateur.
* Risque d'oubli.
* Temps d'intervention important.
* Manque de répétabilité.

Cette solution a été écartée car elle ne répondait pas à l'objectif d'automatisation.

### Solution 2 : Réservoir gravitaire

L'utilisation d'un réservoir placé en hauteur permettait de remplir naturellement la conduite.

**Avantages :**

* Simplicité.
* Peu de composants.

**Inconvénients :**

* Contraintes d'encombrement.
* Débit limité.
* Installation difficile dans certaines configurations.

### Solution retenue

La solution retenue repose sur une petite pompe auxiliaire associée à un réservoir de stockage.

Cette architecture permet :

* Un amorçage totalement automatique.
* Une meilleure fiabilité.
* Une intégration simple dans la mini-usine.
* Une réduction du risque de fonctionnement à sec.

## Architecture générale

Le système est composé de :

* Un réservoir de stockage.
* Une pompe auxiliaire d'amorçage.
* Une pompe principale de nettoyage.
* Une alimentation électrique.
* Une logique de commande.

L'ensemble permet de remplir automatiquement le circuit avant le démarrage de la pompe principale.

