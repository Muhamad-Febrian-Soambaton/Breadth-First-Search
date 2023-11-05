# Best First Search Tic Tac Toe
Muhamad Febrian Soambaton 5311421050

## Method
1. Membuka Visual Studio Code.
2. Membuat file python seperti gambar di bawah.

![image](https://github.com/Muhamad-Febrian-Soambaton/Search-Algorithm/assets/148663785/4848f4bc-4f9c-4ea9-8eaf-b6aa068190d8)
   
4. Memasukkan code di bawah kedalam file TicTac.py.

         import tkinter as tk
         from tkinter import messagebox
         from queue import PriorityQueue
         
         # Function to check for a win
         def check_win(player):
             for i in range(3):
                 if board[i][0] == board[i][1] == board[i][2] == player:  # Check rows
                     return True
                 if board[0][i] == board[1][i] == board[2][i] == player:  # Check columns
                     return True
             if board[0][0] == board[1][1] == board[2][2] == player:  # Check diagonals
                 return True
             if board[0][2] == board[1][1] == board[2][0] == player:  # Check diagonals
                 return True
             return False
         
         # Function to check for a draw
         def is_draw():
             return all(board[i][j] != " " for i in range(3) for j in range(3))
         
         # Function to evaluate the board state
         def evaluate_board():
             if check_win("X"):
                 return -1
             elif check_win("O"):
                 return 1
             else:
                 return 0
         
         # Minimax algorithm with alpha-beta pruning
         def minimax(board, depth, maximizing_player):
             if depth == 0 or check_win("X") or check_win("O") or is_draw():
                 return evaluate_board()
         
             if maximizing_player:
                 max_eval = -float("inf")
                 for i in range(3):
                     for j in range(3):
                         if board[i][j] == " ":
                             board[i][j] = "O"
                             eval = minimax(board, depth - 1, False)
                             board[i][j] = " "
                             max_eval = max(max_eval, eval)
                 return max_eval
             else:
                 min_eval = float("inf")
                 for i in range(3):
                     for j in range(3):
                         if board[i][j] == " ":
                             board[i][j] = "X"
                             eval = minimax(board, depth - 1, True)
                             board[i][j] = " "
                             min_eval = min(min_eval, eval)
                 return min_eval
         
         # Best-First Search bot
         def bfs_bot():
             best_score = -float("inf")
             best_move = None
             for i in range(3):
                 for j in range(3):
                     if board[i][j] == " ":
                         board[i][j] = "O"
                         move_score = minimax(board, 2, False)  # Depth limited for faster response
                         board[i][j] = " "
                         if move_score > best_score:
                             best_score = move_score
                             best_move = (i, j)
             return best_move
         
         # Function to handle player's move
         def on_click(row, col):
             if board[row][col] == " ":
                 board[row][col] = "X"
                 buttons[row][col].config(text="X")
                 if check_win("X"):
                     messagebox.showinfo("Tic Tac Toe", "Player X wins!")
                     reset_game()
                 elif is_draw():
                     messagebox.showinfo("Tic Tac Toe", "It's a draw!")
                     reset_game()
                 else:
                     bot_move = bfs_bot()
                     if bot_move:
                         row, col = bot_move
                         board[row][col] = "O"
                         buttons[row][col].config(text="O")
                         if check_win("O"):
                             messagebox.showinfo("Tic Tac Toe", "Player O wins!")
                             reset_game()
                         elif is_draw():
                             messagebox.showinfo("Tic Tac Toe", "It's a draw!")
                             reset_game()
         
         # Function to reset the game
         def reset_game():
             global board
             board = [[" " for _ in range(3)] for _ in range(3)]
             for i in range(3):
                 for j in range(3):
                     buttons[i][j].config(text=" ", state=tk.NORMAL)
         
         # Create the main window
         root = tk.Tk()
         root.title("Tic Tac Toe")
         
         # Initialize variables
         board = [[" " for _ in range(3)] for _ in range(3)]
         
         # Create buttons for the game board
         buttons = [[0, 0, 0], [0, 0, 0], [0, 0, 0]]
         
         for i in range(3):
             for j in range(3):
                 buttons[i][j] = tk.Button(root, text=" ", font=("normal", 20), width=7, height=3,
                                          command=lambda i=i, j=j: on_click(i, j))
                 buttons[i][j].grid(row=i, column=j)
         
         # Create a reset button
         reset_button = tk.Button(root, text="Reset", font=("normal", 20), command=reset_game)
         reset_button.grid(row=3, column=1)
         
         # Start the game
         root.mainloop()

6. Run code dan uji coba hasil.
7. Hasil dan penjelasan code.

## Hasil
![image](https://github.com/Muhamad-Febrian-Soambaton/Search-Algorithm/assets/148663785/a52b0539-d78b-49e7-868b-c82c47b78f8d)
