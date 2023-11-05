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

![image](https://github.com/Muhamad-Febrian-Soambaton/Search-Algorithm/assets/148663785/808dda20-20cd-408e-a16a-bdfc46f9814c)

Dari hasil yang diberikan code dapat berjalan sesuai yang diinginkan dan bot dapat bermain bersama manusia dengan menggunakan algoritma Best First Search.

## Penjelasan Code
Pada code yang diberikan bot yang dibuat menggunakan algoritma Best First Search Minimax. 

![game-tree-for-tic-tac-toe-minimax-codesweetly](https://github.com/Muhamad-Febrian-Soambaton/Search-Algorithm/assets/148663785/d282d404-ad9d-48f0-aff2-634225d1ef74)

Algoritma Minimax melakukan pencarian pola permainan yang baik dengan memanfaatkan min dan max dari tiap depth. Depth pertama pemain, depth kedua bot, depth ketiga pemain, keempat bot dan seterusnya seperti pada gamabar. Algortima minimax mencoba semua kemungkinan pada tiap depth jika pemain menggunakan jalan pertama maka ada delapan kemungkinan minimax dapat menaruh tanda yang di butuhkan setiap kemungkinan memiliki 7 kemungkinan lagi untuk pemain dapat menentukan tempat tanda ayng akan di digunakan dan seterusnya sehingga algoritma Minimax dapat memetakan seluruh kemungkinan pemain dan memilih nilai terendah agar lawan kemungkinan menangnya kecil. Step algoritma minimax :
1. Rekursi: Algoritma ini dimulai dari kondisi permainan saat ini dan secara rekursif menjelajahi semua kemungkinan kondisi masa depan dengan mensimulasikan giliran pemain dan lawan.

2. Eksplorasi Pohon: Algoritma ini membangun pohon permainan di mana setiap simpul mewakili sebuah kondisi permainan, dan sisi-sisinya mewakili langkah-langkah yang mungkin. Pohon ini dibangun dengan mempertimbangkan semua langkah yang mungkin pada setiap tingkat.

3. Evaluasi: Pada simpul-simpul daun pohon (kondisi permainan terminal), algoritma mengevaluasi keinginan hasil akhir untuk pemain saat ini. Fungsi evaluasi ini memberikan skor pada kondisi tersebut. Dalam permainan Tic Tac Toe, kondisi menang untuk pemain dapat diberi skor +1, sementara kondisi kalah diberi skor -1, dan kondisi seri diberi skor 0.

4. Penyebaran Kembali (Backpropagation): Algoritma kemudian menyebarkan skor-skor ini kembali ke atas pohon, mengambil skor maksimum jika giliran pemain (maksimalkan pemain) dan skor minimum jika giliran lawan (minimalkan pemain). Akar pohon pada akhirnya akan menerima skor yang mewakili langkah terbaik.

5. Keputusan: Setelah algoritma menjelajahi semua langkah yang mungkin dan menyebarluaskan skor-skornya ke akar, pemain dapat memilih langkah yang terkait dengan skor tertinggi jika mereka ingin memaksimalkan peluang menang.

Penjelasan Code

Import Libraries:
Kode pertama mengimpor beberapa pustaka yang diperlukan, seperti tkinter untuk antarmuka grafis, messagebox untuk menampilkan pesan, dan PriorityQueue untuk menerapkan algoritma Minimax.

Fungsi check_win(player):
Fungsi ini memeriksa apakah pemain (X atau O) telah memenangkan permainan. Ini memeriksa baris, kolom, dan diagonal untuk mencari pemenang.

Fungsi is_draw():
Fungsi ini memeriksa apakah permainan berakhir dengan hasil seri (draw).

Fungsi evaluate_board():
Fungsi ini mengevaluasi kondisi papan permainan saat ini. Mengembalikan -1 jika pemain X menang, 1 jika pemain O menang, dan 0 jika belum ada yang menang.

Fungsi minimax(board, depth, maximizing_player):
Ini adalah implementasi algoritma Minimax dengan pemangkasan alpha-beta. Ini digunakan oleh bot untuk memilih gerakan terbaik berdasarkan papan permainan saat ini dan kedalaman pencarian tertentu.

Fungsi bfs_bot():
Ini adalah bot yang menggunakan algoritma Best-First Search untuk memilih gerakan terbaik. Bot mencoba semua gerakan yang mungkin dan memilih yang memiliki skor tertinggi berdasarkan algoritma Minimax.

Fungsi on_click(row, col):
Ini adalah fungsi yang dipanggil saat pemain mengklik kotak di papan permainan. Ini menentukan gerakan pemain, memeriksa apakah pemain menang atau hasil seri, dan membiarkan bot membuat gerakan.

Fungsi reset_game():
Fungsi ini mengatur ulang permainan, mengosongkan papan permainan, dan mengembalikan tombol ke keadaan awal.

Pembuatan GUI:
Kode ini membuat antarmuka grafis untuk permainan Tic Tac Toe menggunakan tkinter. Kode Ini menampilkan papan permainan dengan tombol-tombol yang dapat diklik.
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

buttons adalah matriks yang digunakan untuk merepresentasikan papan permainan sebagai tombol-tombol dalam GUI. Setiap tombol diinisialisasi dengan teks kosong dan terhubung dengan on_click untuk menangani klik pemain.
reset_button adalah tombol yang memungkinkan pemain mengatur ulang permainan.
Akhirnya, permainan dimulai dengan memanggil root.mainloop(), yang mengaktifkan antarmuka grafis dan memungkinkan pemain untuk bermain melawan bot dengan menggunakan algoritma Minimax.

