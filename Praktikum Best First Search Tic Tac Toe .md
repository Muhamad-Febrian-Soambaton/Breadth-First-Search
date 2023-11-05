# Best First Search Tic Tac Toe
Muhamad Febrian Soambaton 5311421050

## Method
1. Membuka Visual Studio Code.
2. Membuat file gui dan file utama seperti gambar dibawah.
   ![image](https://github.com/Muhamad-Febrian-Soambaton/Search-Algorithm/assets/148663785/d97e88f9-af84-4f38-9a22-e0905c374f23)
3. Memasukkan code di bawah kedalam file TicTacToe.java.
             public class TicTacToe {
          public static void main(String[] args) {
          TicTacToeGUI game = new TicTacToeGUI();
          game.startGame();
          }
          }
4. Memasukkan code di bawah ini kedalam file TicTacToeGUI.java.
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
5. Run code dan uji coba hasil.
6. Hasil dan penjelasan code.

## Hasil
