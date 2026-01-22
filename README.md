import random
import time

def roll_dice():
    return random.randint(1, 6)

def dice_animation():
    print("""
      _______
     |       |
     |   o   |
     |       |
      ‾‾‾‾‾‾‾
    """)
    time.sleep(0.8)

def fate_trial(target):
    events = ["curse", "blessing", "gamble"]
    event = random.choice(events)

    print(f"\n⚖ Fate Trial begins for {target}...")
    time.sleep(1)

    if event == "curse":
        print("💀 A dark curse strikes!")
        return -1

    elif event == "blessing":
        print("✨ A divine blessing is granted!")
        return 1

    else:
        print("🎲 A risky gamble appears!")
        choice = input("Do you accept the gamble? (y/n): ").lower()
        if choice == "y":
            outcome = random.choice([-2, 2])
            print("The gamble is cast...")
            time.sleep(1)
            return outcome
        else:
            print("You avoided the gamble.")
            return 0

def dice_duel():
    print("\n🎲 Rolling the Dice of Destiny...\n")
    dice_animation()

    player_rolls = [roll_dice(), roll_dice()]
    bot_rolls = [roll_dice(), roll_dice()]

    print(f"Your rolls: {player_rolls}")
    print(f"Bot rolls: {bot_rolls}")

    while True:
        try:
            choice = input("Choose a roll to keep (1 or 2): ")
            if choice in ["1", "2"]:
                player_final = player_rolls[int(choice) - 1]
                break
            else:
                print("Invalid choice.")
        except:
            print("Error. Try again.")

    bot_final = random.choice(bot_rolls)

    print(f"\nYou chose: {player_final}")
    print(f"Bot chose: {bot_final}")

    if player_final > bot_final:
        return "player"
    elif player_final < bot_final:
        return "bot"
    else:
        return "tie"

def main_game():
    player_life = 3
    bot_life = 3
    round_num = 1

    print("\n🌌 Welcome to DICE OF DESTINY 🌌")

    while player_life > 0 and bot_life > 0:
        print(f"\n=== Round {round_num} ===")
        print(f"Your Life: {player_life} | Bot Life: {bot_life}")

        result = dice_duel()

        if result == "tie":
            print("⚔ It's a tie! Fate spares both sides.")

        elif result == "player":
            print("🏆 You win the duel! Bot faces fate.")
            bot_life += fate_trial("Bot")

        else:
            print("💥 You lost the duel! You face fate.")
            player_life += fate_trial("You")

        player_life = max(player_life, 0)
        bot_life = max(bot_life, 0)

        while True:
            exit_choice = input("\nContinue the trial? (y/n): ").lower()
            if exit_choice in ["y", "n"]:
                break
            print("Invalid input.")

        if exit_choice == "n":
            print("You walk away from destiny...")
            break

        round_num += 1

    print("\n🏁 GAME OVER")
    if player_life > bot_life:
        print("✨ You conquered fate itself!")
    elif player_life < bot_life:
        print("💀 Fate was not on your side.")
    else:
        print("⚖ Destiny remains balanced.")

if __name__ == "__main__":
    main_game()
# ubiquitous-enigma
