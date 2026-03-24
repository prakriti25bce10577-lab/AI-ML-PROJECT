import math

# Initialize the board
board = [' ' for _ in range(9)]

def print_board():
    for row in [board[i*3:(i+1)*3] for i in range(3)]:
        print('| ' + ' | '.join(row) + ' |')

def check_winner(state, player):
    win_cond = [[0, 1, 2], [3, 4, 5], [6, 7, 8], [0, 3, 6], [1, 4, 7], [2, 5, 8], [0, 4, 8], [2, 4, 6]]
    return any(all(state[i] == player for i in cond) for cond in win_cond)

def minimax(state, depth, is_maximizing):
    if check_winner(state, 'O'): return 1
    if check_winner(state, 'X'): return -1
    if ' ' not in state: return 0

    if is_maximizing:
        best_score = -math.inf
        for i in range(9):
            if state[i] == ' ':
                state[i] = 'O'
                score = minimax(state, depth + 1, False)
                state[i] = ' '
                best_score = max(score, best_score)
        return best_score
    else:
        best_score = math.inf
        for i in range(9):
            if state[i] == ' ':
                state[i] = 'X'
                score = minimax(state, depth + 1, True)
                state[i] = ' '
                best_score = min(score, best_score)
        return best_score

def ai_move():
    best_score = -math.inf
    move = 0
    for i in range(9):
        if board[i] == ' ':
            board[i] = 'O'
            score = minimax(board, 0, False)
            board[i] = ' '
            if score > best_score:
                best_score = score
                move = i
    board[move] = 'O'

def play_game():
    print("Welcome to AI Tic-Tac-Toe! You are 'X'.")
    while ' ' in board:
        print_board()
        try:
            user_move = int(input("Enter move (0-8): "))
            if board[user_move] != ' ': continue
            board[user_move] = 'X'
        except: continue
        
        if check_winner(board, 'X'):
            print_board(); print("You won!"); return
        if ' ' not in board: break
        
        ai_move()
        if check_winner(board, 'O'):
            print_board(); print("AI wins!"); return
            
    print_board(); print("It's a draw!")

if __name__ == "__main__":
    play_game()



PROJECT REPORT

1. Introduction
​This project is an implementation of the classic Tic-Tac-Toe game where a human player competes against an unbeatable Artificial Intelligence. The core of the project focuses on decision-making algorithms and state-space search in a competitive environment.
​2. Problem Statement
​Traditional games often rely on hard-coded rules, which can be predictable. The challenge was to create an intelligent agent capable of evaluating all possible future moves to ensure it either wins or forces a draw. This is a foundational problem in Artificial Intelligence known as an "adversarial search."
​3. Approach and Tools Used
​Programming Language: Python
​Algorithm: Minimax Algorithm. This is a recursive strategy used in decision-making and game theory to find the optimal move for a player, assuming that the opponent is also playing optimally.
​Data Structures: A simple list of length 9 was used to represent the 3x3 game board, ensuring efficient state management and backtracking.
​Key Libraries: The math library was utilized to handle infinite values during score comparisons.
​4. Key Decisions
​Recursion: I chose recursion to navigate the game tree. The algorithm explores every possible move, assigns a score (1 for win, -1 for loss, 0 for draw), and bubbles that score back up to make the best choice.
​Human vs. AI: To make the game interactive, I implemented a Command Line Interface (CLI) that takes user input (0-8) and updates the board in real-time.
​Unbeatable Logic: By setting the AI as the "Maximizing" player, it consistently seeks the highest possible score, making it impossible for a human to win if the AI plays first or responds correctly.
​5. Challenges Faced
​Recursion Depth: Understanding how the state resets (backtracking) after a recursive call was challenging. I solved this by temporarily changing the board state (board[i] = 'O') and then immediately reverting it (board[i] = ' ') after the function call.
​Input Validation: Handling cases where a user might enter a non-integer or a position that is already taken. I used try-except blocks and conditional checks to keep the game running smoothly.
​6. Learning Outcomes
​Algorithmic Thinking: Gained a deep understanding of how AI evaluates "future" states to make "present" decisions.
​Python Proficiency: Improved my skills in list comprehension, function definitions, and logical branching.
​Game Development Lifecycle: Learned how to structure a project from initializing a board to implementing a win-check logic and a game loop.



