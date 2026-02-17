# Poker Heads-Up - Jeu en console (Texas Hold'em)

Bienvenue dans **Poker Heads-Up**, une implémentation en Python d'un jeu de poker Texas Hold'em en tête-à-tête. Affrontez une intelligence artificielle dans une partie rapide où chaque joueur commence avec une pile de 5 jetons.

## 🎯 Description

Ce projet est un jeu de poker en mode console, conçu pour un affrontement entre un joueur humain et un adversaire contrôlé par l'ordinateur. Il suit le déroulement classique d'une main de Texas Hold'em : distribution, tours d'enchères (préflop, flop, turn, river) et showdown. L'IA prend ses décisions en fonction de la force de sa main et de tirages potentiels, avec une part d'aléatoire pour simuler des comportements variés.

## ✨ Fonctionnalités

- **Mode head-to-head** : Jouez contre une IA.
- **Phases de jeu complètes** : Préflop, Flop, Turn, River, Showdown.
- **Système d'enchères simplifié** : Actions possibles : Check, Bet (1 jeton), All-in, Call, Fold.
- **Détection des mains** : L'IA et le jeu identifient les combinaisons (paire, double paire, brelan, suite, couleur, full, carré, quinte flush).
- **Calcul des outs** : L'IA évalue les tirages (couleur ou suite) pour décider de suivre ou non.
- **Gestion des stacks et du pot** : Les jetons sont mis à jour après chaque main.
- **Interface textuelle simple** : Instructions claires pour le joueur.

## 📋 Prérequis

- **Python 3.x** installé sur votre machine.

## 🚀 Installation et utilisation

1. **Clonez le dépôt** (ou téléchargez le fichier) :
   ```bash
   git clone https://github.com/juniorbattle/poker.git
   cd poker
2. Exécutez le script :
  bash
  python poker.py
  (Si votre fichier s'appelle différemment, adaptez la commande.)

3. Suivez les instructions dans la console pour jouer.

## 🎮 Comment jouer

Le jeu commence par la distribution de deux cartes privées à chaque joueur.

À chaque tour d'enchères, des actions vous sont proposées :

- **1. Check** : Si personne n'a misé avant vous.
- **2. Bet** : Miser 1 jeton.
- **3. All-in** : Miser tous vos jetons restants.

Si l'adversaire a misé, vous devez choisir entre :
- **1. Call** (suivre)
- **2. Fold** (se coucher)

Après le flop, le turn et la river, les cartes communes sont révélées.  
Au showdown, la meilleure main remporte le pot.  
À la fin de chaque main, vous pouvez choisir de continuer ou d'arrêter.

**Exemple de déroulement :**
//////// Jeu commencé ! ////////
//////// Distribution des cartes ////////
//////// Flop ////////
Cartes sur le tableau: ['2H', '7C', 'KD']
Joueur 1 : ['AS', '5D'] / Stack : 5
Joueur IA : ['QH', '9S'] / Stack : 5
Montant du pot: 0
(Joueur 1) Choisissez une action : 1.Check / 2.Bet / 3.All-in


## 📜 Règles du jeu

- **Mises** : Les mises sont fixes à 1 jeton (sauf All-in).
- **Stacks** : Chaque joueur commence avec 5 jetons.
- **Détermination du gagnant** : Les mains sont classées selon la hiérarchie standard du poker (quinte flush > carré > full > couleur > suite > brelan > double paire > paire > carte haute).
- **Égalité** : En cas d'égalité de combinaison, la carte la plus haute départage (kicker).
- **IA** : L'IA adapte son comportement selon qu'elle est en position de mise ou non, la force de sa main, et la présence de tirages.

## 🧠 Structure du code

Le code est organisé en fonctions principales :

- `init()` : Mélange le paquet.
- `distribute_cards()` : Distribue deux cartes à chaque joueur.
- `show_cards_board()` : Ajoute des cartes communes au tableau.
- `card_value()` : Convertit une carte en valeur numérique.
- `determine_hand()` : Identifie la combinaison d'une main.
- `calculate_outs()` : Vérifie les tirages (flush, straight).
- `determine_best_cards()` : Calcule un score pour départager les égalités.
- `determine_winner()` : Compare les deux mains.
- `determine_action_playerAI()` : Logique de décision de l'IA.
- `decide_actions()` : Gère le déroulement des tours d'enchères.
- **Boucle principale** : Enchaîne les phases de jeu.

## 🔧 Personnalisation

Vous pouvez facilement modifier certains paramètres :

- **Taille des stacks** : Changez les valeurs initiales `hero_stack` et `villain_stack`.
- **Montant des mises** : Ajustez la valeur des bets (actuellement 1) dans les fonctions de gestion des actions.
- **Comportement de l'IA** : Modifiez les seuils dans `determine_action_playerAI()` pour rendre l'IA plus agressive ou plus passive.
- **Affichage** : Adaptez les messages pour améliorer l'interface.

## ⚠️ Remarques

- Ce jeu est une simulation simplifiée du poker, idéale pour comprendre la logique d'un jeu de cartes en Python.
- L'IA n'est pas parfaite : elle ne bluffe pas et ses décisions sont basées sur des règles simples.
- Le code peut être étendu pour gérer des mises variables, plus de joueurs, ou une interface graphique.

**Amusez-vous bien et que le meilleur gagne !** ♠️♥️♦️♣️

L'IA n'est pas parfaite : elle ne bluffe pas et ses décisions sont basées sur des règles simples.

Le code peut être étendu pour gérer des mises variables, plus de joueurs, ou une interface graphique.

Amusez-vous bien et que le meilleur gagne ! ♠️♥️♦️♣️
