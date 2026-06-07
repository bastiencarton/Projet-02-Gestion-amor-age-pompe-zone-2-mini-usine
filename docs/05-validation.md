---

layout: default
title: Validation
nav_order: 6
---

# Validation

Cette partie présente les essais réalisés afin de vérifier le bon fonctionnement du système d’autoamorçage de pompe.

Après la réalisation du montage hydraulique, électrique et informatique, nous avons validé progressivement chaque partie du système. L’objectif était de ne pas tester directement l’ensemble complet, mais de vérifier chaque élément séparément avant de réaliser un essai global.

La validation a donc été réalisée en plusieurs étapes :

1. validation du capteur de niveau ;
2. validation du relais ;
3. validation de la commande de la pompe auxiliaire ;
4. validation de la communication avec l’automate ;
5. validation du fonctionnement complet du système.

---

## Méthode de validation

Pour valider le système, nous avons suivi une méthode progressive.

Chaque élément a d’abord été testé seul, afin de vérifier son fonctionnement de base. Une fois chaque composant validé, nous avons testé les interactions entre les différents éléments.

Cette méthode permet de repérer plus facilement l’origine d’un problème. Par exemple, si la pompe ne démarre pas, le problème peut venir du programme, du relais, du câblage ou de l’alimentation. En testant chaque partie séparément, il est plus simple d’identifier la cause.

La validation a donc été organisée de la manière suivante :

| Élément testé          | Objectif du test                             | Résultat attendu                                        |
| ---------------------- | -------------------------------------------- | ------------------------------------------------------- |
| Capteur de niveau C1   | Vérifier la détection du niveau d’eau        | Changement d’état visible dans le programme             |
| Relais                 | Vérifier la commande par l’ESP32             | Commutation du relais                                   |
| Pompe auxiliaire P2    | Vérifier le démarrage et l’arrêt de la pompe | Pompe alimentée uniquement lorsque le relais est activé |
| Communication automate | Vérifier l’échange d’informations            | Données correctement transmises                         |
| Système complet        | Vérifier le fonctionnement automatique       | Remplissage du réservoir selon le niveau détecté        |

---

## Validation du capteur de niveau C1

Le premier test réalisé concerne le capteur de niveau **C1**.

Le capteur est installé dans le réservoir secondaire **R1**. Son rôle est de détecter si le niveau d’eau est suffisant ou non.

Pour valider son fonctionnement, nous avons branché le capteur sur une entrée numérique de l’ESP32. L’entrée a été configurée avec l’instruction `INPUT_PULLUP`, afin d’obtenir une lecture stable.

Le programme de test affichait l’état du capteur dans le moniteur série.

Extrait simplifié du code utilisé :

```cpp
const int PIN_FLOTTEUR = 4;

int dernierEtat = -1;

void setup() {
  Serial.begin(115200);
  pinMode(PIN_FLOTTEUR, INPUT_PULLUP);
}

void loop() {
  int etat = digitalRead(PIN_FLOTTEUR);

  if (etat != dernierEtat) {
    dernierEtat = etat;
    Serial.print("Nouvel état brut = ");
    Serial.println(etat);
  }

  delay(50);
}
```

Le test consistait à modifier la position du flotteur afin de vérifier que l’état lu par l’ESP32 changeait correctement.

Lorsque le flotteur change de position, l’état affiché dans le moniteur série change également. Cela confirme que le capteur est correctement détecté par l’ESP32.

Ce test permet donc de valider :

* le branchement du capteur ;
* la lecture de l’entrée numérique ;
* la détection du changement d’état ;
* l’utilisation de `INPUT_PULLUP`.

Ajouter ici une capture du moniteur série :

![Test du capteur de niveau](assets/images/test-capteur-niveau.png)

---

## Validation du relais

Le deuxième test concerne le relais de commande.

