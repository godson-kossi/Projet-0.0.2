# Projet-0.0.2

Sujet de Projet : Création d’un Jeu du Pendu en Python avec Tkinter
Contexte :
Le jeu du pendu est un jeu classique où un joueur doit deviner un mot lettre par lettre, avec un nombre limité de tentatives. À chaque lettre incorrecte, une partie d'un bonhomme est dessinée. Si le mot est deviné correctement avant que le bonhomme ne soit entièrement dessiné, le joueur gagne. Sinon, le joueur perd.

Objectif :
Vous êtes chargé de développer une version graphique du jeu du pendu en utilisant Python et la bibliothèque Tkinter. Le jeu devra inclure plusieurs fonctionnalités interactives pour rendre l'expérience utilisateur agréable et fluide.

Spécifications :
Interface Utilisateur :

Créez une interface graphique à l'aide de Tkinter qui permet de :
Afficher l’état actuel du mot à deviner sous forme de tirets (_) correspondant aux lettres non découvertes.
Permettre à l'utilisateur de sélectionner une lettre en cliquant sur un bouton pour chaque lettre de l'alphabet.
Afficher un dessin du bonhomme qui se complète à chaque erreur.
Afficher le score du joueur, les tentatives restantes, et le niveau actuel.
Logique de Jeu :

Le mot à deviner doit être choisi aléatoirement parmi une liste de mots.
Pour chaque lettre devinée correctement, révélez la ou les lettres dans le mot.
Si une lettre est incorrecte, réduisez le nombre de tentatives restantes et ajoutez une partie du bonhomme sur le dessin.
Le jeu se termine lorsque le mot est deviné correctement ou lorsque toutes les tentatives sont épuisées (le bonhomme est entièrement dessiné).
Système de Score :

Implémentez un système de score qui :
Ajoute des points pour chaque lettre devinée correctement.
Récompense davantage les mots plus longs ou contenant des lettres plus rares.
Fait progresser le joueur de niveau en niveau avec des mots de plus en plus difficiles.
Fonctionnalités Bonus (optionnelles) :

