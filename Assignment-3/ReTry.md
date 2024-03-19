## User
Give code for 2 player yahtzee game 
## Chatgpt
Below is a simple Python code for a 2-player Yahtzee game:

```python
import random

# Function to roll dice
def roll_dice(num_dice):
    return [random.randint(1, 6) for _ in range(num_dice)]

# Function to count occurrences of each face value
def count_dice(dice):
    counts = [0] * 7
    for die in dice:
        counts[die] += 1
    return counts[1:]

# Function to score a Yahtzee
def score_yahtzee(dice):
    if max(dice) == min(dice):
        return 50
    return 0

# Function to score a specific face
def score_face(dice, face):
    return dice.count(face) * face

# Function to score a specific category
def score_category(dice, category):
    if category == 'Yahtzee':
        return score_yahtzee(dice)
    elif category in ('Ones', 'Twos', 'Threes', 'Fours', 'Fives', 'Sixes'):
        return score_face(dice, int(category[-1]))
    else:
        return sum(dice)

# Function to display current scorecard
def display_scorecard(scorecard):
    print("\nCurrent Scorecard:")
    for category, score in scorecard.items():
        print(f"{category}: {score}")
    print()

# Main function to play the game
def play_yahtzee():
    players = ['Player 1', 'Player 2']
    scorecards = {player: {category: None for category in ('Ones', 'Twos', 'Threes', 'Fours', 'Fives', 'Sixes', 'Three of a Kind', 'Four of a Kind', 'Full House', 'Small Straight', 'Large Straight', 'Yahtzee', 'Chance')} for player in players}

    for round_num in range(13):
        print(f"Round {round_num + 1}")
        for player in players:
            print(f"{player}'s turn:")
            input("Press Enter to roll the dice...")
            dice = roll_dice(5)
            print(f"Roll: {dice}")
            display_scorecard(scorecards[player])
            category = input("Enter category to score: ").strip()
            while scorecards[player][category] is not None:
                category = input("Category already scored. Enter another category: ").strip()
            score = score_category(dice, category)
            scorecards[player][category] = score
            display_scorecard(scorecards[player])
    
    # Final scoring
    for player in players:
        total_score = sum(score for score in scorecards[player].values() if score is not None)
        print(f"{player}'s Final Score: {total_score}")

if __name__ == "__main__":
    play_yahtzee()
```

This code provides a simple console-based implementation of a 2-player Yahtzee game. Players take turns rolling dice and choosing categories to score. The game consists of 13 rounds, after which the final scores are calculated and displayed. Each player's scorecard is displayed after each turn to keep track of their progress.

## User
give using object oriantation also play for 13 rounds giving the summed up score each round 
All the conditions should be there 

## CHatGPT
Here's an updated version of the Yahtzee game using object-oriented programming (OOP) principles and scoring for 13 rounds with all the necessary conditions:

```python
import random

class YahtzeeGame:
    def __init__(self, players):
        self.players = players
        self.round_scores = {player: [] for player in players}
        self.scorecards = {player: {category: None for category in ('Ones', 'Twos', 'Threes', 'Fours', 'Fives', 'Sixes', 'Three of a Kind', 'Four of a Kind', 'Full House', 'Small Straight', 'Large Straight', 'Yahtzee', 'Chance')} for player in players}

    def roll_dice(self, num_dice):
        return [random.randint(1, 6) for _ in range(num_dice)]

    def count_dice(self, dice):
        counts = [0] * 7
        for die in dice:
            counts[die] += 1
        return counts[1:]

    def score_yahtzee(self, dice):
        if max(dice) == min(dice):
            return 50
        return 0

    def score_face(self, dice, face):
        return dice.count(face) * face

    def score_category(self, dice, category):
        if category == 'Yahtzee':
            return self.score_yahtzee(dice)
        elif category in ('Ones', 'Twos', 'Threes', 'Fours', 'Fives', 'Sixes'):
            return self.score_face(dice, int(category[-1]))
        elif category == 'Three of a Kind':
            counts = self.count_dice(dice)
            if max(counts) >= 3:
                return sum(dice)
            return 0
        elif category == 'Four of a Kind':
            counts = self.count_dice(dice)
            if max(counts) >= 4:
                return sum(dice)
            return 0
        elif category == 'Full House':
            counts = self.count_dice(dice)
            if 2 in counts and 3 in counts:
                return 25
            return 0
        elif category == 'Small Straight':
            if sorted(set(dice)) in ([1, 2, 3, 4], [2, 3, 4, 5], [3, 4, 5, 6]):
                return 30
            return 0
        elif category == 'Large Straight':
            if sorted(set(dice)) in ([1, 2, 3, 4, 5], [2, 3, 4, 5, 6]):
                return 40
            return 0
        elif category == 'Chance':
            return sum(dice)
        return 0

    def display_scorecard(self, player):
        print("\nCurrent Scorecard for", player)
        for category, score in self.scorecards[player].items():
            print(f"{category}: {score}")
        print()

    def play_round(self, round_num):
        round_scores_sum = {}
        for player in self.players:
            print(f"\nRound {round_num} - {player}'s turn:")
            input("Press Enter to roll the dice...")
            dice = self.roll_dice(5)
            print(f"Roll: {dice}")
            self.display_scorecard(player)
            category = input("Enter category to score: ").strip()
            while self.scorecards[player][category] is not None:
                category = input("Category already scored. Enter another category: ").strip()
            score = self.score_category(dice, category)
            self.scorecards[player][category] = score
            self.round_scores[player].append(score)
            self.display_scorecard(player)
            round_scores_sum[player] = sum(self.round_scores[player])
        print("\nRound", round_num, "Scores:", round_scores_sum)

    def play(self):
        for round_num in range(1, 14):
            self.play_round(round_num)
        print("\nFinal Scores:")
        for player in self.players:
            total_score = sum(score for score in self.scorecards[player].values() if score is not None)
            print(f"{player}: {total_score}")

if __name__ == "__main__":
    players = ['Player 1', 'Player 2']
    yahtzee_game = YahtzeeGame(players)
    yahtzee_game.play()
```