La pompe auxiliaire **P2** ne peut pas être commandée directement par l’ESP32. Le relais sert donc d’intermédiaire entre la carte de commande et la pompe.

L’objectif du test était de vérifier que l’ESP32 pouvait bien activer et désactiver le relais.

Nous avons utilisé un programme simple permettant d’activer puis de désactiver le relais à intervalles réguliers. Pendant le test, nous pouvions entendre le relais commuter.

Ce bruit de commutation confirme que la commande envoyée par l’ESP32 est bien prise en compte par le relais.

Le test a permis de valider :

* la communication entre l’ESP32 et le module relais ;
* l’activation du relais ;
* la désactivation du relais ;
* le bon comportement de la sortie de commande.

Ajouter ici une photo ou une vidéo du test du relais :

![Test du relais](assets/images/test-relais.jpg)

---

## Validation de la commande de la pompe auxiliaire P2

Après avoir validé le relais seul, nous avons testé la commande de la pompe auxiliaire **P2**.

L’objectif était de vérifier que la pompe démarre uniquement lorsque le relais est activé.

Lors de ce test, la pompe a été branchée sur le contact **normalement ouvert** du relais. Ce choix permet d’avoir un fonctionnement plus sûr : lorsque le relais n’est pas activé, la pompe reste arrêtée.

Le test consistait à activer le relais depuis l’ESP32, puis à vérifier que la pompe était bien alimentée.

Le fonctionnement attendu était le suivant :

| État du relais   | État attendu de la pompe |
| ---------------- | ------------------------ |
| Relais désactivé | Pompe arrêtée            |
| Relais activé    | Pompe en fonctionnement  |

Ce test a permis de confirmer que la pompe auxiliaire peut être commandée automatiquement par l’ESP32 à travers le relais.

Une difficulté a été rencontrée pendant cette étape. Au départ, la pompe était branchée sur le contact normalement fermé du relais. Le comportement obtenu n’était donc pas celui attendu. Après correction du branchement sur le contact normalement ouvert, la commande de la pompe est devenue cohérente.

Ce test permet donc de valider :

* le câblage de la pompe sur le relais ;
* l’utilisation du contact normalement ouvert ;
* l’activation de la pompe par l’ESP32 ;
* l’arrêt de la pompe lorsque le relais est désactivé.

Ajouter ici une photo du branchement du relais et de la pompe :

![Branchement relais pompe](assets/images/branchement-relais-pompe.jpg)

---

## Validation de la logique automatique

Une fois le capteur, le relais et la pompe validés séparément, nous avons testé la logique automatique du programme.

Le principe est le suivant :

* si le niveau d’eau dans le réservoir R1 est insuffisant, la pompe auxiliaire P2 doit démarrer ;
* si le niveau d’eau est suffisant, la pompe auxiliaire P2 doit s’arrêter.

La logique peut être représentée simplement de cette manière :

```cpp
if (niveauDetecte == false) {
  // Niveau insuffisant
  // Activation de la pompe auxiliaire
}
else {
  // Niveau suffisant
  // Arrêt de la pompe auxiliaire
}
```

Le test consistait à modifier manuellement la position du flotteur afin de simuler un niveau d’eau bas ou haut.

Lorsque le niveau est détecté comme insuffisant, le relais s’active et la pompe auxiliaire démarre. Lorsque le niveau devient suffisant, le relais se désactive et la pompe s’arrête.

Ce test valide la logique principale du système.

Ajouter ici un schéma simple du fonctionnement automatique :

![Logique automatique](assets/images/logique-automatique.png)

---

## Validation de la communication avec l’automate

Le système doit également communiquer avec l’automate de l’installation.

L’objectif de cette communication est de transmettre des informations utiles au système de supervision, comme l’état du capteur de niveau ou l’état de la pompe auxiliaire.

Pendant cette étape, nous avons vérifié que les informations envoyées par l’ESP32 étaient correctement interprétées côté automate.

