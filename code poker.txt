import random
from collections import Counter

# Déclaration des variables
hero_stack = 5
villain_stack = 5
game_pot = 0
init_cards = [
    '2H', '3H', '4H', '5H', '6H', '7H', '8H', '9H', '10H', 'JH', 'QH', 'KH', 'AH',
    '2D', '3D', '4D', '5D', '6D', '7D', '8D', '9D', '10D', 'JD', 'QD', 'KD', 'AD',
    '2C', '3C', '4C', '5C', '6C', '7C', '8C', '9C', '10C', 'JC', 'QC', 'KC', 'AC',
    '2S', '3S', '4S', '5S', '6S', '7S', '8S', '9S', '10S', 'JS', 'QS', 'KS', 'AS'
]
boards_cards = [] 

# Initialisation des cartes
def init():
    current_cards = init_cards.copy()
    random.shuffle(current_cards)
    return current_cards

# Distribution des cartes aux joueurs
def distribute_cards(current_cards):
    hero_cards = random.sample(current_cards, 2)
    remaining_cards = list(set(current_cards) - set(hero_cards))
    villain_cards = random.sample(remaining_cards, 2)
    return hero_cards, villain_cards

# Affichage des cartes du tableau
def show_cards_board(current_cards, number_cards):
    global boards_cards
    new_cards = random.sample(current_cards, number_cards)
    boards_cards.extend(new_cards)  # Ajouter les nouvelles cartes au tableau
    for card in new_cards:
        current_cards.remove(card)  # Retirer les cartes du paquet
    print("Cartes sur le tableau:", boards_cards)

# Conversion des cartes en valeurs numériques
def card_value(card):
    if card[:-1].isdigit():
        return int(card[:-1])
    return 10 + "TJQKA".index(card[:-1])

# Détermination de la main du joueur
def determine_hand(temp_cards, temp_board):
    all_cards = temp_board + temp_cards
    value_counts = Counter(card[:-1] for card in all_cards)
    sorted_value_counts = sorted(value_counts.values(), reverse=True)
    
    suit_counts = Counter(card[-1] for card in all_cards)
    is_flush = any(count >= 5 for count in suit_counts.values())
    
    unique_values = sorted(set(card_value(card) for card in all_cards))
    is_straight = any(unique_values[i] == unique_values[i + 4] - 4 for i in range(len(unique_values) - 4))
    
    if set(unique_values) == {10, 11, 12, 13, 14}:
        is_straight = True

    # Vérification des combinaisons de mains
    if is_flush and is_straight:
        return "Straight Flush", unique_values
    if 4 in sorted_value_counts:
        return "Four of a Kind", unique_values
    if sorted_value_counts.count(3) == 1 and sorted_value_counts.count(2) == 1:
        return "Full House", unique_values
    if is_flush:
        return "Flush", unique_values
    if is_straight:
        return "Straight", unique_values
    if 3 in sorted_value_counts:
        return "Three of a Kind", unique_values
    if sorted_value_counts.count(2) == 2:
        return "Two Pair", unique_values
    if sorted_value_counts.count(2) == 1:
        return "One Pair", unique_values
    return "High Card", unique_values

def calculate_outs(temp_cards, temp_board):
    all_cards = temp_cards + temp_board
    suit_counts = Counter(card[-1] for card in all_cards)
    
    # Vérifier s'il y a une possibilité de flush
    is_flush = any(count >= 4 for count in suit_counts.values())

    # Vérifier s'il y a une possibilité de straight
    unique_values = sorted(set(card_value(card) for card in all_cards))
    is_straight = any(unique_values[i] == unique_values[i + 4] - 4 for i in range(len(unique_values) - 4))

    # Gérer la condition spéciale pour l'échelle royale (10, J, Q, K, A)
    if set(unique_values) == {10, 11, 12, 13, 14}:
        is_straight = True

    return is_flush, is_straight

def determine_best_cards(temp_cards, temp_board):
    all_cards = temp_cards + temp_board
    value_counts = Counter(card[:-1] for card in all_cards)
    temp_res = []
    num = 0
    qty = 0
    
    for value, count in value_counts.items():
        temp_res.append([count, card_value(value + 'x')])
        
    temp_res.sort(reverse=True)

    for res in temp_res:
        num += (res[0] * res[1])
        qty += res[0]
        if qty >= 5:
            break
    
    return num

