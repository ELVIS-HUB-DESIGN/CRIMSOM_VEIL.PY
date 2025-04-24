# CRIMSOM_VEIL.PY
import time
import random

# Character and story setup
class Character:
    def __init__(self, name, role, secret):
        self.name = name
        self.role = role
        self.secret = secret
        self.trust = 50

    def speak(self, dialogue):
        print(f"{self.name}: {dialogue}")

class Game:
    def __init__(self):
        self.characters = self.create_characters()
        self.day = 1
        self.sanity = 100
        self.found_clues = []
        self.inventory = []
        self.sound_effects = {
            'storm': 'storm.mp3',
            'creak': 'creak.mp3',
            'whisper': 'whisper.mp3'
        }

    def create_characters(self):
        return [
            Character("Adrian Cole", "Thriller Novelist", "Covered up a war crime"),
            Character("Sylvia Renard", "Gothic Romance Author", "Had an affair with Elias"),
            Character("Tobias Finch", "Satirist", "Knows about secret passages"),
            Character("Dr. Helena Moorcroft", "Neuroscientist", "Experimented on patients"),
            Character("Noah Blackwell", "Fantasy Writer", "Plagiarized Elias's work"),
            Character("Jun Park", "Poet", "Knows the asylum's occult history"),
            Character("Clara Vale", "Podcaster", "Obsessed with Elias's murder theories")
        ]

    def intro(self):
        print("\nWelcome to Crimson Veil.")
        time.sleep(1)
        print("You are Maren Voss, a former profiler trapped at Briar Hollow Estate.")
        time.sleep(1)
        print("Elias Thorne is dead. Everyone is a suspect. Including you.")
        time.sleep(2)
        self.start_day()

    def start_day(self):
        print(f"\nDay {self.day} begins. Fog blankets the estate.")
        print("You must choose your next move:\n")
        print("1. Investigate Elias's study")
        print("2. Interrogate a guest")
        print("3. Rest (restore sanity)")
        choice = input("What do you do? ")

        if choice == "1":
            self.investigate_study()
        elif choice == "2":
            self.interrogate_menu()
        elif choice == "3":
            self.rest()
        else:
            print("Invalid choice. The walls creak ominously.")
            self.start_day()

    def investigate_study(self):
        print("\nYou enter the dimly-lit study. Something feels off.")
        time.sleep(1)
        print("You find a torn manuscript page and a trail of red wax...")
        self.found_clues.append("Torn Page")
        self.inventory.append("Torn Page")
        self.sanity -= 5
        self.end_day()

    def interrogate_menu(self):
        print("\nWho would you like to interrogate?")
        for idx, char in enumerate(self.characters):
            print(f"{idx+1}. {char.name} - {char.role}")
        try:
            choice = int(input("Choose a number: ")) - 1
            self.interrogate(self.characters[choice])
        except (IndexError, ValueError):
            print("That person fades into the mist... Try again.")
            self.interrogate_menu()

    def interrogate(self, character):
        print(f"\nYou sit across from {character.name}.")
        character.speak("What do you want to know?")
        time.sleep(1)
        print("You notice a flicker of something in their eyes.")
        self.found_clues.append(f"{character.name}'s Reaction")
        character.trust -= 10
        self.sanity -= 2
        self.end_day()

    def rest(self):
        print("\nYou return to your room, haunted by the whispers...")
        self.sanity = min(self.sanity + 10, 100)
        self.end_day()

    def end_day(self):
        self.day += 1
        if self.sanity <= 0:
            print("\nYou collapse, overcome by hallucinations. The storm swallows you whole...")
            self.conclude_game("bad")
        elif self.day > 3:
            self.conclude_game()
        else:
            self.start_day()

    def conclude_game(self, outcome="good"):
        print("\nThe storm breaks. The truth claws its way from the shadows.")
        print("Your clues:")
        for clue in self.found_clues:
            print(f"- {clue}")
        print("Who do you accuse?")
        for idx, char in enumerate(self.characters):
            print(f"{idx+1}. {char.name}")

        try:
            final_choice = int(input("Enter number: ")) - 1
            print(f"\nYou accuse {self.characters[final_choice].name}.")
            if outcome == "bad":
                print("But did you get it right...? The veil lifts. The truth waits.")
            else:
                print("The veil lifts. You see through the lies. The truth has been revealed.")
        except:
            print("The truth slips through your fingers like smoke.")
        print("\n--- END ---")

if __name__ == "__main__":
    game = Game()
    game.intro()