Une attention particulière a été portée à l’organisation des données. En effet, les informations peuvent être échangées sous forme de mots ou de registres. Un mot correspond à deux octets, ce qui impose de bien organiser les données envoyées ou reçues.

Cette étape a permis de mieux comprendre la relation entre :

* les données envoyées par l’ESP32 ;
* les registres utilisés par l’automate ;
* l’interprétation des informations dans le programme automate.

La validation de cette partie confirme que l’ESP32 peut être intégrée dans l’installation existante sans remplacer l’automate principal.

Ajouter ici une capture de la configuration automate ou de la communication :

![Validation communication automate](assets/images/validation-communication-automate.png)

---

## Validation du fonctionnement complet

Après avoir validé chaque élément séparément, nous avons réalisé un test global du système.

Le test complet consistait à vérifier l’enchaînement suivant :

1. le niveau d’eau dans le réservoir R1 diminue ;
2. le capteur C1 détecte un niveau insuffisant ;
3. l’ESP32 lit l’état du capteur ;
4. l’ESP32 active le relais ;
5. la pompe auxiliaire P2 démarre ;
6. le réservoir R1 se remplit ;
7. le capteur C1 détecte un niveau suffisant ;
8. l’ESP32 désactive le relais ;
9. la pompe auxiliaire P2 s’arrête.

Ce fonctionnement correspond au comportement attendu pour le système d’autoamorçage.

Le test global a permis de valider l’ensemble de la chaîne :

* détection du niveau ;
* traitement par l’ESP32 ;
* commande du relais ;
* alimentation de la pompe ;
* remplissage du réservoir ;
* arrêt automatique lorsque le niveau est suffisant.

Ajouter ici une photo ou une vidéo du test complet :

![Test complet du système](assets/images/test-complet-systeme.jpg)

---

## Problèmes rencontrés pendant les essais

Les essais ont permis de mettre en évidence plusieurs difficultés.

La première difficulté concernait le branchement du relais. Le relais semblait fonctionner, car nous pouvions entendre sa commutation, mais la pompe n’était pas alimentée correctement. Le problème venait du branchement sur le mauvais contact du relais. Après passage sur le contact normalement ouvert, le fonctionnement est devenu correct.

La deuxième difficulté concernait la lecture du capteur de niveau. Il a fallu comprendre le comportement du contact du flotteur et adapter le programme pour utiliser correctement l’entrée en `INPUT_PULLUP`.

La troisième difficulté concernait la communication avec l’automate. Il a fallu bien comprendre l’organisation des mots et des octets afin que les informations soient correctement interprétées.

Ces problèmes ont été utiles, car ils ont permis de mieux comprendre le fonctionnement réel du système et de corriger progressivement les erreurs.

---

## Limites de la validation

Même si le système fonctionne, certains points restent à améliorer ou à tester plus précisément.

Les principales limites identifiées sont :

* la nécessité de tester le système sur une durée plus longue ;
* la vérification de l’étanchéité du réservoir ;
* la sécurisation définitive du coffret électrique ;
* l’amélioration de la protection contre les projections d’eau ;
* la vérification du comportement en cas de coupure d’alimentation ;
* la gestion d’éventuels défauts de capteur ou de pompe.

Ces points pourront être traités dans les améliorations futures du projet.

---

## Bilan de la validation

La validation progressive a permis de confirmer que les principaux éléments du système fonctionnent correctement.

Le capteur de niveau est bien détecté par l’ESP32. Le relais peut être commandé par la carte. La pompe auxiliaire peut être activée ou arrêtée automatiquement. La communication avec l’automate permet d’intégrer le système dans l’installation existante.

Le système répond donc au besoin principal : maintenir une réserve d’eau dans le réservoir secondaire afin de limiter le risque de désamorçage de la pompe principale.

Cette validation montre que la solution retenue est fonctionnelle et cohérente avec les objectifs du projet.
