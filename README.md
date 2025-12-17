# task-2
building a simple Tic-Tac-Toe AI in Python! A great way to understand game logic, decision-making, and how an AI chooses the best move. Loving the learning process so far!
# Simple Tic-Tac-Toe AI with Minimax + Alpha-Beta Pruning

import math

def show(board):
    print()
    for i in range(3):
        print(board[i*3], "|", board[i*3+1], "|", board[i*3+2])
    print()

def winner(board, player):
    wins = [
        (0,1,2),(3,4,5),(6,7,8),
        (0,3,6),(1,4,7),(2,5,8),
        (0,4,8),(2,4,6)
    ]
    return any(board[a] == board[b] == board[c] == player for a,b,c in wins)

def minimax(board, depth, is_max, ai, human, alpha, beta):
    if winner(board, ai):
        return 1
    if winner(board, human):
        return -1
    if " " not in board:
        return 0

    if is_max:
        best = -math.inf
        for i in range(9):
            if board[i] == " ":
                board[i] = ai
                val = minimax(board, depth+1, False, ai, human, alpha, beta)
                board[i] = " "
                best = max(best, val)
                alpha = max(alpha, val)
                if beta <= alpha:
                    break  # prune
        return best

    else:
        best = math.inf
        for i in range(9):
            if board[i] == " ":
                board[i] = human
                val = minimax(board, depth+1, True, ai, human, alpha, beta)
                board[i] = " "
                best = min(best, val)
                beta = min(beta, val)
                if beta <= alpha:
                    break  # prune
        return best

def best_move(board, ai, human):
    best_val = -math.inf
    move = -1
    for i in range(9):
        if board[i] == " ":
            board[i] = ai
            val = minimax(board, 0, False, ai, human, -math.inf, math.inf)
            board[i] = " "
            if val > best_val:
                best_val = val
                move = i
    return move

# Game Loop
board = [" "] * 9
human = input("Choose X or O: ").upper()
ai = "O" if human == "X" else "X"
turn = "X"
show(board)

while True:
    if winner(board, human):
        print("You win!")
        break
    if winner(board, ai):
        print("AI wins!")
        break
    if " " not in board:
        print("Draw!")
        break

    if turn == human:
        pos = int(input("Enter position (1-9): ")) - 1
        if board[pos] == " ":
            board[pos] = human
            turn = ai
        else:
            print("Taken. Try again.")
    else:
        move = best_move(board, ai, human)
        board[move] = ai
        print(f"AI placed {ai} at position {move+1}")
        turn = human

    show(board)

