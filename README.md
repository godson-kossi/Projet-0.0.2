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
