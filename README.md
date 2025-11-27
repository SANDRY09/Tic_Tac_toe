import tkinter as tk
from tkinter import messagebox

# --- Konfigurasi Awal Game ---

# List untuk melacak status papan permainan (9 kotak)
board = [" " for _ in range(9)]
# Menentukan pemain pertama
current_player = "X"
# Status permainan sedang berjalan
game_running = True
# List untuk menyimpan referensi objek tombol Tkinter
buttons = []

def handle_click(index):
    """Menangani klik tombol di papan permainan."""
    global current_player, game_running
    
    # Memeriksa apakah kotak kosong dan permainan masih berjalan
    if board[index] == " " and game_running:
        board[index] = current_player
        # Memperbarui teks tombol yang diklik dan menonaktifkannya
        buttons[index].config(text=current_player, state="disabled", disabledforeground="black")
        
        # Memeriksa apakah ada pemenang setelah giliran ini
        if check_win(current_player):
            winner = current_player
            game_running = False
            messagebox.showinfo("Tic Tac Toe", f"Pemain {winner} MENANG!")
            disable_all_buttons()
        # Memeriksa apakah permainan seri
        elif check_draw():
            game_running = False
            messagebox.showinfo("Tic Tac Toe", "Permainan SERI!")
            disable_all_buttons()
        # Jika belum menang atau seri, ganti giliran pemain
        else:
            current_player = "O" if current_player == "X" else "X"
            status_label.config(text=f"Giliran Pemain: {current_player}")

def check_win(player):
    """Memeriksa semua kondisi menang (baris, kolom, diagonal)."""
    win_conditions = [
        (0, 1, 2), (3, 4, 5), (6, 7, 8), # Baris
        (0, 3, 6), (1, 4, 7), (2, 5, 8), # Kolom
        (0, 4, 8), (2, 4, 6)             # Diagonal
    ]
    for condition in win_conditions:
        if all(board[i] == player for i in condition):
            return True
    return False

def check_draw():
    """Memeriksa apakah permainan seri (semua kotak terisi)."""
    # Jika tidak ada lagi spasi kosong (" ") di papan, maka seri
    return " " not in board

def reset_game():
    """Mereset ulang permainan ke kondisi awal."""
    global board, current_player, game_running
    board = [" " for _ in range(9)]
    current_player = "X"
    game_running = True
    status_label.config(text=f"Giliran Pemain: {current_player}")
    # Mengaktifkan kembali semua tombol dan menghapus teksnya
    for button in buttons:
        button.config(text=" ", state="normal")

def disable_all_buttons():
    """Menonaktifkan semua tombol setelah permainan selesai (menang/seri)."""
    for button in buttons:
        button.config(state="disabled")

# --- Setup GUI Tkinter ---

# Membuat jendela utama aplikasi
root = tk.Tk()
root.title("Tic Tac Toe (Python Tkinter)")
root.resizable(False, False) # Mencegah perubahan ukuran jendela

# Frame untuk menampung grid papan permainan
board_frame = tk.Frame(root)
board_frame.pack(pady=10)

# Membuat 9 tombol (3x3 grid)
for i in range(9):
    button = tk.Button(
        board_frame,
        text=" ",
        font=("Helvetica", 20, "bold"),
        height=2,
        width=5,
        # Menggunakan lambda untuk meneruskan indeks ke handle_click saat tombol diklik
        command=lambda index=i: handle_click(index)
    )
    # Menempatkan tombol dalam grid (baris i//3, kolom i%3)
    button.grid(row=i // 3, column=i % 3, padx=5, pady=5)
    buttons.append(button)

# Label untuk menampilkan status atau giliran pemain
status_label = tk.Label(root, text=f"Giliran Pemain: {current_player}", font=("Helvetica", 14))
status_label.pack(pady=10)

# Tombol reset untuk memulai ulang permainan
reset_button = tk.Button(root, text="Mulai Ulang", command=reset_game)
reset_button.pack(pady=10)

# Memulai loop utama Tkinter agar jendela tetap terbuka dan interaktif
root.mainloop()
