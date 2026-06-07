---

layout: default
title: Réalisation
nav_order: 5
---

# Réalisation

Cette partie présente la réalisation concrète du système d’autoamorçage de pompe. L’objectif est de montrer comment les différents éléments ont été installés, câblés, programmés puis testés.

Le projet a été réalisé à partir d’une installation déjà existante. Cette installation comportait une pompe principale **P1**, utilisée pour alimenter la zone depuis la cuve principale. Notre travail a consisté à ajouter un système complémentaire permettant de maintenir l’amorçage de cette pompe.

---

## Installation existante

Avant notre intervention, l’installation était composée principalement :

* d’une cuve principale ;
* d’une conduite d’aspiration ;
* d’une pompe principale **P1** ;
* d’une conduite de refoulement vers l’installation.

Le problème rencontré venait du risque de désamorçage de la pompe principale. Lorsqu’une pompe se désamorce, elle n’arrive plus correctement à aspirer l’eau. Cela peut empêcher le bon fonctionnement de l’installation et nécessiter une intervention manuelle.

Notre objectif était donc de concevoir une solution permettant de conserver une réserve d’eau disponible pour faciliter le maintien de l’amorçage.

---

## Principe général de la solution réalisée

La solution retenue repose sur l’ajout de plusieurs éléments autour de l’installation existante.

Les principaux éléments ajoutés sont :

* un réservoir secondaire **R1** ;
* une pompe auxiliaire auto-amorçante **P2** ;
* un capteur de niveau **C1** ;
* un clapet anti-retour **C2** ;
* une carte **ESP32** ;
* un relais de commande ;
* une alimentation adaptée à la partie commande.

Le réservoir **R1** sert de réserve d’eau d’amorçage. La pompe auxiliaire **P2** permet de remplir ce réservoir lorsque le niveau d’eau est insuffisant. Le capteur **C1** permet de détecter le niveau d’eau dans le réservoir. Enfin, le clapet anti-retour **C2** permet d’éviter que l’eau ne reparte dans le mauvais sens lorsque la pompe s’arrête.

Le fonctionnement retenu est donc le suivant :

1. Le réservoir R1 contient une réserve d’eau.
2. Le capteur C1 surveille le niveau d’eau dans ce réservoir.
3. Si le niveau est trop bas, la pompe auxiliaire P2 est activée.
4. Lorsque le niveau est suffisant, la pompe P2 est arrêtée.
5. Le réservoir R1 permet de maintenir l’amorçage de la pompe principale P1.

Cette solution permet de limiter les risques de désamorçage tout en conservant l’installation principale déjà existante.

---

## Réalisation hydraulique

La première partie de la réalisation concerne l’ajout des éléments hydrauliques.

Nous avons ajouté le réservoir secondaire **R1** afin de disposer d’une réserve d’eau proche de la pompe principale. Ce réservoir permet de conserver une quantité d’eau disponible pour faciliter le maintien de l’amorçage.

La pompe auxiliaire **P2** a été ajoutée pour remplir ce réservoir. Son rôle n’est pas d’alimenter toute l’installation, mais uniquement d’assurer le remplissage du réservoir d’amorçage.

Le clapet anti-retour **C2** a été installé sur la conduite d’aspiration. Son rôle est d’empêcher l’eau de repartir vers la cuve principale lorsque la pompe est arrêtée. Cela permet de conserver de l’eau dans la conduite et de réduire le risque de désamorçage.

Ajouter ici une image du schéma hydraulique :

![Schéma hydraulique](assets/images/schema-hydraulique.png)

Ajouter ici une photo du montage hydraulique :

![Montage hydraulique](assets/images/montage-hydraulique.jpg)

---

## Installation du capteur de niveau C1

Le capteur de niveau **C1** a été installé dans le réservoir secondaire **R1**.

Son rôle est de détecter si le niveau d’eau est suffisant. Le capteur fonctionne comme un contact électrique. Selon la position du flotteur, le contact est soit ouvert, soit fermé.

