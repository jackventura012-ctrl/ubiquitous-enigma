import random

def draw_card():
    """Draw a random card from a standard deck (values only)."""
    cards = ["A", "2", "3", "4", "5", "6", "7", "8", "9", "10", "J", "Q", "K"]
    return random.choice(cards)

def card_value(card):
    """Return the Trinity Draw value of a card."""
    if card == "A":
        return 1
    elif card in ["10", "J", "Q", "K"]:
        return 0
    else:
        return int(card)

def hand_value(hand):
    """Calculate total hand value using mod 10 rule."""
    total = sum(card_value(card) for card in hand)
    return total % 10

def player_turn(hand):
    """Handle the player's optional third card decision."""
    value = hand_value(hand)
    print(f"\nYour hand: {hand} | Value: {value}")

    if value <= 5:
        while True:
            choice = input("Draw a third card? (y/n): ").lower()
            if choice == "y":
                card = draw_card()
                hand.append(card)
                print(f"You drew: {card}")
                break
            elif choice == "n":
                print("You chose to stand.")
                break
            else:
                print("Invalid input. Please enter y or n.")
    else:
        print("You must stand (value is above 5).")

    return hand

def house_turn(hand):
    """House draws automatically based on fixed rules."""
    value = hand_value(hand)
    print(f"\nHouse hand: {hand} | Value: {value}")

    if value <= 5:
        card = draw_card()
        hand.append(card)
        print(f"House draws a card.")
    else:
        print("House stands.")

    return hand

def determine_winner(player_hand, house_hand):
    """Determine the round winner."""
    player_value = hand_value(player_hand)
    house_value = hand_value(house_hand)

    print("\n--- FINAL HANDS ---")
    print(f"Your hand: {player_hand} | Value: {player_value}")
    print(f"House hand: {house_hand} | Value: {house_value}")

    if player_value > 9:
        return "house"
    if house_value > 9:
        return "player"

    if player_value > house_value:
        return "player"
    elif player_value < house_value:
        return "house"
    else:
        return "tie"

def main():
    player_lives = 5
    house_lives = 5
    round_number = 1

    print("\n🃏 WELCOME TO TRINITY DRAW 🃏")
    print("Reach the closest value to 9 without going over.\n")

    while player_lives > 0 and house_lives > 0:
        print(f"\n=== ROUND {round_number} ===")
        print(f"Your Lives: {player_lives} | House Lives: {house_lives}")

        # Initial deal
        player_hand = [draw_card(), draw_card()]
        house_hand = [draw_card(), draw_card()]

        # Player and house turns
        player_hand = player_turn(player_hand)
        house_hand = house_turn(house_hand)

        # Determine winner
        result = determine_winner(player_hand, house_hand)

        if result == "player":
            print("\n🎉 You win the round!")
            house_lives -= 1
        elif result == "house":
            print("\n💀 House wins the round.")
            player_lives -= 1
        else:
            print("\n⚖ It's a tie. No lives lost.")

        # Exit option
        while True:
            exit_choice = input("\nContinue playing? (y/n): ").lower()
            if exit_choice in ["y", "n"]:
                break
            print("Invalid choice.")

        if exit_choice == "n":
            print("\nYou chose to leave the table.")
            break

        round_number += 1

    # Game over
    print("\n🏁 GAME OVER")
    if player_lives > house_lives:
        print("🏆 You outlasted the House!")
    elif player_lives < house_lives:
        print("💀 The House claims victory.")
    else:
        print("⚖ The game ends in balance.")


if __name__ == "__main__":
    main()