This version of the code uses a `YahtzeeGame` class to encapsulate the game logic. Each player has their own scorecard, and the game proceeds through 13 rounds, with scores displayed after each round and final scores displayed at the end. All the necessary conditions for scoring are implemented within the `score_category` method.

## User
The code is not asking for re rolls after 2 re rolls are done or when user clicks exit the options for scoring should be shown

## CHatGPT
Apologies for the oversight. Below is the updated code that allows players to choose whether to reroll dice up to two times and to show scoring options after choosing not to reroll or after the second reroll:

```python
import random

class YahtzeeGame:
    def __init__(self, players):
        self.players = players
        self.round_scores = {player: [] for player in players}
        self.scorecards = {player: {category: None for category in ('Ones', 'Twos', 'Threes', 'Fours', 'Fives', 'Sixes', 'Three of a Kind', 'Four of a Kind', 'Full House', 'Small Straight', 'Large Straight', 'Yahtzee', 'Chance')} for player in players}

    def roll_dice(self, num_dice):
        return [random.randint(1, 6) for _ in range(num_dice)]

    def count_dice(self, dice):
        counts = [0] * 7
        for die in dice:
            counts[die] += 1
        return counts[1:]

    def score_yahtzee(self, dice):
        if max(dice) == min(dice):
            return 50
        return 0

    def score_face(self, dice, face):
        return dice.count(face) * face

    def score_category(self, dice, category):
        if category == 'Yahtzee':
            return self.score_yahtzee(dice)
        elif category in ('Ones', 'Twos', 'Threes', 'Fours', 'Fives', 'Sixes'):
            return self.score_face(dice, int(category[-1]))
        elif category == 'Three of a Kind':
            counts = self.count_dice(dice)
            if max(counts) >= 3:
                return sum(dice)
            return 0
        elif category == 'Four of a Kind':
            counts = self.count_dice(dice)
            if max(counts) >= 4:
                return sum(dice)
            return 0
        elif category == 'Full House':
            counts = self.count_dice(dice)
            if 2 in counts and 3 in counts:
                return 25
            return 0
        elif category == 'Small Straight':
            if sorted(set(dice)) in ([1, 2, 3, 4], [2, 3, 4, 5], [3, 4, 5, 6]):
                return 30
            return 0
        elif category == 'Large Straight':
            if sorted(set(dice)) in ([1, 2, 3, 4, 5], [2, 3, 4, 5, 6]):
                return 40
            return 0
        elif category == 'Chance':
            return sum(dice)
        return 0

    def display_scorecard(self, player):
        print("\nCurrent Scorecard for", player)
        for category, score in self.scorecards[player].items():
            if score is None:
                print(f"{category}: Not scored")
            else:
                print(f"{category}: {score}")
        print()

    def reroll_dice(self, dice, num_rerolls):
        print("Current Dice:", dice)
        for _ in range(num_rerolls):
            reroll_indices = list(map(int, input("Enter the indices of dice to reroll (space-separated, starting from 1) or '0' to stop rerolling: ").split()))
            if reroll_indices == [0]:
                break
            for index in reroll_indices:
                dice[index - 1] = random.randint(1, 6)
            print("New Dice:", dice)
        return dice

    def play_round(self, round_num):
        round_scores_sum = {}
        for player in self.players:
            print(f"\nRound {round_num} - {player}'s turn:")
            input("Press Enter to roll the dice...")
            dice = self.roll_dice(5)
            print(f"Roll: {dice}")
            for reroll in range(2):
                reroll_choice = input("Do you want to reroll? (yes/no): ").strip().lower()
                if reroll_choice == 'yes':
                    dice = self.reroll_dice(dice, 5 - len(dice))
                else:
                    break
            self.display_scorecard(player)
            category = input("Enter category to score: ").strip()
            while self.scorecards[player][category] is not None:
                category = input("Category already scored. Enter another category: ").strip()
            score = self.score_category(dice, category)
            self.scorecards[player][category] = score
            self.round_scores[player].append(score)
            self.display_scorecard(player)
            round_scores_sum[player] = sum(self.round_scores[player])
        print("\nRound", round_num, "Scores:", round_scores_sum)

    def play(self):
        for round_num in range(1, 14):
            self.play_round(round_num)
        print("\nFinal Scores:")
        for player in self.players:
            total_score = sum(score for score in self.scorecards[player].values() if score is not None)
            print(f"{player}: {total_score}")

if __name__ == "__main__":
    players = ['Player 1', 'Player 2']
    yahtzee_game = YahtzeeGame(players)
    yahtzee_game.play()
```