Ce choix est adapté au projet car nous n’avons pas besoin de mesurer précisément la hauteur d’eau. Nous avons seulement besoin de savoir si le niveau est suffisant ou non.

Le capteur est ensuite relié à une entrée numérique de l’ESP32. Le programme lit l’état de cette entrée pour savoir s’il faut activer ou arrêter la pompe auxiliaire.

Ajouter ici une photo du capteur de niveau :

![Capteur de niveau C1](assets/images/capteur-niveau-c1.jpg)

---

## Réalisation électrique

Une partie importante du projet a été la réalisation de la partie électrique.

Nous avons tiré une alimentation depuis l’armoire principale en **220 V monophasé**. Cette alimentation arrive ensuite dans un coffret dédié à notre système.

Dans ce coffret, nous avons installé les éléments nécessaires à la commande :

* un transformateur 220 V vers 24 V ;
* une alimentation adaptée à la carte ESP32 ;
* la carte ESP32 ;
* une carte relais ;
* des borniers de raccordement ;
* les connexions vers le capteur de niveau ;
* les connexions vers la pompe auxiliaire.

L’objectif était de séparer correctement la partie puissance et la partie commande.

La partie puissance concerne l’alimentation de la pompe.
La partie commande concerne l’ESP32, le capteur de niveau et le relais.

Cette séparation est importante car l’ESP32 ne peut pas commander directement une pompe. Elle ne peut fournir qu’un faible courant sur ses sorties. Le relais permet donc de faire le lien entre la carte de commande et la pompe.

Ajouter ici une photo du coffret électrique :

![Coffret électrique](assets/images/coffret-electrique.jpg)

---

## Commande de la pompe par relais

La pompe auxiliaire **P2** est commandée à l’aide d’un relais.

Le relais fonctionne comme un interrupteur commandé électriquement :

* l’ESP32 envoie un signal de commande ;
* le relais change d’état ;
* la pompe est alimentée ou coupée.

Dans notre montage, nous avons utilisé le contact **normalement ouvert** du relais. Cela signifie que, lorsque le relais n’est pas activé, la pompe n’est pas alimentée. Elle ne démarre que lorsque l’ESP32 active le relais.

Ce choix est plus sécurisant car, en cas de problème ou d’arrêt de la commande, la pompe reste coupée.

---

## Branchement du capteur sur l’ESP32

Le capteur de niveau est branché sur une entrée numérique de l’ESP32.

Dans notre cas, l’entrée est configurée avec l’instruction `INPUT_PULLUP`. Cela permet d’utiliser la résistance de tirage interne de la carte et d’obtenir une lecture stable de l’état du capteur.

Le principe est le suivant :

* lorsque le contact est ouvert, l’entrée lit un état logique ;
* lorsque le contact est fermé, l’entrée change d’état ;
* le programme détecte ce changement et agit en conséquence.

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

Cette lecture permet ensuite de décider si la pompe auxiliaire doit être activée ou arrêtée.

---

## Programmation de l’ESP32

La carte ESP32 a été programmée avec l’environnement Arduino.

Le programme réalise principalement trois actions :

1. lire l’état du capteur de niveau ;
2. commander le relais de la pompe auxiliaire ;
3. communiquer certaines informations avec l’automate.

La logique générale du programme est simple :

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

Le but était de garder un programme clair et compréhensible. Cela permet de faciliter les tests, les corrections et les futures améliorations du système.

---

## Communication avec l’automate

Le système doit également être capable d’échanger des informations avec l’automate de l’installation.

Cette communication permet notamment de transmettre :

* l’état du capteur de niveau ;
* l’état de la pompe auxiliaire ;
* une information de défaut ;
* une information d’autorisation ou de fonctionnement.

L’intérêt de cette communication est d’intégrer notre système dans l’installation déjà existante. L’automate conserve ainsi son rôle de supervision, tandis que l’ESP32 gère la partie ajoutée liée à l’autoamorçage.

