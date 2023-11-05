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

   
5. Memasukkan code di bawah ini kedalam file TicTacToeGUI.java.

            import java.awt.*;
            import java.awt.event.*;
            import javax.swing.*;
      
       public class TicTacToeGUI {
       private JFrame frame;
       private JButton[][] buttons;
       private char currentPlayer;
       private JLabel statusLabel;
      
       public TicTacToeGUI() {
       frame = new JFrame("Tic-Tac-Toe");
       buttons = new JButton[3][3];
       currentPlayer = 'X';
      
       statusLabel = new JLabel("Player " + currentPlayer + "'s turn");
       statusLabel.setFont(new Font("Arial", Font.PLAIN, 18));
      
       frame.setLayout(new GridLayout(4, 3));
      
       for (int i = 0; i < 3; i++) {
           for (int j = 0; j < 3; j++) {
               buttons[i][j] = new JButton("");
               buttons[i][j].setFont(new Font("Arial", Font.PLAIN, 48));
               buttons[i][j].setBackground(Color.WHITE);
               buttons[i][j].setFocusPainted(false);
               buttons[i][j].addActionListener(new ButtonListener(i, j));
               frame.add(buttons[i][j]);
           }
       }
      
       JButton resetButton = new JButton("Reset");
       resetButton.setFont(new Font("Arial", Font.PLAIN, 18));
       resetButton.addActionListener(new ResetListener());
       frame.add(statusLabel);
       frame.add(resetButton);
       }
      
       public void startGame() {
       frame.setSize(300, 400);
       frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
       frame.setVisible(true);
       }
      
       private class ButtonListener implements ActionListener {
       private int row;
       private int col;
      
       public ButtonListener(int row, int col) {
           this.row = row;
           this.col = col;
       }
      
       public void actionPerformed(ActionEvent e) {
           if (buttons[row][col].getText().equals("") && currentPlayer != ' ') {
               buttons[row][col].setText(currentPlayer + "");
               buttons[row][col].setForeground(currentPlayer == 'X' ? Color.BLUE : Color.RED);
               buttons[row][col].setFont(new Font("Arial", Font.BOLD, 48));
               currentPlayer = (currentPlayer == 'X') ? 'O' : 'X';
               statusLabel.setText("Player " + currentPlayer + "'s turn");
      
               if (checkForWin()) {
                   statusLabel.setText("Player " + currentPlayer + " wins!");
                   currentPlayer = ' ';
               }
           }
       }
       }
      
       private class ResetListener implements ActionListener {
       public void actionPerformed(ActionEvent e) {
           for (int i = 0; i < 3; i++) {
               for (int j = 0; j < 3; j++) {
                   buttons[i][j].setText("");
                   buttons[i][j].setBackground(Color.WHITE);
               }
           }
           currentPlayer = 'X';
           statusLabel.setText("Player " + currentPlayer + "'s turn");
       }
       }
      
       private boolean checkForWin() {
       // Check rows, columns, and diagonals for a win
       for (int i = 0; i < 3; i++) {
           if (buttons[i][0].getText().equals(buttons[i][1].getText()) &&
               buttons[i][1].getText().equals(buttons[i][2].getText()) &&
               !buttons[i][0].getText().equals("")) {
               highlightWinningLine(i, 0, i, 2);
               return true;
           }
      
           if (buttons[0][i].getText().equals(buttons[1][i].getText()) &&
               buttons[1][i].getText().equals(buttons[2][i].getText()) &&
               !buttons[0][i].getText().equals("")) {
               highlightWinningLine(0, i, 2, i);
               return true;
           }
       }
      
       if (buttons[0][0].getText().equals(buttons[1][1].getText()) &&
           buttons[1][1].getText().equals(buttons[2][2].getText()) &&
           !buttons[0][0].getText().equals("")) {
           highlightWinningLine(0, 0, 2, 2);
           return true;
       }
      
       if (buttons[0][2].getText().equals(buttons[1][1].getText()) &&
           buttons[1][1].getText().equals(buttons[2][0].getText()) &&
           !buttons[0][2].getText().equals("")) {
           highlightWinningLine(0, 2, 2, 0);
           return true;
       }
      
       return false;
       }
      
       private void highlightWinningLine(int row1, int col1, int row2, int col2) {
       buttons[row1][col1].setBackground(Color.GREEN);
       buttons[row2][col2].setBackground(Color.GREEN);
       }
       }
6. Run code dan uji coba hasil.
7. Hasil dan penjelasan code.

## Hasil