In this version, players are prompted whether they want to reroll after each roll. They can choose to reroll dice up to two times, and after the second reroll or when they choose not to reroll, they are prompted to select a category to score.
## User
Re roll is still not correct also in the yahtzee game during re roll which dies to reroll should also be asked (indeces based) 
## ChatGPT
Apologies for the oversight. I've updated the code to correctly handle rerolls and to ask the player which dice to reroll during each reroll phase. Here's the revised version:

```python
import random

class YahtzeeGame:
    def __init__(self, players):
        self.players = players
        self.round_scores = {player: [] for player in players}
        self.scorecards = {player: {category: None for category in ('Ones', 'Twos', 'Threes', 'Fours', 'Fives', 'Sixes', 'Three of a Kind', 'Four of a Kind', 'Full House', 'Small Straight', 'Large Straight', 'Yahtzee', 'Chance')} for player in players}

    def roll_dice(self, num_dice):
        return [random.randint(1, 6) for _ in range(num_dice)]

    def count_dice(self, dice):
        counts = [0] * 7
        for die in dice:
            counts[die] += 1
        return counts[1:]

    def score_yahtzee(self, dice):
        if max(dice) == min(dice):
            return 50
        return 0

    def score_face(self, dice, face):
        return dice.count(face) * face

    def score_category(self, dice, category):
        if category == 'Yahtzee':
            return self.score_yahtzee(dice)
        elif category in ('Ones', 'Twos', 'Threes', 'Fours', 'Fives', 'Sixes'):
            return self.score_face(dice, int(category[-1]))
        elif category == 'Three of a Kind':
            counts = self.count_dice(dice)
            if max(counts) >= 3:
                return sum(dice)
            return 0
        elif category == 'Four of a Kind':
            counts = self.count_dice(dice)
            if max(counts) >= 4:
                return sum(dice)
            return 0
        elif category == 'Full House':
            counts = self.count_dice(dice)
            if 2 in counts and 3 in counts:
                return 25
            return 0
        elif category == 'Small Straight':
            if sorted(set(dice)) in ([1, 2, 3, 4], [2, 3, 4, 5], [3, 4, 5, 6]):
                return 30
            return 0
        elif category == 'Large Straight':
            if sorted(set(dice)) in ([1, 2, 3, 4, 5], [2, 3, 4, 5, 6]):
                return 40
            return 0
        elif category == 'Chance':
            return sum(dice)
        return 0

    def display_scorecard(self, player):
        print("\nCurrent Scorecard for", player)
        for category, score in self.scorecards[player].items():
            if score is None:
                print(f"{category}: Not scored")
            else:
                print(f"{category}: {score}")
        print()

    def reroll_dice(self, dice):
        reroll_indices = list(map(int, input("Enter the indices of dice to reroll (space-separated, starting from 1) or '0' to stop rerolling: ").split()))
        if reroll_indices == [0]:
            return dice
        for index in reroll_indices:
            dice[index - 1] = random.randint(1, 6)
        print("New Dice:", dice)
        return dice

    def play_round(self, round_num):
        round_scores_sum = {}
        for player in self.players:
            print(f"\nRound {round_num} - {player}'s turn:")
            input("Press Enter to roll the dice...")
            dice = self.roll_dice(5)
            print(f"Roll: {dice}")
            for reroll in range(2):
                reroll_choice = input("Do you want to reroll? (yes/no): ").strip().lower()
                if reroll_choice == 'yes':
                    dice = self.reroll_dice(dice)
                    print(f"Reroll {reroll + 1}: {dice}")
                else:
                    break
            self.display_scorecard(player)
            category = input("Enter category to score: ").strip()
            while self.scorecards[player][category] is not None:
                category = input("Category already scored. Enter another category: ").strip()
            score = self.score_category(dice, category)
            self.scorecards[player][category] = score
            self.round_scores[player].append(score)
            self.display_scorecard(player)
            round_scores_sum[player] = sum(self.round_scores[player])
        print("\nRound", round_num, "Scores:", round_scores_sum)

    def play(self):
        for round_num in range(1, 14):
            self.play_round(round_num)
        print("\nFinal Scores:")
        for player in self.players:
            total_score = sum(score for score in self.scorecards[player].values() if score is not None)
            print(f"{player}: {total_score}")

if __name__ == "__main__":
    players = ['Player 1', 'Player 2']
    yahtzee_game = YahtzeeGame(players)
    yahtzee_game.play()
```