Ajouter une horloge qui affiche le temps écoulé depuis le début du jeu.
Afficher le meilleur score atteint par le joueur dans une session précédente (en sauvegardant les scores dans un fichier local).
Ajouter la possibilité de choisir le niveau de difficulté (facile, moyen, difficile), qui pourrait affecter le nombre de tentatives ou la longueur des mots à deviner.
Ajouter une animation pour le bonhomme lorsque le joueur perd ou gagne.
Contraintes :
Le jeu doit être entièrement interactif via une interface Tkinter.
Le code doit être propre, bien structuré et commenté.
Vous pouvez utiliser des bibliothèques supplémentaires comme PIL (Pillow) pour la gestion des images, mais l'utilisation de bibliothèques pour simplifier la logique du jeu (comme pygame) est interdite.
Critères d'évaluation :
Fonctionnalité : Le jeu est-il complet et fonctionne-t-il correctement selon les spécifications ?
Interface Utilisateur : L'interface est-elle claire et agréable à utiliser ?
Qualité du Code : Le code est-il bien structuré, lisible et commenté ?
Créativité : Des fonctionnalités supplémentaires ou des améliorations ont-elles été ajoutées pour enrichir l'expérience de jeu ?
Ressources :
Bibliothèque Tkinter : https://docs.python.org/3/library/tkinter.html
Pillow (Gestion d'images) : https://pillow.readthedocs.io/en/stable/
Durée estimée :
Ce projet devrait prendre environ 5 à 7 heures pour être complété, selon le niveau de complexité des fonctionnalités bonus.



///////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////



import tkinter as tk
from tkinter import simpledialog, messagebox
from PIL import Image, ImageTk
import random
import time

# Liste des mots à deviner
word_list = ["JAURES", "SAMUEL", "RUBY", "C", "LEO", "KOSSI", "GO", "BELINA", "TOM", "ARTHUR", "ALEXANDRE", "VICTOR", "CONSTANT"]

# Niveaux d'étoiles avec caractères associés
star_level1_chars = {'A', 'E', 'I', 'O', 'U'}
star_level2_chars = {'T', 'R', 'N', 'S'}

# Variables globales
remaining_attempts = 5
score = 0
level = 1
entry_boxes = []
word_to_guess = ""
masked_word = []
best_score = 0
target_score = 0
start_time = None

class TimeCircle(tk.Canvas):
    def __init__(self, master, **kwargs):
        super().__init__(master, **kwargs)
        self.create_oval(0, 0, kwargs['width'], kwargs['height'], outline='black', width=2)
        self.text_id = self.create_text(kwargs['width'] // 2, kwargs['height'] // 2, text="", font=('Arial', 10))

    def update_time(self, time_str):
        self.itemconfig(self.text_id, text=time_str)

def draw_hangman(attempts):
    canvas.delete("hangman")

    base_x = 250
    base_y = 80
    cord_length = 20

    if attempts < 1:  # Tête
        canvas.create_oval(base_x - 20, base_y + cord_length - 20, base_x + 20, base_y + cord_length + 20, outline="black", width=2, tags="hangman")
    if attempts < 1:  # Corps
        canvas.create_line(base_x, base_y + cord_length, base_x, base_y + cord_length + 50, fill="black", width=2, tags="hangman")
    if attempts < 1:  # Bras gauche
        canvas.create_line(base_x, base_y + cord_length + 10, base_x - 30, base_y + cord_length + 40, fill="black", width=2, tags="hangman")
    if attempts < 1:  # Bras droit
        canvas.create_line(base_x, base_y + cord_length + 10, base_x + 30, base_y + cord_length + 40, fill="black", width=2, tags="hangman")
    if attempts < 1:  # Jambe gauche
        canvas.create_line(base_x, base_y + cord_length + 50, base_x - 30, base_y + cord_length + 100, fill="black", width=2, tags="hangman")
    if attempts < 1:  # Jambe droite
        canvas.create_line(base_x, base_y + cord_length + 50, base_x + 30, base_y + cord_length + 100, fill="black", width=2, tags="hangman")

def start_new_level(attempts_label, score_label, level_label):
    global remaining_attempts, word_to_guess, masked_word, entry_boxes, start_time

    word_to_guess = random.choice(word_list)
    masked_word = ["_" for _ in word_to_guess]
    remaining_attempts = 5
    start_time = time.time()  # Enregistrer le temps de début

    for entry in entry_boxes:
        entry.destroy()
    
    entry_boxes.clear()

    for i, _ in enumerate(word_to_guess):
        entry = tk.Entry(window, width=2, font=('Arial', 18), justify='center')
        entry.place(x=180 + i * 40, y=300)
        entry.insert(0, "_")
        entry.config(state="disabled")
        entry_boxes.append(entry)

    attempts_label.config(text=f"Essais restants: {remaining_attempts}")
    score_label.config(text=f"Score: {score}")
    level_label.config(text=f"Niveau: {level}")

    draw_hangman(remaining_attempts)

def on_letter_click(letter, attempts_label, score_label, level_label, hour_circle, minute_circle, second_circle):
    global remaining_attempts, score, word_to_guess, level, best_score

    if letter in word_to_guess:
        for i, char in enumerate(word_to_guess):
            if char == letter:
                masked_word[i] = letter
                entry_boxes[i].config(state="normal")
                entry_boxes[i].delete(0, tk.END)
                entry_boxes[i].insert(0, letter)
                entry_boxes[i].config(state="disabled")
                
        if "_" not in masked_word:
            if any(char in star_level1_chars for char in word_to_guess):
                score += 2
            if any(char in star_level2_chars for char in word_to_guess):
                score += 3
            
            if len(word_to_guess) > 4:
                score += 3
            else:
                score += 1
            
            score_label.config(text=f"Score: {score}")
            
            level += 1
            if score > best_score:
                best_score = score
            messagebox.showinfo("Gagné", f"Félicitations ! Vous passez au niveau {level}.")
            start_new_level(attempts_label, score_label, level_label)
            return
    
    else:
        remaining_attempts -= 1
        attempts_label.config(text=f"Essais restants: {remaining_attempts}")
        draw_hangman(remaining_attempts)
        
        if remaining_attempts == 0:
            messagebox.showinfo("Perdu", f"Dommage ! Vous avez perdu. Le mot était {word_to_guess}.")
            return

def ask_for_target_score():
    global target_score
    target_score_str = simpledialog.askstring("Objectif du Score", "Entrez l'objectif de score:")
    if target_score_str is not None:
        try:
            target_score = int(target_score_str)
        except ValueError:
            messagebox.showerror("Erreur", "Veuillez entrer un nombre valide.")
            ask_for_target_score()

def update_clock(clock_label):
    current_time = time.strftime('%H:%M:%S')
    clock_label.config(text=f"Temps act: {current_time}")
    window.after(1000, update_clock, clock_label)

def update_elapsed_time(hour_circle, minute_circle, second_circle):
    global start_time
    if start_time:
        elapsed_time = time.time() - start_time
        hours, remainder = divmod(int(elapsed_time), 3600)
        minutes, seconds = divmod(remainder, 60)
        hour_circle.update_time(f"{hours:02}")
        minute_circle.update_time(f"{minutes:02}")
        second_circle.update_time(f"{seconds:02}")
    window.after(1000, update_elapsed_time, hour_circle, minute_circle, second_circle)

def hangman_gui():
    global entry_boxes, window, canvas
    
    window = tk.Tk()
    window.title("Jeu du Pendu")
    window.geometry("600x400")

    ask_for_target_score()

    background_img = Image.open('C:/Users/AMEVOR kossi/Downloads/2.png')
    background_img = background_img.resize((600, 400), Image.LANCZOS)
    bg_photo = ImageTk.PhotoImage(background_img)

    canvas = tk.Canvas(window, width=600, height=400)
    canvas.pack(fill="both", expand=True)

    canvas.create_image(0, 0, anchor=tk.NW, image=bg_photo)

    canvas.create_line(200, 50, 200, 220, width=9)  # Poteau vertical
    canvas.create_line(200, 50, 250, 50, width=5)   # Poteau horizontal
    canvas.create_line(250, 50, 250, 80, width=3)   # Corde

    star_label1 = tk.Label(window, text="⭐", font=('Arial', 12), bg="#87CEEB")
    star_label1.place(x=10, y=300)
    
    star_label2 = tk.Label(window, text="⭐⭐", font=('Arial', 12), bg="#FFD700")
    star_label2.place(x=10, y=350)

    clock_label = tk.Label(window, text="", font=('Arial', 12), bg="#87CEEB")
    clock_label.place(x=430, y=140)
    update_clock(clock_label)

    hour_circle = TimeCircle(window, width=25, height=25)
    hour_circle.place(x=10, y=100)
    
    minute_circle = TimeCircle(window, width=25, height=25)
    minute_circle.place(x=40, y=100)
    
    second_circle = TimeCircle(window, width=25, height=25)
    second_circle.place(x=70, y=100)

    update_elapsed_time(hour_circle, minute_circle, second_circle)

    letters = "ABCDEFGHIJKLMNOPQRSTUVWXYZ"
    letter_buttons = {}
    for i, letter in enumerate(letters):
        btn = tk.Button(window, text=letter, width=3, height=1, font=('Arial', 12),
                        bg="#D2B48C", fg="white", 
                        command=lambda l=letter: on_letter_click(l, attempts_label, score_label, level_label, hour_circle, minute_circle, second_circle))
        btn.place(x=435 + (i % 4) * 40, y=185 + (i // 4) * 30)
        letter_buttons[letter] = btn

    score_frame = tk.Frame(window, bg="#87CEEB")
    score_frame.place(x=10, y=10, width=380, height=30)

    level_label = tk.Label(score_frame, text=f"Niveau: {level}", font=('Arial', 10), bg="#87CEEB")
    level_label.grid(row=0, column=0, padx=5)
    
    tk.Label(score_frame, text=f"Objectif: {target_score}", font=('Arial', 10), bg="#87CEEB").grid(row=0, column=1, padx=5)
    tk.Label(score_frame, text=f"Meilleur: {best_score}", font=('Arial', 10), bg="#87CEEB").grid(row=0, column=2, padx=5)
    
    score_label = tk.Label(score_frame, text=f"Score: {score}", font=('Arial', 10), bg="#87CEEB")
    score_label.grid(row=0, column=3, padx=5)

    attempts_label = tk.Label(window, text=f"Essais restants: {remaining_attempts}", font=('Arial', 10), bg="#87CEEB")
    attempts_label.place(x=180, y=350)

    start_new_level(attempts_label, score_label, level_label)

    window.mainloop()

hangman_gui()