# Détermination du gagnant
def determine_winner(player1_cards, player2_cards, temp_board):
    hand1, values1 = determine_hand(player1_cards, temp_board)
    hand2, values2 = determine_hand(player2_cards, temp_board)

    ranking = {
        "Straight Flush": 9,
        "Four of a Kind": 8,
        "Full House": 7,
        "Flush": 6,
        "Straight": 5,
        "Three of a Kind": 4,
        "Two Pair": 3,
        "One Pair": 2,
        "High Card": 1
    }

    if ranking[hand1] > ranking[hand2]:
        return "Joueur 1", hand1
    elif ranking[hand1] < ranking[hand2]:
        return "Joueur IA", hand2
    else:
        best_cards1 = determine_best_cards(hero_cards, temp_board)
        best_cards2 = determine_best_cards(villain_cards, temp_board)

        if best_cards1 > best_cards2:
            return "Joueur 1", hand1
        elif best_cards1 < best_cards2:
            return "Joueur IA", hand2
        else:
            return "Égalité !", None

def determine_action_playerAI(temp_cards, temp_board, temp_pot, is_first):
    hand, values = determine_hand(temp_cards, temp_board)
    ranking = {
        "Straight Flush": 9,
        "Four of a Kind": 8,
        "Full House": 7,
        "Flush": 6,
        "Straight": 5,
        "Three of a Kind": 4,
        "Two Pair": 3,
        "One Pair": 2,
        "High Card": 1
    }
    random_number = random.randint(1, 10)
    
    if is_first:
        if ranking[hand] > 4:
             # Générer un nombre aléatoire entre 1 et 10
            return '3' if temp_pot > 0 and random_number > 5 else '2'
        elif ranking[hand] > 1:
             # Générer un nombre aléatoire entre 1 et 10
            return '3' if random_number > 7 else '2'
        else:
            flush, straight = calculate_outs(temp_cards, temp_board)
            if((flush or straight) and random_number > 9):
                return '2'
            else:
                return '1'
    else:
        if ranking[hand] > 2:
            return '1'
        else:
            flush, straight = calculate_outs(temp_cards, temp_board)
            if (flush or straight) and random_number > 5:
                return '1'
            elif ranking[hand] > 1 and random_number > 7:
                return '1'
            else:
                return '2'
        
# Gestion des actions des joueurs
def decide_actions(next_action, is_hero_turn):
    global hero_stack, villain_stack, boards_cards, game_pot
    action1 = action2 = None  # Initialisation des actions

    if is_hero_turn:
        # Action du joueur 1 (héros)
        print('(Joueur 1) Choisissez une action : 1.Check / 2.Bet / 3.All-in')
        action1 = input()
        
        if action1 == '1':
            print('Joueur 1 décide de Check !')
        elif action1 == '2':
            if hero_stack > 0:
                print('Joueur 1 décide de Bet !')
        elif action1 == '3':
            print('Joueur 1 décide de All-in !')
            #game_pot += hero_stack
            #hero_stack = 0

        # Action du joueur 2 (vilain)
        if action1 == '1':
            print('Joueur IA check aussi...')
        else:
            # Selon les cartes le joueur IA choisira une action 
            print('(Joueur IA) choisit une action...')
            #action2 = input()
            action2 = determine_action_playerAI(villain_cards, boards_cards, game_pot, False)
            if action2 == '1':
                print('Joueur IA décide de Call !')
                if action1 == '3':
                    if hero_stack > villain_stack:
                        game_pot += villain_stack
                        hero_stack -= villain_stack
                        villain_stack = 0
                    elif villain_stack > hero_stack:
                        game_pot += hero_stack
                        villain_stack -= hero_stack
                        hero_stack = 0
                    else: 
                        game_pot += hero_stack
                        game_pot += villain_stack
                        hero_stack = 0
                        villain_stack = 0
                else:
                    villain_stack -= 1
                    hero_stack -= 1
                    game_pot += 2
            elif action2 == '2':
                print('Joueur IA décide de Fold !')
                print('Joueur 1 gagne le pot !')
                hero_stack += game_pot
                game_pot = 0  # Réinitialiser le pot
                return 'pause'
    else:
        # Action du joueur IA (vilain)
        # Selon les cartes le joueur IA choisira une action 
        print('(Joueur IA) choisit une action...')
        #action2 = input()
        action2 = determine_action_playerAI(villain_cards, boards_cards, game_pot, True)
        
        if action2 == '1':
            print('Joueur IA décide de Check !')
        elif action2 == '2':
            if villain_stack > 0:
                print('Joueur IA décide de Bet !')
        elif action2 == '3':
            print('Joueur IA décide de All-in !')

        # Action du joueur 1 (héros)
        if action2 == '1':
            print('Joueur 1 check aussi...')
        else:
            print('(Joueur 1) Choisissez une action : 1.Call / 2.Fold')
            action1 = input()
            if action1 == '1':
                print('Joueur 1 décide de Call !')
                if action2 == '3':
                    if hero_stack > villain_stack:
                        game_pot += villain_stack * 2
                        hero_stack -= villain_stack
                        villain_stack = 0
                    elif villain_stack > hero_stack:
                        game_pot += hero_stack * 2
                        villain_stack -= hero_stack
                        hero_stack = 0
                    else: 
                        game_pot += hero_stack
                        game_pot += villain_stack
                        hero_stack = 0
                        villain_stack = 0
                else:
                    villain_stack -= 1
                    hero_stack -= 1
                    game_pot += 2
            elif action1 == '2':
                print('Joueur 1 décide de Fold !')
                print('Joueur IA gagne le pot !')
                villain_stack += game_pot
                game_pot = 0  # Réinitialiser le pot
                return 'pause'

    return next_action if action1 != '3' and action2 != '3' else 'showdown'

