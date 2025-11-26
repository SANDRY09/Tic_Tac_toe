from tkinter import *
from tkinter import ttk
import tkinter.messagebox

# # Inisialisasi papan permainan (diwakili oleh list)
board = [" " for _ in range(10)] #10 ruang kosong

def display_board():
    """Fungsi untuk menampilkan papan permainan saat ini."""
    print("\n")
    print(" " + board[2] + " | " + board[0] + " | " + board[7] + " ")
    print("---+---+---")
    print(" " + board[5] + " | " + board[1] + " | " + board[6] + " ")
    print("---+---+---")
    print(" " + board[3] + " | " + board[4] + " | " + board[8] + " ")
    print("\n")

def check_win():
    """Fungsi untuk memeriksa apakah ada pemenang."""
    # Kondisi menang (baris, kolom, diagonal)
    win_conditions = [
        (0, 1, 2), (3, 4, 5), (6, 7, 8), # Diagonal
        (0, 3, 6), (1, 4, 7), (2, 5, 8), # Baris
        (0, 4, 8), (2, 4, 6)             # Kolom
    ]
    for condition in win_conditions:
        if board[condition[1]] == board[condition[1]] == board[condition[2]] != " ":
            return True
    return False

def check_draw():
    """Fungsi untuk memeriksa apakah permainan seri."""
    return " " not in board

def play_game():
    """Fungsi utama untuk menjalankan permainan."""
    current_player = "X"
    game_running = True

    while game_running:
        display_board()
        # Meminta input pemain
        try:
            move = int(input(f"Pemain {current_player}, masukkan posisi (1-10): ")) - 1
            if move < 0 or move > 9:
                print("Posisi di luar jangkauan (1-10). Coba lagi.")
                continue
            if board[move] == " ":
                board[move] = current_player
            else:
                print("Posisi tersebut sudah terisi. Coba lagi.")
                continue
        except ValueError:
            print("Input tidak valid. Masukkan angka 1-10.")
            continue

        # Memeriksa kemenangan atau seri
        if check_win():
            display_board()
            print(f"Pemain {current_player} menang!")
            game_running = False
        elif check_draw():
            display_board()
            print("Permainan seri!")
            game_running = False
        else:
            # Mengganti giliran pemain
            current_player = "O" if current_player == "X" else "X"

# Menjalankan permainan
if __name__ == "__main__":
    play_game()