Now the player is asked for dice indices to reroll during each reroll phase, and the rerolled dice are displayed. Also, the rerolling mechanism has been fixed to correctly handle rerolls.

## User
Give the scoring categories input as numbers for easier

## ChatGPT
Certainly! Below is the updated code where scoring categories are represented by numbers instead of their names to make it easier for the player to input their choice:

```python
import random

class YahtzeeGame:
    def __init__(self, players):
        self.players = players
        self.round_scores = {player: [] for player in players}
        self.scorecards = {player: {i: None for i in range(1, 14)} for player in players}

    def roll_dice(self, num_dice):
        return [random.randint(1, 6) for _ in range(num_dice)]

    def count_dice(self, dice):
        counts = [0] * 7
        for die in dice:
            counts[die] += 1
        return counts[1:]

    def score_yahtzee(self, dice):
        if max(dice) == min(dice):
            return 50
        return 0

    def score_face(self, dice, face):
        return dice.count(face) * face

    def score_category(self, dice, category):
        if category == 13:
            return sum(dice)
        elif category in (1, 2, 3, 4, 5, 6):
            return self.score_face(dice, category)
        elif category == 7:
            counts = self.count_dice(dice)
            if max(counts) >= 3:
                return sum(dice)
            return 0
        elif category == 8:
            counts = self.count_dice(dice)
            if max(counts) >= 4:
                return sum(dice)
            return 0
        elif category == 9:
            counts = self.count_dice(dice)
            if 2 in counts and 3 in counts:
                return 25
            return 0
        elif category == 10:
            if sorted(set(dice)) in ([1, 2, 3, 4], [2, 3, 4, 5], [3, 4, 5, 6]):
                return 30
            return 0
        elif category == 11:
            if sorted(set(dice)) in ([1, 2, 3, 4, 5], [2, 3, 4, 5, 6]):
                return 40
            return 0
        elif category == 12:
            if max(dice) == min(dice):
                return 50
            return 0
        return 0

    def display_scorecard(self, player):
        print("\nCurrent Scorecard for", player)
        for category, score in self.scorecards[player].items():
            if score is None:
                print(f"{category}: Not scored")
            else:
                print(f"{category}: {score}")
        print()

    def reroll_dice(self, dice):
        reroll_indices = list(map(int, input("Enter the indices of dice to reroll (space-separated, starting from 1) or '0' to stop rerolling: ").split()))
        if reroll_indices == [0]:
            return dice
        for index in reroll_indices:
            dice[index - 1] = random.randint(1, 6)
        print("New Dice:", dice)
        return dice

    def play_round(self, round_num):
        round_scores_sum = {}
        for player in self.players:
            print(f"\nRound {round_num} - {player}'s turn:")
            input("Press Enter to roll the dice...")
            dice = self.roll_dice(5)
            print(f"Roll: {dice}")
            for reroll in range(2):
                reroll_choice = input("Do you want to reroll? (yes/no): ").strip().lower()
                if reroll_choice == 'yes':
                    dice = self.reroll_dice(dice)
                    print(f"Reroll {reroll + 1}: {dice}")
                else:
                    break
            self.display_scorecard(player)
            category = int(input("Enter category to score (1-13): ").strip())
            while self.scorecards[player][category] is not None:
                category = int(input("Category already scored. Enter another category (1-13): ").strip())
            score = self.score_category(dice, category)
            self.scorecards[player][category] = score
            self.round_scores[player].append(score)
            self.display_scorecard(player)
            round_scores_sum[player] = sum(self.round_scores[player])
        print("\nRound", round_num, "Scores:", round_scores_sum)

    def play(self):
        for round_num in range(1, 14):
            self.play_round(round_num)
        print("\nFinal Scores:")
        for player in self.players:
            total_score = sum(score for score in self.scorecards[player].values() if score is not None)
            print(f"{player}: {total_score}")

if __name__ == "__main__":
    players = ['Player 1', 'Player 2']
    yahtzee_game = YahtzeeGame(players)
    yahtzee_game.play()
```

Now, when prompted to enter a category to score, the player can simply input a number from 1 to 13 for the corresponding category. This should make it more intuitive for the player to choose their scoring options.
