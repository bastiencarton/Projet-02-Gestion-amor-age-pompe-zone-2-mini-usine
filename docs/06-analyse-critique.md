---
title: Analyse critique
layout: default
nav_order: 7
---

# Analyse critique

Cette partie a pour objectif de prendre du recul sur le projet réalisé. Après avoir présenté la réalisation et la validation du système, il est important d’analyser les points forts de la solution, ses limites, ainsi que les améliorations possibles.

Le système réalisé permet de répondre au besoin principal : maintenir une réserve d’eau disponible afin de limiter le risque de désamorçage de la pompe principale **P1**. Cependant, comme il s’agit d’un projet réalisé dans un cadre pédagogique et expérimental, certains points peuvent encore être améliorés pour rendre la solution plus fiable, plus propre et plus proche d’une installation industrielle définitive.

---

## Points positifs du projet

Le premier point positif du projet est que la solution retenue est simple à comprendre. Le système repose sur des éléments clairement identifiables : un réservoir secondaire, une pompe auxiliaire, un capteur de niveau, un relais et une carte ESP32.

Cette architecture rend le fonctionnement assez lisible. Le capteur surveille le niveau d’eau, l’ESP32 traite l’information, puis le relais commande la pompe auxiliaire. Cette logique simple facilite les tests et la recherche de panne.

Un autre point positif est que nous avons conservé l’installation existante. La pompe principale **P1** reste l’élément central du système, et notre solution vient simplement ajouter une fonction d’aide à l’amorçage. Cela limite les modifications importantes sur l’installation de départ.

Le projet nous a également permis de travailler sur plusieurs domaines techniques :

* l’hydraulique, avec le réservoir, la pompe et le clapet anti-retour ;
* l’électricité, avec l’alimentation, le relais et le câblage ;
* l’automatisme, avec l’intégration à l’automate ;
* la programmation, avec l’ESP32 ;
* la communication, avec l’échange d’informations entre l’ESP32 et l’automate.

Cette diversité rend le projet intéressant, car il ne se limite pas à une seule compétence technique.

---

## Pertinence de la solution retenue

La solution retenue est cohérente avec le problème initial. Le désamorçage d’une pompe est souvent lié à une perte d’eau dans la conduite ou à une absence d’eau disponible au moment du redémarrage. L’ajout d’un réservoir secondaire permet donc de conserver une réserve d’eau dédiée à l’amorçage.

Le choix d’une pompe auxiliaire **P2** est également pertinent, car elle permet de remplir le réservoir secondaire sans dépendre directement du fonctionnement de la pompe principale. Les rôles sont ainsi séparés : la pompe principale alimente l’installation, tandis que la pompe auxiliaire s’occupe uniquement de maintenir le réservoir d’amorçage rempli.

Le capteur de niveau **C1** est adapté au besoin, car le système n’a pas besoin de connaître précisément la hauteur d’eau. Il doit simplement savoir si le niveau est suffisant ou non. Un capteur de type contact est donc une solution simple et efficace pour ce type d’application.

Enfin, l’utilisation d’un relais permet de séparer la partie commande de la partie puissance. Cette séparation est importante, car l’ESP32 ne peut pas alimenter directement une pompe.

---

## Limites de la solution

Même si le système répond au besoin principal, il présente encore certaines limites.

La première limite concerne le caractère encore expérimental du montage. Le système fonctionne, mais il devrait être renforcé pour une utilisation longue durée. Certains éléments, comme le coffret, les raccords hydrauliques ou le réservoir, doivent être vérifiés dans des conditions réelles prolongées.

Une autre limite concerne l’étanchéité du réservoir. Comme le système utilise de l’eau à proximité d’éléments électriques, l’étanchéité est un point essentiel. Il faut s’assurer qu’aucune fuite ne puisse atteindre la partie électrique ou perturber le fonctionnement du capteur.

La fiabilité du capteur de niveau est également un point à surveiller. Un capteur à flotteur est simple, mais il peut être sensible à un blocage mécanique, à une mauvaise position ou à l’encrassement. Si le capteur donne une mauvaise information, la pompe auxiliaire peut être activée ou arrêtée au mauvais moment.

La communication avec l’automate représente aussi une limite potentielle. L’échange de données doit être parfaitement maîtrisé pour éviter les erreurs d’interprétation. La gestion des mots, des octets et des registres demande de la rigueur, car une mauvaise configuration peut entraîner une information incorrecte côté automate.

---

## Limites liées à la sécurité

La sécurité est un point très important dans ce projet, car le système mélange de l’eau, de l’électricité et une pompe.

Le choix d’utiliser un relais normalement ouvert est positif, car par défaut la pompe reste arrêtée. Cependant, il faudrait aller plus loin dans la sécurisation du système.

Par exemple, il serait intéressant d’ajouter une gestion des défauts. Si le capteur ne change jamais d’état, si la pompe tourne trop longtemps ou si le réservoir ne se remplit pas, le système devrait pouvoir détecter une anomalie et arrêter automatiquement la pompe.

