## 1. Les bases de l'algorithmie

### Qu'est-ce qu'un algorithme ?

Un **algorithme** est un **enchaînement d'instructions** permettant de résoudre un problème ou d'effectuer une tâche.

Tout algorithme fonctionne selon le cycle **ETS** :

```text
Entrée → Traitement → Sortie
```

- **Entrée** : les données fournies à l'algorithme.
    
- **Traitement** : les opérations effectuées sur ces données.
    
- **Sortie** : le résultat obtenu.
    

---

### Qu'est-ce qu'une variable ?

Une **variable** est un espace nommé permettant de **stocker une valeur**.

Exemple :

```python
age = 22
nom = "Samuel"
taille = 1.82
```

Ici :

- `age` contient `22`
    
- `nom` contient `"Samuel"`
    
- `taille` contient `1.82`
    

---

## 2. Les opérateurs mathématiques

|Opérateur|Nom|Explication|Exemple|
|---|---|---|---|
|`+`|Addition|Additionne deux nombres|`5 + 2 → 7`|
|`-`|Soustraction|Soustrait un nombre à un autre|`5 - 2 → 3`|
|`*`|Multiplication|Multiplie deux nombres|`5 * 2 → 10`|
|`/`|Division|Effectue une division|`5 / 2 → 2.5`|
|`//`|Division entière|Donne uniquement le quotient entier|`5 // 2 → 2`|
|`%`|Modulo|Donne le reste d'une division|`5 % 2 → 1`|

> [!IMPORTANT]  
> `//` donne le **quotient entier**, tandis que `%` donne le **reste** de la division.

---

## 3. Exercice — Convertir des secondes en minutes et secondes

### Énoncé

On dispose d'un nombre de secondes. Il faut le transformer en **minutes et secondes**.

Exemple :

```text
135 secondes → 2 minutes et 15 secondes
```

### Solution

```python
secondes = int(input("Entrez le nombre de secondes : "))

minutes = secondes // 60
secondes = secondes % 60

print(str(minutes) + " minutes et " + str(secondes) + " secondes")
```

### Explication

Avec `135` secondes :

```text
135 // 60 = 2
135 % 60 = 15
```

On obtient donc :

```text
2 minutes et 15 secondes
```

---

## 4. Les conventions d'écriture

### Le Snake Case

Le **snake_case** est une convention permettant d'écrire les noms de variables en séparant les mots par des `_`.

Exemple :

```python
age_utilisateur = 22
annee_de_naissance = 2004
nom_utilisateur = "Samuel"
```

On évite généralement :

```python
ageutilisateur
age-utilisateur
```

---

### Les commentaires

En Python, un commentaire commence par `#`.

Il permet d'ajouter une explication dans le code. Le commentaire n'est pas exécuté.

```python
# Demande l'âge de l'utilisateur
age = int(input("Quel âge as-tu ? "))
```

---

### Les deux-points `:`

Les **deux-points** sont utilisés notamment après certaines instructions qui introduisent un bloc de code :

```python
if age >= 18:
    print("Tu es majeur.")
```

Les `:` indiquent que les instructions indentées qui suivent appartiennent au `if`.

---

### L'indentation

L'**indentation** correspond au décalage au début d'une ligne.

En Python, elle est obligatoire pour délimiter les blocs de code.

```python
if age >= 18:
    print("Tu es majeur.")
```

Ici, `print()` est indenté : il appartient donc au `if`.

> [!WARNING]  
> Une mauvaise indentation peut provoquer une erreur ou modifier le fonctionnement du programme.

---

# 5. Les types de données en algorithmie

## `int` — Entier

Le type `int` représente un **nombre entier**.

Exemples :

```python
age = 22
nombre = 100
temperature = -5
```

---

## `str` — Chaîne de caractères

Le type `str` représente du **texte**, c'est-à-dire une chaîne de caractères.

Le texte doit être placé entre guillemets.

```python
nom = "Samuel"
ville = "Paris"
```

> [!NOTE]  
> `"22"` est une chaîne de caractères, tandis que `22` est un entier.

---

## `float` — Nombre décimal

Le type `float` représente un **nombre à virgule**.

En Python, on utilise un **point `.`** et non une virgule `,`.

