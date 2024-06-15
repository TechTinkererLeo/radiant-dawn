import random

# Define the Player class
class Player:
    def __init__(self, name):
        self.name = name
        self.health = 100
        self.inventory = []

    def take_damage(self, amount):
        self.health -= amount
        print(f"{self.name} takes {amount} damage. Health is now {self.health}.")

    def heal(self, amount):
        self.health += amount
        print(f"{self.name} heals {amount} health. Health is now {self.health}.")

    def pick_item(self, item):
        self.inventory.append(item)
        print(f"{self.name} picks up {item}.")

# Define the Location class
class Location:
    def __init__(self, name, description):
        self.name = name
        self.description = description
        self.items = []
        self.enemy = None

    def set_items(self, items):
        self.items = items

    def set_enemy(self, enemy):
        self.enemy = enemy

# Define the Enemy class
class Enemy:
    def __init__(self, name, health, attack):
        self.name = name
        self.health = health
        self.attack = attack

    def take_damage(self, amount):
        self.health -= amount
        print(f"{self.name} takes {amount} damage. Health is now {self.health}.")

    def is_alive(self):
        return self.health > 0

# Function to create game locations
def create_locations():
    forest = Location("Forest", "A dark and eerie forest.")
    cave = Location("Cave", "A damp and cold cave.")
    village = Location("Village", "A small, peaceful village.")
    
    forest.set_items(["sword", "shield"])
    cave.set_items(["potion", "map"])
    village.set_items(["food", "water"])

    forest.set_enemy(Enemy("Goblin", 30, 10))
    cave.set_enemy(Enemy("Wolf", 40, 15))
    village.set_enemy(Enemy("Bandit", 50, 20))

    return [forest, cave, village]

# Main game loop
def game():
    print("Welcome to Radiant Dawn!")
    player_name = input("Enter your name: ")
    player = Player(player_name)

    locations = create_locations()

    current_location = locations[0]

    while True:
        print(f"\nYou are at the {current_location.name}.")
        print(current_location.description)

        if current_location.items:
            print("You see the following items:")
            for item in current_location.items:
                print(f"- {item}")

        if current_location.enemy and current_location.enemy.is_alive():
            print(f"You encounter a {current_location.enemy.name}!")
            action = input("Do you want to (fight) or (flee)? ").lower()
            if action == "fight":
                while current_location.enemy.is_alive():
                    player.take_damage(current_location.enemy.attack)
                    current_location.enemy.take_damage(random.randint(10, 30))
                    if not player.health > 0:
                        print(f"{player.name} has been defeated!")
                        return
                print(f"{player.name} has defeated the {current_location.enemy.name}!")
            else:
                print(f"{player.name} flees from the {current_location.enemy.name}!")
                continue

        action = input("Do you want to (explore) or (pick up) an item? ").lower()
        if action == "explore":
            next_location = random.choice(locations)
            current_location = next_location
        elif action == "pick up":
            item = input("Enter the item you want to pick up: ").lower()
            if item in current_location.items:
                player.pick_item(item)
                current_location.items.remove(item)
            else:
                print("Item not found.")

        if player.health <= 0:
            print(f"{player.name} has perished.")
            break

if __name__ == "__main__":
    game()
