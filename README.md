This function is responsible for printing the current state of the board. It takes the parameter board, which is a list representing the Tic Tac Toe board.
The function loops through the indices of the board list from 0 to 8. If the index is not zero and is divisible by 3, it prints a new line to start a new row.
For each index, it checks the corresponding value in the board list. If the value is 0, it prints an underscore "_". If the value is 1, it prints an uppercase letter "O". If the value is -1, it prints a lowercase letter "x".
Finally, it prints two new lines to leave some space after the board is printed.


def ConstBoard(board):
    print("Current State of the Board: \n\n")
    for i in range(0, 9):
        if (i > 0) and (i % 3 == 0):
            print("\n");
        if board[i] == 0:
            print("_", end=" ")
        if board[i] == 1:
            print("O", end=" ")
        if board[i] == -1:
            print("x", end=" ")
    print("\n\n")  
    
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------    
These two functions are used to get the user input for the position they want to place their X or O on the tic-tac-toe board. The input is taken from the user through the console and then converted to an integer type. The input value represents the index of the position on the board where the player wants to place their move.
In the `User1Turn` function, the input prompt asks the user to enter the position where they want to place their X, and if the position is already occupied by an O or X, the function prints a "Wrong Move" message and terminates the program using the `exit(0)` function. If the position is valid, the board is updated by setting the corresponding value in the board list to -1.
Similarly, in the `User2Turn` function, the user is prompted to enter the position where they want to place their O. The function also checks if the position is already occupied and terminates the program if it is. If the position is valid, the board is updated by setting the corresponding value in the board list to 1.
These functions are called alternately during the game depending on the player's turn, and the corresponding X or O is placed on the board.

    def User1Turn(board):
    pos = input("Enter X's position from [1, 2, ..., 9]: ")
    pos = int(pos)
    if board[pos - 1] != 0:
        print("Wrong Move")
        exit(0)
    board[pos - 1] = -1

     

def User2Turn(board):
    pos = input("Enter O's position from [1, 2, ..., 9]: ")
    pos = int(pos)
    if board[pos - 1] != 0:
        print("Wrong Move")
        exit(0)
    board[pos - 1] = 1
    
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
This is the `CompTurn` function which is called when the user chooses to play against the computer. It takes the current state of the board as an input argument and returns the updated state of the board after the computer has made its move. 
The function implements the minimax algorithm to determine the best move for the computer. It first initializes two variables `pos` and `value` to -1 and -2 respectively. `pos` stores the position on the board where the computer will make its move and `value` stores the score of the best move found so far.
The function then loops through each cell of the board using the `range()` function. If a cell is empty (i.e., its value is 0), the computer sets the value of that cell to 1 (i.e., the computer's token). It then calls the `minmax()` function with the updated board and the opposing player's token (-1) as input arguments. The returned value is the score of the current move.
Next, the function sets the value of the cell back to 0 to restore the board to its original state. If the score of the current move is greater than the value of the best move found so far, the function updates the value of `value` and sets `pos` to the current position. 
Finally, the function sets the value of the cell at `pos` to 1 and returns the updated state of the board with the computer's move.

def minmax(board, player):
    x = analyzeboard(board)
    if x != 0:
        return x * player
    pos = -1
    value = -2
    for i in range(0, 9):
        if board[i] == 0:
            board[i] = player
            score = -minmax(board, player*-1)
            board[i] = 0
            if score > value:
                value = score
                pos = i
    if pos == -1:
        return 0
    return value



def CompTurn(board):
    pos = -1
    value = -2
    for i in range(0, 9):
        if board[i] == 0:
            board[i] = 1
            score = -minmax(board, -1)
            board[i] = 0
            if score > value:
                value = score
                pos = i
    board[pos] = 1 
    
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------

Firstly, there is a function called analyzeboard that takes in a list board as an argument. This function checks if any player has won the game by analyzing the Tic Tac Toe board. The board is represented as a list of length 9, where each element represents a cell on the Tic Tac Toe board. The function analyzeboard checks all possible winning combinations using the variable cb, which is a list of lists. Each inner list in cb contains the indices of the cells that need to be checked to see if a player has won. If a player has won, the function returns the value of the winning player (1 for player 0 and -1 for player X). If no player has won, the function returns 0.
Secondly, there is a main function that asks the user whether they want to play against the computer or against another player. Depending on the user's choice, the function initializes the game accordingly. If the user chooses to play against the computer, the game starts with the computer playing as player 0 and the user playing as player X. The user is then asked if they want to play first or second. If the user chooses to play first, the game proceeds with the user making the first move, and if the user chooses to play second, the game proceeds with the computer making the first move. If the user chooses to play against another player, the game starts with player 1 playing as player X and player 2 playing as player 0. The user is then asked if player 1 or player 2 will make the first move.
Thirdly, the main function starts a loop that continues until either a player has won or the game ends in a draw. The loop alternates between the two players, allowing them to make their moves. If the player is the computer, the function CompTurn is called to make a move for the computer. If the player is a user, either User1Turn or User2Turn is called to make a move depending on whether the user is playing as player 1 or player 2.
Lastly, after the loop ends, the main function checks who has won the game using the analyzeboard function. If the game ended in a draw, the function prints "draw". If player X won, the function prints "player x is the winner". If player 0 won, the function prints "player 0 is the winner".


def analyzeboard(board):
    cb = [[0,1,2], [3,4,5], [6,7,8], [0,3,6], [1,4,7], [2,5,8], [0,4,8], [2,4,6]]
    for i in range(0,8):
        if(board[cb[i][0]]!=0 and board[cb[i][0]]==board[cb[i][1]] and board[cb[i][0]]==board[cb[i][2]]):
            return board[cb[i][0]]
    return 0
     

def main():
    choice = input("Press 1 to play against the computer or 2 to play against another player: ")
    choice = int(choice)
    board = [0,0,0,0,0,0,0,0,0]
    if(choice == 1):
        print("Computer: 0 vs You: X")
        player = input("Enter 1 to play first or 2 to play second: ")
        player = int(player)
        for i in range(0, 9):
            if(analyzeboard(board) != 0):
                break
            if((i+player) % 2 == 0):
                CompTurn(board)
            else:
                ConstBoard(board)
                User1Turn(board)
    elif(choice == 2):
        print("Player 1: X vs Player 2: 0")
        player = input("Enter 1 if Player 1 plays first or 2 if Player 2 plays first: ")
        player = int(player)
        for i in range(0, 9):
            if(analyzeboard(board) != 0):
                break
            if((i+player) % 2 == 0):
                ConstBoard(board)
                User2Turn(board)
            else:
                ConstBoard(board)
                User1Turn(board)
    else:
        print("Invalid choice.")
        return
    x = analyzeboard(board)
    if(x==0):
        ConstBoard(board)
        print('draw')
    if(x==-1):
        ConstBoard(board)
        print('player x is the winner')
    if(x==1):
        ConstBoard(board)
        print('player 0 is the winner')

main()