# Boucle de jeu
is_hero_turn = True  # Commence par le héros
while True:
    if hero_stack <= 0 or villain_stack <= 0:
        print("Jeu terminé !")
        break

    print("//////// Jeu commencé ! ////////")
    boards_cards.clear()
    game_pot = 0  # Réinitialiser le pot de jeu
    current_cards = init()
    hero_cards, villain_cards = distribute_cards(current_cards)
    print("//////// Distribution des cartes ////////")
    # Phase Préflop
    action_state = 'flop'
    
    # Phase Flop
    if action_state == 'flop':
        print("//////// Flop //////// ")
        show_cards_board(current_cards, 3)
        print(f'Joueur 1 : {hero_cards} / Stack : {hero_stack}')
        print(f'Joueur IA : {villain_cards} / Stack : {villain_stack}')
        print(f"Montant du pot: {game_pot}")
        print("//////////////////////")
        action_state = decide_actions('turn', is_hero_turn)
    
    # Phase Turn
    if action_state == 'turn':
        print("//////// Turn ////////")
        show_cards_board(current_cards, 1)
        print(f'Joueur 1 : {hero_cards} / Stack : {hero_stack}')
        print(f'Joueur IA : {villain_cards} / Stack : {villain_stack}')
        print(f"Montant du pot: {game_pot}")
        print("//////////////////////")
        action_state = decide_actions('river', is_hero_turn)

    # Phase River
    if action_state == 'river':
        print("//////// River ////////")
        show_cards_board(current_cards, 1)
        print(f'Joueur 1 : {hero_cards} / Stack : {hero_stack}')
        print(f'Joueur IA : {villain_cards} / Stack : {villain_stack}')
        print(f"Montant du pot: {game_pot}")
        print("//////////////////////")
        action_state = decide_actions('showdown', is_hero_turn)

    # Phase Showdown
    if action_state == 'showdown':
        print("//////// Showdown ////////")
        if(len(boards_cards) < 5):
            show_cards_board(current_cards, (5-len(boards_cards)))
        winner, hand = determine_winner(hero_cards, villain_cards, boards_cards)
        print("Cartes sur le tableau:", boards_cards)
        print(f"Joueur 1 cartes : {hero_cards}")
        print(f"Joueur IA cartes : {villain_cards}")
        print(f"Montant du pot: {game_pot}")
        print("//////////////////////")
        if winner != "Égalité !":
            print(f"{winner} gagne avec {hand}")
            if winner == "Joueur 1":
                hero_stack += game_pot  # Attribuer le pot au gagnant
            else:
                villain_stack += game_pot  # Attribuer le pot au gagnant
        else:
            print("Pas de gagnant cette fois.")
        
        game_pot = 0  # Réinitialiser le pot
        action_state = 'pause'
        
    if action_state == 'pause':
        print('Passez au jeu suivant ? (o/n)')
        if input().lower() != 'o':
            break

    # Alterner les rôles pour le prochain tour
    is_hero_turn = not is_hero_turn
    