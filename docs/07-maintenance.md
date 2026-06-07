---

layout: default
title: Maintenance et précautions d'utilisation
nav_order: 8
---

# Maintenance et précautions d’utilisation

Cette partie présente les précautions à respecter lors de l’utilisation du système d’autoamorçage de pompe, ainsi que les opérations de maintenance nécessaires pour conserver un fonctionnement fiable.

Le système associe plusieurs éléments sensibles : une pompe, de l’eau, une alimentation électrique, un capteur de niveau, un relais, une carte ESP32 et une communication avec l’automate. Il est donc important de respecter certaines règles afin de limiter les risques de panne, de mauvais fonctionnement ou de danger pour l’utilisateur.

---

## Précautions générales d’utilisation

Avant toute utilisation du système, il est nécessaire de vérifier visuellement l’état général de l’installation.

L’utilisateur doit s’assurer que le réservoir secondaire **R1** est correctement positionné, que les raccords hydrauliques sont bien fixés et qu’aucune fuite d’eau n’est visible autour du montage.

Il faut également vérifier que le coffret électrique est fermé et que les câbles ne sont pas endommagés. Comme le système utilise de l’eau à proximité d’éléments électriques, toute trace d’humidité anormale doit être prise au sérieux.

Le système ne doit pas être utilisé si :

* une fuite d’eau est visible ;
* un câble est abîmé ;
* le coffret électrique est ouvert ;
* le capteur de niveau est mal positionné ;
* la pompe émet un bruit anormal ;
* le relais commute de manière répétée sans raison ;
* l’ESP32 ou l’automate signale un comportement incohérent.

En cas de doute, il est préférable d’arrêter le système et de vérifier l’installation avant de poursuivre les essais.

---

## Précautions électriques

La partie électrique doit être manipulée avec beaucoup de précaution.

Avant toute intervention dans le coffret électrique, l’alimentation doit être coupée. Il ne faut jamais intervenir sur les borniers, le relais, l’alimentation ou la carte ESP32 lorsque le système est sous tension.

La séparation entre la partie puissance et la partie commande doit être conservée. La partie puissance concerne principalement l’alimentation de la pompe, tandis que la partie commande concerne l’ESP32, le capteur de niveau et le relais.

Cette séparation permet de protéger la carte électronique et de limiter les risques électriques.

Les précautions principales sont les suivantes :

* couper l’alimentation avant toute intervention ;
* ne pas toucher les borniers lorsque le système est alimenté ;
* ne pas laisser le coffret électrique ouvert pendant le fonctionnement ;
* vérifier que les câbles sont correctement serrés ;
* éviter tout contact entre l’eau et les éléments électriques ;
* ne pas modifier le câblage sans validation préalable ;
* ne pas court-circuiter le relais ou le capteur.

Le relais doit rester câblé sur le contact **normalement ouvert** afin que la pompe soit arrêtée par défaut. Ce choix est plus sûr, car en cas de coupure de commande ou de problème sur l’ESP32, la pompe reste à l’arrêt.

---

## Précautions hydrauliques

La partie hydraulique doit également être contrôlée régulièrement.

Le réservoir secondaire **R1** doit être suffisamment étanche pour éviter toute fuite. Une fuite pourrait réduire l’efficacité du système d’amorçage, mais aussi créer un risque si l’eau atteint la partie électrique.

Le clapet anti-retour **C2** doit être surveillé, car son rôle est important pour maintenir l’eau dans la conduite. S’il fonctionne mal, l’eau peut repartir vers la cuve principale et provoquer un nouveau désamorçage de la pompe principale **P1**.

Il faut donc vérifier régulièrement :

* l’absence de fuite autour du réservoir ;
* l’état des raccords hydrauliques ;
* le bon positionnement des tuyaux ;
* le bon fonctionnement du clapet anti-retour ;
* l’absence d’obstruction dans les conduites ;
* le niveau d’eau dans le réservoir secondaire.

Si le réservoir ne se remplit pas correctement, il faut arrêter le système et identifier l’origine du problème avant de relancer la pompe.

---

## Précautions liées au capteur de niveau

Le capteur de niveau **C1** est un élément essentiel du système. Il permet à l’ESP32 de savoir si le réservoir secondaire contient suffisamment d’eau.

Il faut donc s’assurer que le capteur est bien fixé et que le flotteur peut bouger librement. Si le flotteur est bloqué, l’information envoyée à l’ESP32 peut être incorrecte.

Un mauvais état du capteur peut entraîner plusieurs problèmes :

* la pompe auxiliaire peut démarrer alors que le niveau est déjà suffisant ;
* la pompe peut rester arrêtée alors que le réservoir est vide ;
* le système peut alterner trop rapidement entre marche et arrêt ;
* l’automate peut recevoir une information incohérente.

Lors de la maintenance, il faut donc vérifier que le capteur change bien d’état lorsque le niveau d’eau varie.

---

## Maintenance préventive

La maintenance préventive consiste à vérifier régulièrement le système avant qu’une panne n’apparaisse.

