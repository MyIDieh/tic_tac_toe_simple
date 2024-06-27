import os
import random
from colorama import Fore, Back, Style

playAgain = "y"
plays = 0
whoPlay = 2 # 1:CPU ; 2:Player
maxPlay = 9
vic = "n"

theMatch = [
    [" ", " ", " "], # L0C0 L0C1 L0C2
    [" ", " ", " "], # L1C0 L1C1 L1C2
    [" ", " ", " "], # L2C0 L2C1 L2C2
]


def theScreen():
    global theMatch
    global plays
    os.system("cls")
    print("    0   1   2")
    print(f"0:  {theMatch[0][0]} | {theMatch[0][1]} | {theMatch[0][2]}")
    print("   ___________")
    print(f"1:  {theMatch[1][0]} | {theMatch[1][1]} | {theMatch[1][2]}")
    print("   ___________")
    print(f"2:  {theMatch[2][0]} | {theMatch[2][1]} | {theMatch[2][2]}")
    print(f"Num plays: {Fore.GREEN + str(plays) + Fore.RESET}")


def playerPlays():
    global plays
    global whoPlay
    global maxPlay

    if whoPlay == 2 and plays < maxPlay:
        try:
            l = int(input("Row   : "))
            c = int(input("Column: "))
            while theMatch[l][c] != " ":
                l = int(input("Row   : "))
                c = int(input("Column: "))
            theMatch[l][c] = "X"
            whoPlay = 1
            plays += 1
        except:
            print("Row or Column is an invalid value. Try again.")
            # vic = "n"


def cpuPlays():
    global plays
    global whoPlay
    global maxPlay
    if whoPlay == 1 and plays < maxPlay:
        l = random.randrange(0, 3)
        c = random.randrange(0, 3)
        while theMatch[l][c] != " ":
            l = random.randrange(0, 3)
            c = random.randrange(0, 3)
        theMatch[l][c] = "O"
        plays += 1
        whoPlay = 2


def verifyVictory():
    global theMatch
    global vic
    victory = "n"
    symbols = ["X", "O"]
    for s in symbols:
        victory = "n"
        # Verify victories in ROWs
        il = ic = 0
        while il < 3:
            soma = 0
            ic = 0
            while ic < 3:
                if (theMatch[il][ic] == s):
                    soma += 1
                ic += 1
            if (soma == 3):
                victory = s
                vic = s  # Update vic when victory condition is met
                break
            il += 1
        if (victory != "n"):
            break
        # Verify victories in COLUMNSs
        il = ic = 0
        while ic < 3:
            soma = 0
            il = 0
            while il < 3:
                if (theMatch[il][ic] == s):
                    soma += 1
                il += 1
            if (soma == 3):
                victory = s
                vic = s  # Update vic when victory condition is met
                break
            ic += 1
        if (victory != "n"):
            break
        # Verify victories in diagonals [1]
        soma = 0
        idiagonal = 0
        while idiagonal < 3:
            if (theMatch[idiagonal][idiagonal] == s):
                soma += 1
            idiagonal += 1
        if (soma == 3):
            victory = s
            vic = s  # Update vic when victory condition is met
            break
        # Verify victories in diagonals [2]
        soma = 0
        idiagonall = 0
        idiagonalc = 2
        while idiagonalc >= 0:
            if (theMatch[idiagonall][idiagonalc] == s):
                soma += 1
            idiagonall += 1
            idiagonalc -= 1
        if (soma == 3):
            victory = s
            vic = s  # Update vic when victory condition is met
            break
    return victory


def theReset():
    global theMatch
    global plays
    global whoPlay
    global maxPlay
    global vic
    plays = 0
    whoPlay = 2  # 1:CPU ; 2:Player
    maxPlay = 9
    vic = "n"

    theMatch = [
        [" ", " ", " "],  # L0C0 L0C1 L0C2
        [" ", " ", " "],  # L1C0 L1C1 L1C2
        [" ", " ", " "],  # L2C0 L2C1 L2C2
    ]


while (playAgain == "Y" or playAgain == "y"):

    while True:
        theScreen()
        playerPlays()
        cpuPlays()
        theScreen()
        vit = verifyVictory()
        if (vit != "n") or (plays >= maxPlay):
            break

    print(Fore.RED + "The Match is Completed" + Fore.YELLOW)
    if (vit == "X" or vit == "O"):
        print("Result : Player " + vic + " Won")
    else:
        print("Draw")

    playAgain = input(Fore.BLUE + "Play again? [Y/N]: " + Fore.RESET)
    theReset()
