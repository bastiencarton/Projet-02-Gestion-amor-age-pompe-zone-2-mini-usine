---

layout: home
nav_order: 1
title: Accueil
permalink: /
---

# Projet de gestion d’amorçage de pompe

Bienvenue sur la documentation de notre projet de **gestion de l’amorçage de pompe pour la zone 2 de la mini-usine**.

Ce site présente le travail réalisé autour de l’automatisation de l’amorçage d’une pompe utilisée dans la partie lavage des graviers. L’objectif est d’expliquer le contexte du projet, les choix techniques effectués, la conception de la solution, les étapes de réalisation, les tests menés ainsi que les améliorations possibles.

[Notre repo GitHub](https://github.com/bastiencarton/Projet-02-Gestion-amor-age-pompe-zone-2-mini-usine){: .btn .btn-primary .fs-5 .mb-4 .mb-md-0 }

---

## À propos du projet

Dans la mini-usine d’UniLaSalle Amiens, les graviers sont transportés sur différents convoyeurs avant d’être lavés, séchés, chauffés puis conditionnés. Notre projet concerne plus particulièrement la **zone de lavage**, où une pompe hydraulique permet d’envoyer de l’eau sous pression afin de nettoyer les graviers.

Actuellement, cette pompe nécessite un amorçage manuel avant son démarrage. Cette opération consiste à verser de l’eau dans le corps de pompe afin de permettre la création de l’aspiration. Ce fonctionnement demande une intervention humaine systématique et peut entraîner des risques en cas d’oubli, notamment un fonctionnement à sec de la pompe.

Le but du projet est donc de proposer une solution permettant de rendre cette phase d’amorçage plus automatique, plus fiable et plus facile à superviser.

---

## Objectif du projet

L’objectif principal est de concevoir un système capable de rendre la pompe automatiquement amorçable après un premier amorçage manuel.

La solution doit permettre de :

* maintenir une réserve d’eau pour les démarrages suivants ;
* détecter un niveau d’eau insuffisant ;
* éviter le fonctionnement à sec de la pompe ;
* limiter les interventions humaines ;
* transmettre les informations de fonctionnement à l’automate ;
* permettre une supervision depuis l’interface HMI.

---

## Présentation générale de la solution

La solution développée repose sur l’ajout d’un système de commande et de supervision autour de la pompe existante.

Le fonctionnement général est le suivant : le système surveille le niveau d’eau disponible pour l’amorçage, commande les éléments nécessaires au remplissage ou au démarrage, puis transmet les informations utiles vers l’automate. L’opérateur peut également suivre l’état du système et intervenir depuis l’interface HMI si nécessaire.

Cette solution permet d’améliorer la fiabilité de l’installation tout en conservant le matériel déjà présent dans la mini-usine.

---

## Organisation de la documentation

La documentation est organisée en plusieurs parties :

* **Contexte et besoin** : présentation du problème initial et des objectifs du projet.
* **Choix techniques** : justification des composants et des solutions retenues.
* **Conception détaillée** : description du fonctionnement prévu du système.
* **Réalisation** : présentation du câblage, de la programmation et de l’intégration.
* **Validation** : tests réalisés et résultats obtenus.
* **Analyse critique** : limites du système et améliorations possibles.
* **Maintenance et utilisation** : consignes pour utiliser ou reprendre le projet.



---

## Vidéo de démonstration

Cette vidéo présente le fonctionnement du système d'amorçage automatique ainsi que les différentes étapes de réalisation du projet.

{% raw %}
<iframe width="100%" height="600"
src="https://www.youtube.com/embed/cvKz3VldXyE"
title="Vidéo de démonstration"
frameborder="0"
allowfullscreen>
</iframe>
{% endraw %}