Elle permet de limiter les risques de défaut et d’augmenter la fiabilité de l’installation.

Les contrôles suivants peuvent être réalisés régulièrement :

| Élément contrôlé      | Action à réaliser                          | Fréquence conseillée |
| --------------------- | ------------------------------------------ | -------------------- |
| Réservoir R1          | Vérifier l’absence de fuite                | À chaque utilisation |
| Capteur C1            | Vérifier que le flotteur bouge librement   | Régulièrement        |
| Pompe auxiliaire P2   | Vérifier le démarrage et l’arrêt           | Lors des essais      |
| Clapet anti-retour C2 | Vérifier qu’il bloque bien le retour d’eau | Régulièrement        |
| Relais                | Vérifier la commutation                    | Lors des tests       |
| Câblage               | Vérifier le serrage et l’état des fils     | Avant intervention   |
| Coffret électrique    | Vérifier qu’il est fermé et protégé        | À chaque utilisation |
| Programme ESP32       | Vérifier le comportement attendu           | Après modification   |

Cette maintenance préventive est importante car le système est destiné à fonctionner automatiquement. Un défaut non détecté pourrait empêcher l’amorçage correct de la pompe principale.

---

## Maintenance corrective

La maintenance corrective intervient lorsqu’un problème est constaté pendant l’utilisation ou les essais.

Avant toute recherche de panne, il faut couper l’alimentation du système si une intervention sur le câblage, le relais, la pompe ou le coffret est nécessaire.

Quelques exemples de pannes possibles :

| Problème observé                              | Causes possibles                                           | Vérifications à effectuer                          |
| --------------------------------------------- | ---------------------------------------------------------- | -------------------------------------------------- |
| La pompe auxiliaire ne démarre pas            | Relais mal câblé, alimentation absente, défaut de commande | Vérifier le relais, l’alimentation et le programme |
| Le relais commute mais la pompe ne tourne pas | Mauvais contact utilisé, pompe non alimentée               | Vérifier le contact normalement ouvert             |
| Le niveau n’est pas détecté                   | Capteur mal branché ou flotteur bloqué                     | Vérifier le câblage et le mouvement du flotteur    |
| La pompe ne s’arrête pas                      | Capteur bloqué, programme incorrect, relais bloqué         | Vérifier l’état du capteur et du relais            |
| Le réservoir ne se remplit pas                | Pompe défaillante, tuyau bouché, fuite                     | Vérifier la pompe et la conduite                   |
| L’information automate est incohérente        | Mauvaise organisation des données                          | Vérifier les mots, registres et octets échangés    |

Cette méthode permet d’identifier plus facilement l’origine du problème en contrôlant les éléments un par un.

---

## Procédure d’utilisation conseillée

Pour utiliser le système dans de bonnes conditions, il est conseillé de suivre une procédure simple.

Avant le démarrage :

1. vérifier que le réservoir R1 est correctement installé ;
2. vérifier qu’aucune fuite n’est visible ;
3. vérifier que le coffret électrique est fermé ;
4. vérifier que le capteur de niveau est bien positionné ;
5. vérifier que la pompe auxiliaire est correctement raccordée ;
6. mettre le système sous tension.

Pendant le fonctionnement :

1. surveiller le comportement du réservoir ;
2. vérifier que la pompe auxiliaire démarre uniquement lorsque le niveau est insuffisant ;
3. vérifier que la pompe s’arrête lorsque le niveau devient suffisant ;
4. surveiller les éventuels bruits anormaux ;
5. contrôler les informations transmises à l’automate.

Après utilisation :

1. arrêter le système si nécessaire ;
2. vérifier l’absence de fuite ;
3. vérifier que la pompe est bien arrêtée ;
4. laisser le coffret fermé ;
5. noter les éventuels problèmes rencontrés.

---

## Précautions avant modification du système

Toute modification du système doit être réalisée avec prudence.

Avant de modifier le programme ESP32, le câblage électrique ou la configuration automate, il faut bien comprendre le fonctionnement existant. Une modification mal maîtrisée peut provoquer un comportement inattendu de la pompe.

Avant toute modification, il est recommandé de :

* sauvegarder le programme fonctionnel ;
* noter le câblage actuel ;
* prendre des photos du montage ;
* modifier un seul élément à la fois ;
* tester chaque modification séparément ;
* vérifier le fonctionnement complet après modification.

Cette méthode limite le risque de créer une panne difficile à identifier.

---

## Bilan

La maintenance et les précautions d’utilisation sont essentielles pour garantir le bon fonctionnement du système d’autoamorçage.

Le système réalisé fonctionne avec plusieurs éléments qui doivent rester fiables : le capteur de niveau, la pompe auxiliaire, le relais, le réservoir, le clapet anti-retour et la communication avec l’automate.

Une utilisation correcte et une maintenance régulière permettent de limiter les risques de panne, d’améliorer la sécurité et de prolonger la durée de vie du système.

Cette partie montre que le projet ne se limite pas à la réalisation technique. Il faut également penser à l’exploitation du système, à sa sécurité et à sa fiabilité dans le temps.
