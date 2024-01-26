# 19CSE201 - Advanced Programming
![](https://img.shields.io/badge/Batch-22CYS-lightgreen) ![](https://img.shields.io/badge/UG-blue) ![](https://img.shields.io/badge/Subject-AdP-blue)
![](https://img.shields.io/badge/-HPOJ-brown)

## Puzzle based Escape Game
```
import random
class player:
    def __init__(self, name, score):
        #Constructor for player class
        self.name = name
        self.score = score
def errormsg():
    #Error message function
    print("\nOh no! Since you didn't enter a valid digit, our chance to go up the stairs was lost..\n(You have lost this game, but this is not the end.Try again, but don't forget to enter correct options.)\nThankyou for playing!")
def sep():
    #Seperator function
    print("\n\n*****************************************************\n\n")
def conv(file):
    #Function to read and display content from a file
    try:
        with open(file, 'r') as file:
            # Do something with the file content
            content = file.read()
            print(content)
    except FileNotFoundError:
        print(f"The file {file_path} does not exist.")
    except Exception as e:
        print(f"An error occurred: {e}")
def getanswer():
    # Function to get user's answer
    i=input("Enter your answer:")
    return i
def checkanswer(answer,canswer):
    # Function to check if the answer is correct
    if (answer == canswer):
        print("Correct.\n")
        return 1  # Return 1 if the answer is correct
    else:
        print("Incorrect. The correct answer is %c.\n", canswer)
        return 0 # Return 0 f the answer is incorrect
if __name__ == "__main__":
    opt=0
    
    #Print castle entrance
    for i in range(15):
        for s in range(15-i):
            print(" ", end="")
        for j in range(i+1):
            print("* ", end="")
            if(i==14 and j==14):
                print("                                           Welcome to Castle Xcape")    
        print("\n")
    for x in range(15):
        print("     xxxxxxxxxxxxxxxxxxxxx\n")   
    c=input("Hello there..I'm Ada Lovegood, the rightful heir to the throne..\nMy evil uncle has claimed the throne as his, after the passing of my father, King Richard.\nInorder to reclaim my right, I must find my father's legacy will- which is hidden \nsomewhere in the third storey of the castle.\nCan you help me on my journey of reclaiming my birthright?(y/n)\n")
    if (c=='n'or c=='N'):
        print("Too bad! Hope you can help another time..\n")
    elif (c=='Y'or c=='y'):
        print("Thankyou kind person..before we start, can I know your name?\n")
    elif (c!='n' or c!='N'or c!='Y'or c!='y'):
        print("Oh no! It seems like you've selected a wrong option, please enter a valid choice from next time..\n(reminder: please enter either the letter y or n)\n")
    name=input("Enter your name:")
    score=0
    p1=player(name,score)
    print(f"Nice to meet you {p1.name} Come on, lets start our expedition..\n\n")
    sep()
    filename = "c1.txt"
    conv(filename)

    opt = int(input("Enter option number(1/2):"))
    print("\n")

    if opt == 1:
        filename = "c1_1.txt"
        conv(filename)
    elif opt == 2:
        filename = "c1_2.txt"
        conv(filename)
    else:
        errormsg()
    conv("c@.txt")
    
    print("Welcome to Maths challenge\n")
    print("Guard:You will have three questions to answer.\nYou will be given three chances for each question.\nIf you go wrong in all the three chances then sorry you won't get the will\n")
    #question1
    print("Question 1: The town is in danger.Warrior Franklin is the knight\n in shining armour.Everyone is sick and only a special\nherb can cure the sickness.Franklin needs to travel\na certain distance to get to the herb.He has to \ntravel for months to reach the destination.In the month \nof january,he travelled 50 km.By the month of february,\n he covered 100km.By march,he covered 150km.How much total \ndistance he covered till month of may? \n") 
    print("option a:400\n")
    print("option b:350\n")
    print("option c:250\n")
    print("option d:200\n")
    while True:
        answer = getanswer()
        if answer == 'a' or answer == 'b' or answer == 'c' or answer == 'd':
            break
        else:
            print("Invalid option. Please choose a valid option!!")
    p1.score+=checkanswer(answer,'c')
    #question 2
    print("Question 2:If 2+2=6,3+3=11,4+4=18,then find 6+6=?\n")
    print("option a:38\n")
    print("option b:36\n")
    print("option c:34\n")
    print("option d:15\n")
    while True:
        answer = getanswer()
        if answer == 'a' or answer == 'b' or answer == 'c' or answer == 'd':
            break
        else:
            print("Invalid option. Please choose a valid option!!")
    p1.score+=checkanswer(answer,'a')
    #question 3
    print("Question 3: Evaluate the expression 8+10/5-3\n")
    print("option a:4\n")
    print("option b:5\n")
    print("option c:6\n")
    print("option d:7\n")
    while True:
        answer = getanswer()
        if answer == 'a' or answer == 'b' or answer == 'c' or answer == 'd':
            break
        else:
            print("Invalid option. Please choose a valid option!!")
    p1.score+=checkanswer(answer,'d')
    if (p1.score==0):
    
        print(" You scored 0 out of 3 questions.\n")
        print("Oh! No You have lost this game.")
    else:
        print(f" You scored {p1.score} out of 3 questions.\n")
    sep()
    conv("c2.txt")
    opt=int(input("Enter option:"))
    if opt == 1:
        filename = "c2_1.txt"
        conv(filename)
    elif opt == 2:
        filename = "c2_2.txt"
        conv(filename)
    else:
        errormsg()

    #Rock Paper Scissors
    options=['r','p','s']
    print("Andrew: Hello there new visitor.Your task for today will be \nto enter into a tremendous game of rock, paper, and scissors with me.\n")
    print("Let's start the game.\n")
    print("The rules of the game my friend are quite simple.\n")
    print("Enter 'r' for ROCK, 'p' for PAPER, and 's' for SCISSORS: ")
    print("\nTick-tock, the time is running out.\n")
    for round in range(3):
        print(f"Your round {round} begins")
        player=input("Enter your choice of r/p/s:  ")
        if (player != 'r' and player != 'p' and player != 's'):
            print("Oh man, the entered option is invalid. Please enter a correct option. Come on, let's try again.\n")
            rounds-=1
        else:
            boy= random.randint(1,3)
            if (boy==1 and player=='r') or (boy==2 and player=='p') or (boy==3 and player=='s'):
                print("It's a draw!\n")
            elif ((player == 'r' and boy == 2) or (player == 'p' and boy ==3) or (player == 's' and boy == 1)):
                print("Oh no! You have lost the game!You wont get the will\n")
            else:
                print("Congratulations! You have won the game!\n")
        if boy==1:
            boy='r'
        elif boy==2:
            boy='p'
        else:
            boy='s'
        print(f"Ada:{player}, Cousin brother: {boy}\n")
    print("Oh your chances for this game have ended my friend!!Hope I never see you again!!\n\n")
    sep()
    filename="c3.txt"
    conv(filename)
    opt=int(input("Enter option (1/2):"))
    if opt==1:
        conv("c3_1.txt")	
    elif opt==2:
        conv("c3_2.txt")
    else:
        errormsg()
    print("Grandmother: I heard from Andrew that you guys are searching for the legacy will..\nI'll help you find it if you can solve my riddles..\n")
    #Riddle
    print("Grandmother: You will have three questions to answer.\nYou will be given three chances for question 1.\nIf you go wrong in all three chances, then sorry you won't get the will.\nFor the other two questions, you will get one chance.\n")    
    print("Question 1: A guy got an anonymous letter with a clue below it which when decoded will reveal her name. The clue is 'Third of august;Fourth of February;Second of May;Third of December;Sixth of October'. Find her name.\n")
    count=0
    for i in range(3):
        print("this is your chance",i+1)
        ans=input("Enter your answer:")
        if (ans.lower()=="grace"):
            print("The answer is correct!!\n")
            break
        else:
            count = count + 1
            continue
    if count==3:
        print("Sorry! You answered wrong in all attempts.\nYou won't get the will.\n")
    else:
        print("Question 2: One of the three colored buttons (Yellow;Blue;Purple) will set you free, the other two will lock you forever. The clue given is [nte rbu lup otp]. Which button should he press to escape?\n")
        print("Option 1: Blue\nOption 2: Purple\nOption 3: Yellow\n")
        opt=int(input("Choose (1/2/3): "))
        if (opt == 1):
            print("Wrong answer.\n")
        elif (opt == 2):
	        print("The answer is correct!!\n")
        else:
            print("Wrong answer.\n")
    print("Question 3: Maya lives with her parents in India. Maya went missing on a Saturday when his parents went out for dinner. The police were called and the helpers were questioned.")
print("Helper 1 said 'I was packing Maya's bag for school the next day'.")
print("Helper 2 said 'I spent the whole evening cleaning the kitchen'.")
print("Helper 3 said 'I was cooking and listening to music'.")
print("Who kidnapped Maya?")
print("Option 1: Helper 2")
print("Option 2: Helper 3")
print("Option 3: Helper 1")

opt = int(input("Choose (1/2/3): "))
if opt == 1:
    print("Your answer is wrong.")
elif opt == 2:
    print("Your answer is wrong.")
elif opt == 3:
    print("Your answer is right..\n\n\n* Grandma:Well then, good luck my dears...Oh, and   *\n* don't forget to open the left room..              *")

sep()
print("With help from grandmother, you and Ada reached\n the 3rd storey and open the left door, and succeed in finding the legacy will..\nWith your help, Ada became the ruler she was born to be..\n\n\nThankyou for playing!\nCredits:\nAmita\nMeera\nParvathi\nSri Lakshmi ")
```