Dans notre projet, nous avons dû faire attention au format des données échangées, notamment à la lecture des registres et à l’organisation des mots transmis.

---

## Tests réalisés

Les tests ont été réalisés progressivement afin de vérifier chaque partie du système séparément avant de tester l’ensemble complet.

### Test du capteur de niveau

Nous avons d’abord testé le capteur de niveau seul.

L’objectif était de vérifier que le changement d’état du flotteur était bien détecté par l’ESP32. Pour cela, nous avons affiché l’état du capteur dans le moniteur série.

Ce test a permis de valider le branchement du capteur et la lecture de l’entrée numérique.

### Test du relais

Nous avons ensuite testé le relais seul.

L’objectif était de vérifier que l’ESP32 pouvait activer et désactiver le relais. Lors de ce test, nous avons entendu le relais commuter, ce qui a confirmé que la commande fonctionnait.

### Test de la pompe auxiliaire

Après avoir validé le relais, nous avons testé l’alimentation de la pompe auxiliaire.

Le but était de vérifier que la pompe démarrait lorsque le relais était activé et qu’elle s’arrêtait lorsque le relais était désactivé.

### Test du fonctionnement complet

Enfin, nous avons testé le fonctionnement global du système :

1. variation du niveau d’eau dans le réservoir R1 ;
2. détection par le capteur C1 ;
3. activation ou arrêt du relais ;
4. démarrage ou arrêt de la pompe P2 ;
5. maintien d’une réserve d’eau pour l’amorçage de la pompe principale P1.

Ce test a permis de valider la logique complète du système.

---

## Problèmes rencontrés

Plusieurs difficultés ont été rencontrées pendant la réalisation.

La première difficulté concernait le branchement du relais. Au départ, la pompe était branchée sur le contact normalement fermé au lieu du contact normalement ouvert. Le relais s’activait bien, mais la pompe ne fonctionnait pas comme prévu. Après correction du branchement, la commande de la pompe est devenue cohérente avec le programme.

Une autre difficulté concernait la lecture du capteur de niveau. Il a fallu comprendre le fonctionnement du contact du capteur et utiliser correctement l’entrée en `INPUT_PULLUP`.

Enfin, la communication avec l’automate a demandé de bien comprendre l’organisation des données, notamment le fait qu’un mot corresponde à deux octets et que plusieurs mots puissent devoir être regroupés pour former une information complète.

Ces difficultés ont permis de mieux comprendre le fonctionnement réel d’une installation automatisée, aussi bien sur la partie câblage que sur la partie programmation.

---

## Résultat obtenu

À la fin de la réalisation, nous avons obtenu un système capable de :

* surveiller le niveau d’eau dans le réservoir R1 ;
* commander automatiquement la pompe auxiliaire P2 ;
* remplir le réservoir secondaire lorsque le niveau est insuffisant ;
* maintenir une réserve d’eau pour l’amorçage ;
* limiter le risque de désamorçage de la pompe principale P1 ;
* communiquer avec l’automate de l’installation.

Le système obtenu permet donc de valider le principe de fonctionnement retenu. Il reste améliorable, mais il constitue une base fonctionnelle pour répondre au besoin initial.

---

## Bilan de la réalisation

Cette réalisation nous a permis de passer d’un problème concret à une solution technique fonctionnelle.

Nous avons travaillé sur plusieurs aspects du projet :

* la partie hydraulique avec le réservoir, la pompe et le clapet anti-retour ;
* la partie électrique avec l’alimentation, le relais et le câblage ;
* la partie programmation avec l’ESP32 ;
* la partie communication avec l’automate ;
* la partie tests et validation.

Cette étape a été importante car elle a permis de confronter les choix techniques à la réalité du montage. Elle nous a également permis de comprendre l’importance des tests progressifs, du câblage propre et de la séparation entre commande et puissance.