Il faudrait aussi vérifier que le coffret électrique est suffisamment protégé contre l’humidité et les projections d’eau. Dans une version plus aboutie, l’utilisation d’un coffret étanche et d’une protection adaptée serait nécessaire.

---

## Limites liées au programme

Le programme ESP32 a été volontairement conçu de manière simple afin de faciliter les essais. Ce choix est adapté pour un prototype, mais il peut montrer ses limites dans une version définitive.

Actuellement, l’ESP32 fonctionne de manière autonome. Il prend directement en compte l’état du capteur de niveau et commande la pompe auxiliaire sans intervention de l’automate. Cette solution a permis de simplifier le développement et de valider rapidement le fonctionnement du système.

Cependant, dans une version plus aboutie, il serait intéressant d’ajouter une partie de contrôle depuis l’IHM de l’automate. L’opérateur pourrait ainsi visualiser l’état du système, commander certains modes de fonctionnement et recevoir des informations de diagnostic directement depuis l’interface de supervision.

Pour améliorer la robustesse du programme, il serait également intéressant d’ajouter :

* une temporisation pour éviter les déclenchements trop rapides ;
* une sécurité si la pompe fonctionne trop longtemps ;
* une détection de défaut capteur ;
* un mode manuel pour les essais ou la maintenance ;
* un retour d’état clair vers l’automate ;
* la possibilité de piloter certaines fonctions depuis l’IHM de l’automate.

Ces améliorations permettraient d’éviter certains comportements indésirables, par exemple une pompe qui démarre et s’arrête trop souvent à cause de petites variations du niveau, tout en offrant une meilleure supervision et un contrôle plus complet du système.

---

## Difficultés rencontrées

Le projet nous a montré qu’un système simple en théorie peut devenir plus complexe lors de la réalisation.

Une première difficulté a été liée au câblage du relais. Le relais s’activait bien, mais la pompe ne fonctionnait pas comme prévu car le branchement était réalisé sur le mauvais contact. Ce problème a permis de mieux comprendre la différence entre un contact normalement ouvert et un contact normalement fermé.

Une autre difficulté a concerné le capteur de niveau. Il a fallu comprendre son comportement électrique et adapter le programme avec l’utilisation de `INPUT_PULLUP`.

Enfin, la communication avec l’automate a demandé un travail de compréhension supplémentaire. Le fait qu’un mot corresponde à deux octets et que les données soient organisées en registres impose d’être très rigoureux dans la configuration.

Ces difficultés ont été positives, car elles ont permis de progresser et de mieux comprendre le fonctionnement réel d’un système automatisé.

---

## Apports personnels et techniques

Ce projet nous a permis de développer plusieurs compétences.

Sur le plan technique, nous avons appris à intégrer différents composants dans un même système : capteur, relais, pompe, ESP32 et automate. Nous avons également renforcé nos connaissances en câblage, en programmation embarquée et en communication industrielle.

Sur le plan méthodologique, le projet nous a appris l’importance de tester progressivement. Il est plus efficace de valider chaque élément séparément avant de tester l’ensemble complet. Cette méthode permet de trouver plus facilement l’origine d’un problème.

Le projet nous a aussi montré l’importance de la documentation. Comme le système comporte plusieurs parties, il est nécessaire de garder une trace claire des choix techniques, des branchements, du programme et des essais réalisés.

Enfin, ce projet nous a permis de prendre conscience de l’importance de la sécurité lorsqu’un système associe électricité et eau.

---

## Améliorations possibles

Plusieurs améliorations pourraient être envisagées pour rendre le système plus fiable et plus proche d’une solution industrielle.

Il serait d’abord intéressant d’améliorer l’étanchéité du réservoir et de protéger davantage les éléments électriques contre les projections d’eau.

Ensuite, le programme pourrait être complété avec des sécurités supplémentaires, comme une durée maximale de fonctionnement de la pompe ou une détection d’anomalie si le niveau ne change pas après un certain temps.

L’interface avec l’automate pourrait également être améliorée, en envoyant davantage d’informations : état du capteur, état de la pompe, défaut éventuel, mode automatique ou manuel.

Enfin, il serait utile de réaliser des essais sur une durée plus longue afin de vérifier la fiabilité du système dans le temps.

---

## Bilan critique

Globalement, le projet a permis de proposer une solution cohérente au problème d’amorçage de la pompe principale. Le système est compréhensible, fonctionnel et adapté à un prototype.

Cependant, pour passer d’un prototype à une installation réellement durable, plusieurs points doivent encore être améliorés : la sécurité, l’étanchéité, la gestion des défauts, la robustesse du programme et la fiabilité à long terme.

Cette analyse critique montre donc que le projet est une base solide, mais qu’il nécessite encore des améliorations avant une utilisation définitive en conditions réelles.
