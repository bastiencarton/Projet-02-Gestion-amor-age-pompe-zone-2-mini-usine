---

title: Conception détaillée
layout: default
nav_order: 4
------------

# Conception détaillée

## Architecture fonctionnelle

Le système suit la séquence suivante :

1. Mise sous tension.
2. Activation de la pompe auxiliaire.
3. Remplissage du circuit.
4. Amorçage de la pompe principale.
5. Fonctionnement normal.
6. Arrêt du système.

## Réservoir

Le réservoir constitue la réserve d'eau nécessaire à l'amorçage.

Ses fonctions principales sont :

* Stocker l'eau.
* Garantir une alimentation immédiate.
* Limiter les risques de désamorçage.

## Pompe auxiliaire

La pompe auxiliaire a pour rôle de remplir le circuit avant la mise en route de la pompe principale.

Critères de choix :

* Faible consommation.
* Débit suffisant.
* Compatibilité avec le liquide utilisé.
* Encombrement réduit.

## Schéma fonctionnel

```text
Réservoir
    │
    ▼
Pompe auxiliaire
    │
    ▼
Circuit de remplissage
    │
    ▼
Pompe principale
    │
    ▼
Zone de nettoyage
```

## Sécurité

Le système intègre plusieurs mesures de sécurité :

* Protection électrique.
* Contrôle du niveau d'eau.
* Prévention du fonctionnement à sec.
* Arrêt d'urgence.
