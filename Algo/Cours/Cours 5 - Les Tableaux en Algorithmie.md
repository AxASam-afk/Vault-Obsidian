	
- "algorithmique"
- "python"
- tableaux
- listes aliases:
- Tableaux
- Listes Python liens:
- "[Cours 1 : Introduction à Python](Algorithmie%20%E2%80%94%20Introduction%20%C3%A0%20Python.md)"
- "[Cours 3 : Structures itératives (boucles)](Les%20boucles.md)"
- "[Cours 4 : Fonctions](Les%20Fonctions.md)"
- "[Language C/Tableaux.md](../../Language%20C/Cours/Tableaux.md)"

---

# Cours 5 : Les Tableaux en Algorithmie

> [!abstract] Objectif du cours Savoir stocker, lire, modifier et parcourir **plusieurs valeurs dans une seule variable** grâce aux tableaux (appelés _listes_ en Python).

**Sommaire**

- [1. Introduction : l'analogie du train](#1.%20Introduction%20%3A%20l%27analogie%20du%20train)
- [2. Déclaration et structure](#2.%20D%C3%A9claration%20et%20structure)
- [3. Index vs Valeur](#3.%20Index%20vs%20Valeur)
- [4. Le slicing](#4.%20Le%20slicing)
- [5. Manipuler un tableau](#5.%20Manipuler%20un%20tableau)
- [6. Remplir un tableau dynamiquement](#6.%20Remplir%20un%20tableau%20dynamiquement)
- [7. Parcourir un tableau](#7.%20Parcourir%20un%20tableau)
- [8. Tableaux et fonctions](#8.%20Tableaux%20et%20fonctions)
- [Récapitulatif](#R%C3%A9capitulatif)

---

## Pourquoi les tableaux ?

Dans les cours précédents, on stockait une information dans une variable (une boîte). Mais comment faire pour gérer **100 notes d'élèves** ? Créer 100 variables (`note1`, `note2`, ...) serait impossible.

C'est là qu'interviennent les **tableaux**.

---

## 1. Introduction : l'analogie du train

> [!important] Définition : Tableau Un **tableau** est une variable spéciale qui peut contenir **plusieurs valeurs à la fois**.

Imaginons un train :

|Élément du train|Correspondance en algorithmie|
|---|---|
|Le train entier|La variable (le **tableau**)|
|Un wagon|Un emplacement pour une donnée|
|Le numéro du wagon|L'**index**|

```text
 Index :    0         1         2         3
         +-------+ +-------+ +-------+ +-------+
         |  12   | |  15   | |   8   | |  19   |
         +-------+ +-------+ +-------+ +-------+
          wagon 0    wagon 1   wagon 2   wagon 3
```

> [!warning] Attention Le premier wagon porte le numéro **0**, pas 1.

---

## 2. Déclaration et structure

En informatique, un tableau se reconnaît aux **crochets** `[ ]`.

```python
# Un tableau de nombres (entiers)
notes = [12, 15, 8, 19]

# Un tableau de texte (chaînes de caractères)
amis = ["Alice", "Bob", "Charlie"]

# Un tableau vide (une étagère prête à être remplie)
panier = []
```

### 2.1 Diversité des données (l'hétérogénéité)

Contrairement à d'autres langages, un tableau en Python peut contenir des **types de données différents** en même temps.

```python
# On mélange texte, entier, réel et booléen
utilisateur = ["Nicolas", 15, 1.75, True]

print("Nom :", utilisateur[0])
print("Âge :", utilisateur[1])
```

> [!warning] Bonne pratique Même si c'est possible, on évite souvent de mélanger les types si on prévoit de faire des calculs (on ne peut pas additionner un nom et une note !).

---

## 3. Index vs Valeur

> [!important] Le point le plus important du cours Il ne faut **jamais confondre** l'adresse (où c'est rangé) et le contenu (ce qui est rangé).

- **L'index** : le numéro de la « case ». On commence **toujours à compter à 0**.
- **La valeur** : la donnée stockée dans la case.

> [!example] Exemple Dans `amis = ["Alice", "Bob", "Charlie"]` :

- à l'index `0`, la valeur est `"Alice"` ;
- à l'index `1`, la valeur est `"Bob"` ;
- à l'index `2`, la valeur est `"Charlie"`.


```python
amis = ["Alice", "Bob", "Charlie"]

print("Le premier de la liste est", amis[0])
print("Le deuxième de la liste est", amis[1])
```

---

## 4. Le slicing

> [!important] Définition : Slicing Le **slicing** (« tranchage ») permet d'extraire une portion précise d'un tableau pour créer une **sous-liste**. On utilise le symbole `:` entre les crochets.

### La règle d'or : inclus / exclu

Syntaxe : `tableau[début : fin]`

- L'index de **début** est **inclus**.
- L'index de **fin** est **exclu** (l'ordinateur s'arrête juste avant).

```python
scores = [10, 20, 30, 40, 50, 60]

# On veut extraire les scores des index 1, 2 et 3
milieu = scores[1:4]
print("Portion extraite :", milieu)  # Affiche : [20, 30, 40]
```

```text
Index   :  0    1    2    3    4    5
Valeur  : 10   20   30   40   50   60
               ^---------^
            début=1 (inclus)   fin=4 (exclu)
```

---

## 5. Manipuler un tableau

### A. Mesurer : `len()`

La fonction `len()` (pour _length_) donne le **nombre d'éléments**. C'est vital pour savoir quand s'arrêter dans une boucle.

```python
amis = ["Alice", "Bob"]
taille = len(amis)
print("J'ai", taille, "amis dans ma liste")
```

### B. Ajouter : `.append()`

On ajoute un élément **à la fin** du tableau.

```python
fruits = ["Pomme", "Banane"]
fruits.append("Orange")
print("Ma liste est", fruits)
```

### C. Modifier

On utilise l'**index** pour changer le contenu d'une case précise.

```python
notes = [10, 12, 14]
notes[0] = 20  # On remplace le 10 par 20
```

### D. Supprimer : `.pop()`

La méthode `.pop()` permet de retirer un wagon du train.

- **Avec un index** : on retire l'élément à une place précise.
- **Sans index** : on retire le tout dernier élément.

```python
amis = ["Alice", "Bob", "Charlie"]

# On retire Bob qui est à l'index 1
amis.pop(1)
print("Après avoir retiré l'index 1, il reste", amis)

# On retire le dernier élément de la liste (Charlie)
amis.pop()
print("Après avoir retiré le dernier, il reste", amis)
```

> [!summary] Fonctions essentielles
> 
> |Opération|Syntaxe|Effet|
> |---|---|---|
> |Mesurer|`len(t)`|Nombre d'éléments|
> |Ajouter|`t.append(x)`|Ajoute `x` à la fin|
> |Modifier|`t[i] = x`|Remplace la valeur à l'index `i`|
> |Supprimer|`t.pop(i)` / `t.pop()`|Retire l'élément `i` / le dernier|
> 
> 
> 

---

## 6. Remplir un tableau dynamiquement

_Lien avec [Cours 1 : Introduction à Python](Algorithmie%20%E2%80%94%20Introduction%20%C3%A0%20Python.md) et [Cours 3 : Structures itératives (boucles)](Les%20boucles.md)._

On utilise souvent un **tableau vide** comme un **accumulateur** pour stocker des saisies de l'utilisateur.

```python
classe = []
nombre_eleves = 3

for i in range(nombre_eleves):
    nom = input("Entrez le nom de l'élève : ")
    # On ajoute le nom au fur et à mesure
    classe.append(nom)

print("La liste finale est", classe)
```

---

## 7. Parcourir un tableau

_Lien avec [Cours 3 : Structures itératives (boucles)](Les%20boucles.md)._

Pour lire chaque élément un par un, on utilise une boucle `for`.

### Méthode 1 : parcourir les valeurs (direct)

```python
notes = [15, 12, 18]

for n in notes:
    print("La note est", n)
```

### Méthode 2 : parcourir les index (plus précis)

```python
for i in range(len(notes)):
    print("A l'index", i, "on trouve la valeur", notes[i])
```

> [!tip] Quelle méthode choisir ?
> 
> - Méthode 1 : quand on a seulement besoin de **lire les valeurs**.
> - Méthode 2 : quand on a aussi besoin de **l'index** (par exemple pour modifier une case).
> 
> 

---

## 8. Tableaux et fonctions

_Lien avec [Cours 4 : Fonctions](Les%20Fonctions.md)._

Une fonction peut **recevoir** un tableau en paramètre ou en **renvoyer** un.

```python
# Fonction qui calcule la somme des éléments d'un tableau
def calculer_somme(liste_nombres):
    total = 0
    for n in liste_nombres:
        total = total + n
    return total

# Programme principal
mes_notes = [10, 20, 15]
resultat = calculer_somme(mes_notes)
print("La somme totale est", resultat)
```

---

## Récapitulatif

> [!summary] À retenir
> 
> - Un tableau stocke **plusieurs valeurs** dans une seule variable, notée avec `[ ]`.
> - Chaque case a un **index**, et le premier index est **0**.
> - **Index** = adresse de la case, **valeur** = contenu de la case.
> - Slicing `t[début:fin]` : début **inclus**, fin **exclue**.
> - Fonctions essentielles : `len()`, `.append()`, modification par index, `.pop()`.
> - Un tableau vide sert d'**accumulateur** pour collecter des données.
> - On parcourt un tableau avec `for n in t` ou `for i in range(len(t))`.
> - Un tableau peut être passé en **paramètre** d'une fonction.