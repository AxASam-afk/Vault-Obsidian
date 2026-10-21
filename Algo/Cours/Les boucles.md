	
## tags: [algorithmique, programmation, cours, python]

# Cours 3 — Les structures itératives (boucles)

Après avoir appris à stocker des données (Cours 1) et à faire des choix (Cours 2), on apprend ici à **répéter des actions**.

## Objectifs du cours

- Comprendre à quoi sert une boucle
- Savoir utiliser `for` avec `range()`
- Savoir construire un accumulateur (somme, texte)
- Savoir utiliser `while` et le combiner avec des conditions multiples
- Savoir contrôler une boucle avec `break` et `continue`

---

## 1. Pourquoi utiliser des boucles ?

> [!important] Définition Une **boucle** (ou **itération**) permet de répéter une action plusieurs fois sans réécrire le code à chaque fois.

Une boucle permet de :

- répéter une action un **nombre précis** de fois
- répéter une action **tant qu'une condition** n'est pas remplie (ex : attendre le bon mot de passe)

---

## 2. La boucle "Pour" (`for`)

Idéale quand on connaît **à l'avance** le nombre de répétitions.

### A. La fonction `range()`

|Écriture|Résultat|
|---|---|
|`range(5)`|de 0 à 4 (5 nombres au total)|
|`range(1, 6)`|de 1 à 5 (commence à 1, s'arrête juste avant 6)|
|`range(2, 11, 2)`|de 2 à 10, de 2 en 2 (le **pas**)|

### B. Exemple simple

```python
for i in range(1, 4):
    print("Lancement dans", i)
print("Décollage !")
```

**Résultat affiché :**

```
Lancement dans 1
Lancement dans 2
Lancement dans 3
Décollage !
```

---

## 3. Les accumulateurs : calculer et construire

> [!important] Définition Un **accumulateur** est une variable créée **avant** la boucle, qui se met à jour à **chaque tour** pour construire un résultat final (somme, texte, liste…).

### A. Accumulateur de nombres (somme)

```python
total = 0
for i in range(1, 6):
    total = total + i  # On ajoute la valeur de i à chaque tour
print("La somme des nombres de 1 à 5 est", total)
```

**Résultat affiché :**

```
La somme des nombres de 1 à 5 est 15
```

### B. Accumulateur de texte (concaténation)

```python
phrase_finale = ""
for i in range(3):
    mot = input("Entrez un mot : ")
    phrase_finale = phrase_finale + " " + mot  # On "colle" le mot au précédent
print("Votre phrase est :", phrase_finale)
```

**Résultat affiché** (si l'utilisateur saisit "Bonjour", "le", "monde") :

```
Entrez un mot : Bonjour
Entrez un mot : le
Entrez un mot : monde
Votre phrase est :  Bonjour le monde
```

_(le résultat dépend de la saisie ; ici il y a un espace au début car `phrase_finale` part de `""`)_

---

## 4. La boucle "Tant que" (`while`)

Utilisée quand on **ne sait pas à l'avance** combien de fois on va répéter l'action : on continue tant que la condition est vraie.

### A. Syntaxe de base

```python
compteur = 1
while compteur <= 5:
    print("C'est le tour numéro", compteur)
    compteur = compteur + 1  # Très important pour éviter une boucle infinie !
```

**Résultat affiché :**

```
C'est le tour numéro 1
C'est le tour numéro 2
C'est le tour numéro 3
C'est le tour numéro 4
C'est le tour numéro 5
```

> [!important] Attention Sans instruction qui fait évoluer la condition (ici `compteur = compteur + 1`), la boucle `while` tourne indéfiniment → **boucle infinie**.

### B. Combiner plusieurs conditions (`and` / `or`)

```python
tentatives = 0
code_secret = "1234"
reponse = ""

# On continue tant qu'on a moins de 3 essais ET que le code est faux
while tentatives < 3 and reponse != code_secret:
    reponse = input("Entrez le code : ")
    tentatives = tentatives + 1

if reponse == code_secret:
    print("Accès autorisé")
else:
    print("Compte bloqué")
```

**Résultat affiché** (deux scénarios possibles) :

```
# Si le bon code est entré avant le 3e essai
Entrez le code : 0000
Entrez le code : 1234
Accès autorisé

# Si les 3 essais sont faux
Entrez le code : 0000
Entrez le code : 1111
Entrez le code : 2222
Compte bloqué
```

---

## 5. Contrôler la boucle : `break` et `continue`

- `break` : arrête **immédiatement** la boucle
- `continue` : saute la fin du tour actuel et passe directement au tour suivant

```python
for i in range(1, 11):
    # Si i vaut 3, on saute la suite et on passe au tour suivant
    if i == 3:
        continue
    # Si i vaut 7, on stoppe directement toute la boucle
    if i == 7:
        break
    print("Valeur de i :", i)

print("Fin de la boucle")
```

**Résultat affiché :**

```
Valeur de i : 1
Valeur de i : 2
Valeur de i : 4
Valeur de i : 5
Valeur de i : 6
Fin de la boucle
```

_(le 3 est sauté par `continue`, et la boucle s'arrête au 7 à cause du `break`)_

---

## Fiche mémo

- `for` : nombre de répétitions **connu à l'avance**, souvent avec `range()`
- `while` : on répète **tant qu'une condition** est vraie, nombre de tours inconnu à l'avance
- Un accumulateur se déclare **avant** la boucle et se met à jour **à chaque tour**
- Toujours faire évoluer la condition d'un `while` pour éviter une boucle infinie
- `break` = sortie immédiate de la boucle · `continue` = passe au tour suivant

---

liens:
[Exo boucles](../EXO/Exo%20boucles.md)
[Language C/Structures de Contrôle.md](../../Language%20C/Cours/Structures%20de%20Contr%C3%B4le.md)