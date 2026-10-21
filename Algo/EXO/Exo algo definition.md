
## tags: [algorithmique, python, ESGI, exercices, corrige, fonctions]

# Corrigé Exercices Cours 4 — Fonctions (Python)

Lié à : [Fonctions (Python)](../Cours/Les%20Fonctions.md) · [Fonctions](../../Language%20C/Cours/Fonctions.md) (version C)

> [!important] Exercices manquants Les énoncés 5 et 8 n'ont pas été transmis — cette fiche ne couvre donc que les exercices 1, 2, 3, 4, 6 et 7. À compléter dès que tu as les deux énoncés manquants.

---

## Exercice 1 — Message de bienvenue

```python
def afficher_accueil():
    print("Bienvenue dans le programme d'algo")

afficher_accueil()
```

> [!important] Le plus simple des cas : une procédure Pas de paramètre, pas de `return` : la fonction se contente d'afficher un texte. C'est une **procédure** (voir [Fonctions (Python)](../Cours/Les%20Fonctions.md)), pas une fonction au sens strict.

---

## Exercice 2 — Calcul du triple

```python
def calculer_triple(nombre):
    return nombre * 3

resultat = calculer_triple(7)
print(f"Le triple de 7 est : {resultat}")
```

> [!important] Toujours définir avant d'appeler La fonction est définie **en premier**, tout en haut du fichier ; le programme principal (l'appel et l'affichage) vient après. C'est la "règle d'or" vue dans [Fonctions (Python)](../Cours/Les%20Fonctions.md).

---

## Exercice 3 — Moyenne de deux notes

```python
def moyenne_deux_notes(note1, note2):
    return (note1 + note2) / 2

note1 = float(input("Entrez note 1 : "))
note2 = float(input("Entrez note 2 : "))
moyenne = moyenne_deux_notes(note1, note2)

print(f"La moyenne est de : {moyenne}")
```

> [!important] `float` plutôt que `int` Des notes peuvent avoir des décimales (12.5, 15.5…) : `float(input(...))` plutôt que `int(input(...))`.

---

## Exercice 4 — Score avec valeur par défaut

```python
def afficher_score(pseudo, point=0):
    print(f"{pseudo} a {point} points")

afficher_score("Alice", 100)
afficher_score("Bob")
```

> [!important] Paramètre par défaut `point=0` dans l'en-tête donne une valeur automatique si l'appelant ne la précise pas — `afficher_score("Bob")` affiche donc `Bob a 0 points`.

---

## Exercice 5 — _(énoncé manquant)_

---

## Exercice 6 — Test de majorité

```python
def est_majeur(age):
    return age >= 18   # déjà un booléen, pas besoin de if/else

if est_majeur(int(input("Entrez votre âge : "))):
    print("Accès autorisé")
else:
    print("Accès refusé")
```

> [!important] Simplification `age >= 18` **est déjà** `True` ou `False` : écrire `if age >= 18: return True else: return False` fait deux fois le même travail pour rien. `return age >= 18` suffit.

---

## Exercice 7 — Surface d'un rectangle

```python
def calculer_surface(longueur, largeur):
    return longueur * largeur

longueur = float(input("Entrez la longueur (L) : "))
largeur = float(input("Entrez la largeur (l) : "))
surface = calculer_surface(longueur, largeur)

print(f"La surface est de {surface} m2")
```

> [!important] Nommer ses variables clairement Éviter `l` seul comme nom de variable : à l'écran, il se confond avec `1` (chiffre un) ou `I` (i majuscule) selon la police utilisée. Préférer `largeur`, `l_rect`, etc.

---

## Exercice 8 — _(énoncé manquant)_

---

## Voir aussi

[Fonctions (Python)](../Cours/Les%20Fonctions.md) · [Fonctions](../../Language%20C/Cours/Fonctions.md) (version C) · [Variables et Opérateurs](../../Language%20C/Cours/Variables%20et%20Op%C3%A9rateurs.md) · [Structures de Contrôle](../../Language%20C/Cours/Structures%20de%20Contr%C3%B4le.md)