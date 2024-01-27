# 19CSE201 - Advanced Programming
![](https://img.shields.io/badge/Batch-22CYS-lightgreen) ![](https://img.shields.io/badge/UG-blue) ![](https://img.shields.io/badge/Subject-AdP-blue)
![](https://img.shields.io/badge/-HPOJ-brown)

## Hangman

```
'''

 * Amrita Vishwa Vidyapeetham
 * TIFAC-CORE in Cyber Security
 * 19CSE201 - Advance Programming Lab Project
 * Terminal based Hangman
 * Authors: Adithya N S, Anaswara Suresh M K, C S Amritha, R Sruthi
 * Created Date: 22-January-2024
 
'''
import random

class colors:
    class fg:
        black = '\033[30m'
        red = '\033[31m'
        green = '\033[32m'
        orange = '\033[33m'
        blue = '\033[34m'
        purple = '\033[35m'
        cyan = '\033[36m'
        lightgrey = '\033[37m'
        darkgrey = '\033[90m'
        lightred = '\033[91m'
        lightgreen = '\033[92m'
        yellow = '\033[93m'
        lightblue = '\033[94m'
        pink = '\033[95m'
        lightcyan = '\033[96m'

class Hangman:
    '''
    Base class -Hangman. All the other classes such as the easy hangman, medium hangman and hard hangman inherits all the properties from this class.It uses multiple inheritance.
    '''
    def __init__(self, lines, a, b): #list of lines - lines. a,b- range within which the word and hint should belong to.
        self.rand_num = lines[random.randint(a, b)] # returns the element at the random index.
        self.ls_word_hint = self.rand_num.split(",") # splits the element at ','. This returns a list with 2 items. In zeroth index is the word and in the first index is the hint.
        self.word = self.ls_word_hint[0].lower() 
        self.hint = self.ls_word_hint[1]
        self.guesses = [] # to store all the guesses made by the player.
        self.wrongmoves = 0
        self.hang_man = [
            """
              ___
              |   |
                  |
                  |
                  |
                  |
            __|
            """,
            """
              ___
              |   |
              O   |
                  |
                  |
                  |
            __|
            """,
            """
              ___
              |   |
              O   |
              |   |
                  |
                  |
            __|
            """,
            """
              ___
              |   |
              O   |
             /|   |
                  |
                  |
            __|
            """,
            """
              ___
              |   |
              O   |
             /|\\  |
                  |
                  |
            __|
            """,
            """
              ___
              |   |
              O   |
             /|\\  |
             /    |
                  |
            __|
            """,
            """
              ___
              |   |
              O   |
             /|\\  |
             / \\  |
                  |
            __|
            OHH NOO!! YOU DIED
            """
        ]

    def guess(self, guess_char):
        if guess_char in self.guesses:
            print(colors.fg.orange,"Character already guessed!!")
        else:
            self.guesses.append(guess_char)
        if guess_char not in self.word:
            self.wrongmoves += 1

    def display_word(self):# displays the letters being guessed which is right.
        display = ''
        for letter in self.word:
            if letter in self.guesses:
                display += letter
            else:
                display += '_'
        print(display)

    def word_guessed(self): #checks if the guesses list has all the letters in the word.
        return set(self.word) <= set(self.guesses)

    def display_game(self): #displays the hangman based on the number of wrong moves
        if self.wrongmoves > 0:
            if self.wrongmoves == 7 and not self.word_guessed():
                print(self.hang_man[6])
            else:
                print( colors.fg.red,self.hang_man[self.wrongmoves-1])


class EasyHangman(Hangman): #easy hangman which inherits all the methods from the base class , hangman.
    def __init__(self, lines):
        super().__init__(lines, 0, 19)

    def guess(self, guess_char):
        super().guess(guess_char)

    def display_word(self):
        super().display_word()

    def display_game(self):
        super().display_game()


class MediumHangman(Hangman): #medium hangman which inherits all the methods from the base class , hangman.
    def __init__(self, lines):
        super().__init__(lines, 20, 39)

    def guess(self, guess_char):
        super().guess(guess_char)

    def display_word(self):
        super().display_word()

    def display_game(self):
        super().display_game()


class HardHangman(Hangman):  #hard hangman which inherits all the methods from the base class , hangman.
    def __init__(self, lines):
        super().__init__(lines, 40, 59)

    def guess(self, guess_char):
        super().guess(guess_char)

    def display_word(self):
        super().display_word()

    def display_game(self):
        super().display_game()


def score(name, wrongmoves): #calculates the score of each player. Stores it in a file called highscore.txt. And updates highscore of each player.
    default_score = 70
    final_score = default_score - (wrongmoves * 10)
    score_file_path = "highscore.txt"
    print(colors.fg.pink,"Your Score:",final_score)
    with open(score_file_path, "r+") as score_file:
        content = score_file.readlines()
        contentsplit = [i.split(",") for i in content]
        found = False

        for i in range(len(contentsplit)):
            if contentsplit[i][0] == name:
                found = True
                if int(contentsplit[i][1]) < final_score:
                    contentsplit[i][1] = str(final_score)
                    print(colors.fg.green,"Your Highest score:", final_score)
                else:
                    print(colors.fg.green,"Your Highest score:", contentsplit[i][1])

        if not found:
            contentsplit.append([name, str(final_score)])
            print("Your Highest score:", final_score)

        
        score_file.seek(0)

       
        score_file.truncate()

        
        for entry in contentsplit:
            
            if len(entry) == 2:
                score_file.write(f"{entry[0]},{entry[1]}\n")


# Main
f = open("wordhint.txt", "r")
lines = f.readlines()
f.close()

print(colors.fg.purple,"Welcome to hangman!!")
print(colors.fg.orange,"Enter player name:")
name = input()
print(colors.fg.blue,"Choose the difficulty level:")
print(colors.fg.cyan,"For Easy, enter 1")
print(colors.fg.cyan,"For Medium, enter 2")
print(colors.fg.cyan,"For Hard, enter 3")
level = int(input("Enter your choice: "))

if level == 1:
    game = EasyHangman(lines)
elif level == 2:
    game = MediumHangman(lines)
elif level == 3:
    game = HardHangman(lines)

while game.wrongmoves <= 7:
    print(colors.fg.yellow,"Hint:", game.hint)
    game.display_word()
    print(colors.fg.red,"Wrong Moves:", game.wrongmoves)
    game.display_game()

    if game.wrongmoves == 7 and not game.word_guessed():
        print(colors.fg.red,"You Lost!!")
        print(colors.fg.green,"Word:", game.word)
        score(name, game.wrongmoves)
        break

    print(colors.fg.purple,"Enter your guess:")
    guess_char = input().lower()

    while len(guess_char) != 1:
        print(colors.fg.red,"Please enter a valid character!")
        guess_char = input().lower()

    game.guess(guess_char)

    if game.word_guessed():
        print(colors.fg.green,"Congratulations! You guessed the word:", game.word)
        score(name, game.wrongmoves)
        break
```
