# Battleship
import random

def show_menu():
    print("\n" + "="*40)
    print("        BATTLESHIP GAME")
    print("="*40)
    print("1. Start New Game")
    print("2. Game Instructions")
    print("3. Exit")
    print("="*40)
    
    while True:
        try:
            choice = int(input("Enter your choice (1-3): "))
            if 1 <= choice <= 3:
                return choice
            print("Please enter a number between 1 and 3")
        except ValueError:
            print("Please enter a valid number!")

def show_instructions():
    print("\n" + "="*50)
    print("           GAME INSTRUCTIONS")
    print("="*50)
    print("• Each player has a 5x5 grid with 3 hidden ships")
    print("• Players take turns guessing coordinates (0-4)")
    print("• 'O' = Ocean (untouched)")
    print("• 'X' = Miss")
    print("• 'H' = Hit (ship destroyed)")
    print("• First to sink all opponent's ships wins!")
    print("• Enter coordinates as numbers (0-4)")
    print("="*50)
    input("Press Enter to return to menu...")

def create_board():
    return [["O" for _ in range(5)] for _ in range(5)]

def print_board(board, player_name=""):
    if player_name:
        print(f"\n{player_name}'s Board:")
    for row in board:
        print(" ".join(row))

def place_ships(board, num_ships):
    ships = []
    for _ in range(num_ships):
        ship_row = random.randint(0, 4)
        ship_col = random.randint(0, 4)
        while (ship_row, ship_col) in ships:
            ship_row = random.randint(0, 4)
            ship_col = random.randint(0, 4)
        ships.append((ship_row, ship_col))
    return ships

def get_valid_input(prompt, min_val, max_val):
    while True:
        try:
            value = int(input(prompt))
            if min_val <= value <= max_val:
                return value
            print(f"Please enter a number between {min_val} and {max_val}")
        except ValueError:
            print("Please enter a valid number!")

def play_game():
    num_ships = 3
    player1_board = create_board()
    player1_ships = place_ships(player1_board, num_ships)
    
    player2_board = create_board()
    player2_ships = place_ships(player2_board, num_ships)
    
    turn = 1
    while True:
        if turn % 2 != 0:
            print("\n" + "-"*30)
            print("PLAYER 1'S TURN")
            print("-"*30)
            guess_row = get_valid_input("Guess Row (0-4): ", 0, 4)
            guess_col = get_valid_input("Guess Col (0-4): ", 0, 4)
            
            if (guess_row, guess_col) in player2_ships:
                print("💥 Player 1 HIT a ship!")
                player2_ships.remove((guess_row, guess_col))
                player2_board[guess_row][guess_col] = "H"
                if not player2_ships:
                    print("🎉 Player 1 WINS! All enemy ships sunk!")
                    break
            else:
                if player2_board[guess_row][guess_col] in ["X", "H"]:
                    print("You already guessed that position!")
                else:
                    print("💦 Player 1 missed!")
                    player2_board[guess_row][guess_col] = "X"
            print_board(player2_board, "Enemy")
        else:
            print("\n" + "-"*30)
            print("PLAYER 2'S TURN")
            print("-"*30)
            guess_row = get_valid_input("Guess Row (0-4): ", 0, 4)
            guess_col = get_valid_input("Guess Col (0-4): ", 0, 4)
            
            if (guess_row, guess_col) in player1_ships:
                print("💥 Player 2 HIT a ship!")
                player1_ships.remove((guess_row, guess_col))
                player1_board[guess_row][guess_col] = "H"
                if not player1_ships:
                    print("🎉 Player 2 WINS! All enemy ships sunk!")
                    break
            else:
                if player1_board[guess_row][guess_col] in ["X", "H"]:
                    print("You already guessed that position!")
                else:
                    print("💦 Player 2 missed!")
                    player1_board[guess_row][guess_col] = "X"
            print_board(player1_board, "Enemy")
        
        turn += 1

def battleship_game():
    while True:
        choice = show_menu()
        
        if choice == 1:
            play_game()
            input("\nGame over! Press Enter to return to menu...")
        elif choice == 2:
            show_instructions()
        elif choice == 3:
            print("Thanks for playing! Goodbye! 👋")
            break


battleship_game()