```python
taille = 1.82
prix = 19.99
```

---

## `bool` — Booléen

Le type `bool` possède seulement **deux valeurs possibles** :

- `True` → vrai
    
- `False` → faux
    

Exemple :

```python
win = True
```

---

# 6. Opérations et compatibilité des types

## Addition vs concaténation

L'opérateur `+` peut avoir un comportement différent selon le type de données.

### Avec des nombres

L'opérateur `+` effectue une **addition mathématique** :

```python
5 + 3
```

Résultat :

```text
8
```

### Avec des chaînes de caractères

L'opérateur `+` effectue une **concaténation**, c'est-à-dire qu'il combine les chaînes.

```python
"Bonjour " + "Samuel"
```

Résultat :

```text
Bonjour Samuel
```

> [!IMPORTANT]  
> On ne peut pas directement additionner un `str` et un `int`.
> 
> ```python
> "Age : " + 22
> ```
> 
> Pour cela, il faut convertir la valeur ou utiliser une **f-string**.

---

# 7. Exercice — Âge dans un an

### Énoncé

Demander l'âge de l'utilisateur et lui indiquer quel âge il aura dans un an.

### Solution

```python
age = int(input("Ton âge actuel : "))

age_plus_un_an = age + 1

print(f"Actuellement tu as {age} ans et dans un an tu auras {age_plus_un_an} ans")
```

### À retenir

```python
age_plus_un_an = age + 1
```

On prend la valeur de `age` et on lui ajoute `1`.

---

# 8. Exercice — Prédiction

### Énoncé

Créer un programme contenant quatre variables :

- `nom`
    
- `age`
    
- `taille`
    
- `win`
    

Puis afficher une phrase indiquant les informations de la personne et si elle a gagné.

### Solution

```python
nom = "Samuel"
age = 22
taille = 1.82
win = True

if win:
    win = "Gagné"
else:
    win = "Perdu"

print(f"{nom}, mesure {taille}m à {age} ans, et a {win}")
```

### Explication

`win` est initialement un booléen :

```python
win = True
```

On utilise ensuite une condition :

```python
if win:
    win = "Gagné"
else:
    win = "Perdu"
```

Si `win` vaut `True`, la variable devient `"Gagné"`.

Sinon, elle devient `"Perdu"`.

---

# 9. Exercice — Calculer l'aire d'un rectangle

### Énoncé

Demander la longueur et la largeur d'un rectangle puis calculer son aire.

La formule est :

```text
Aire = longueur × largeur
```

### Solution

```python
longueur = float(input("Entrez une longueur : "))
largeur = float(input("Entrez une largeur : "))

aire = longueur * largeur

print("L'aire est :", aire)
```

### Exemple

Si :

```text
longueur = 5
largeur = 3
```

Alors :

```text
aire = 5 × 3 = 15
```

---

# 10. Exercice — Trouver l'année de naissance

### Énoncé

Demander l'âge de l'utilisateur et calculer son année de naissance.

### Solution

```python
from datetime import date

age = int(input("Please enter your age: "))

annee = date.today().year
annee_de_naissance = annee - age

print("You were born in the year:", annee_de_naissance)
```

### Explication

On récupère l'année actuelle :

```python
annee = date.today().year
```

Puis on soustrait l'âge :

```python
annee_de_naissance = annee - age
```

> [!NOTE]  
> Ce calcul donne une année approximative si on ne tient pas compte de la date exacte d'anniversaire. Pour déterminer précisément l'année de naissance, il faudrait également connaître la date de naissance ou la date d'anniversaire.


liens:
	[Transition — Des données aux décisions](Transition%20%E2%80%94%20Des%20donn%C3%A9es%20aux%20d%C3%A9cisions.md)
		[Les boucles](Les%20boucles.md)
		[Exo boucles](../EXO/Exo%20boucles.md)
		[Projet  Borne d'Attribution de Logement  Étudiant](../EXO/Projet%20%20Borne%20d%27Attribution%20de%20Logement%20%20%C3%89tudiant.md)
		[Transition — Des données aux décisions](Transition%20%E2%80%94%20Des%20donn%C3%A9es%20aux%20d%C3%A9cisions.md)
		 [Les Fonctions](Les%20Fonctions.md)
