# Paper- Scissors- Rock : Python-Game
"""
Python 101
Project: Rock-Paper-Scissors GUI
Author: Jes Williams Mariner
"""

import tkinter as tk
from tkinter import simpledialog, messagebox
import random

# Choices
R = "ROCK"
P = "PAPER"
S = "SCISSORS"
choices = [R, P, S]

# Scores
p1_score = 0
p2_score = 0

# Game mode: PvP or PvC
def choose_mode():
    mode = simpledialog.askstring("Game Mode", "Choose Mode:\n1. Player vs Computer\n2. Player vs Player\nEnter 1 or 2:")
    if mode == "1":
        return "PvC"
    elif mode == "2":
        return "PvP"
    else:
        messagebox.showerror("Error", "Invalid mode. Defaulting to PvC.")
        return "PvC"

game_mode = choose_mode()

# Player names
player1_name = simpledialog.askstring("Player 1", "Enter Player 1 name:").title()
if game_mode == "PvP":
    player2_name = simpledialog.askstring("Player 2", "Enter Player 2 name:").title()
else:
    player2_name = "Computer"

# Tkinter window
root = tk.Tk()
root.title("Rock-Paper-Scissors")
root.geometry("400x300")

result_text = tk.StringVar()
score_text = tk.StringVar()
score_text.set(f"{player1_name}: 0   {player2_name}: 0")

def determine_winner(p1_choice, p2_choice):
    global p1_score, p2_score
    if p1_choice == p2_choice:
        result = f"🤝 Draw! Both chose {p1_choice}"
    elif (p1_choice == R and p2_choice == S) or \
         (p1_choice == S and p2_choice == P) or \
         (p1_choice == P and p2_choice == R):
        p1_score += 1
        result = f"✅ {player1_name} wins! {p1_choice} beats {p2_choice}"
    else:
        p2_score += 1
        result = f"🏆 {player2_name} wins! {p2_choice} beats {p1_choice}"
    result_text.set(result)
    score_text.set(f"{player1_name}: {p1_score}   {player2_name}: {p2_score}")

def player_choice(choice):
    if game_mode == "PvP":
        # Ask Player 2 for input
        p2_choice = simpledialog.askstring(f"{player2_name}'s Turn", f"{player2_name}, enter your choice (Rock/Paper/Scissors):").strip().upper()
        while p2_choice not in choices:
            messagebox.showerror("Invalid Input", "Please enter Rock, Paper or Scissors.")
            p2_choice = simpledialog.askstring(f"{player2_name}'s Turn", f"{player2_name}, enter your choice (Rock/Paper/Scissors):").strip().upper()
    else:
        # Computer random choice
        p2_choice = random.choice(choices)
    determine_winner(choice, p2_choice)

# Buttons
tk.Label(root, text=f"{player1_name}, choose your move:", font=("Arial", 14)).pack(pady=10)
button_frame = tk.Frame(root)
button_frame.pack()

tk.Button(button_frame, text="🪨 Rock", width=10, command=lambda: player_choice(R)).grid(row=0, column=0, padx=5)
tk.Button(button_frame, text="📄 Paper", width=10, command=lambda: player_choice(P)).grid(row=0, column=1, padx=5)
tk.Button(button_frame, text="✂️ Scissors", width=10, command=lambda: player_choice(S)).grid(row=0, column=2, padx=5)

# Result and Score
tk.Label(root, textvariable=result_text, font=("Arial", 12), fg="blue").pack(pady=15)
tk.Label(root, textvariable=score_text, font=("Arial", 12, "bold")).pack(pady=10)

# Exit button
tk.Button(root, text="Exit", command=root.destroy).pack(pady=10)

root.mainloop()
