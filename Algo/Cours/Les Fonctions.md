
## tags: [algorithmique, python, ESGI, fonctions]

# Fonctions (Python)

Lié à : [Fonctions](../../Language%20C/Cours/Fonctions.md) (version C) · [Variables et Opérateurs](../../Language%20C/Cours/Variables%20et%20Op%C3%A9rateurs.md) · [Structures de Contrôle](../../Language%20C/Cours/Structures%20de%20Contr%C3%B4le.md)

## Pourquoi utiliser des fonctions ?

> [!important] Définition Une **fonction** est un bloc de code nommé qui accomplit une tâche précise. Elle permet d'organiser et de **réutiliser** du code sans le réécrire.

Principe **DRY** (_Don't Repeat Yourself_) : si le calcul du prix TTC doit être fait 10 fois dans le programme, mieux vaut l'écrire une fois dans une fonction que de le dupliquer 10 fois.

Avantages :

- Éviter la répétition
- Rendre le code lisible : un nom clair pour une action (ex : `calculer_tva`)
- Faciliter la correction : un seul endroit à modifier si le calcul change

> [!important] Analogie de la recette **Définir** une fonction, c'est écrire une recette de cuisine : l'écrire ne fait pas apparaître le plat. Pour "manger", il faut **appeler** la fonction (l'exécuter).

## Définition et appel

Mot-clé `def` (pour "définir"), suivi du nom de la fonction et de parenthèses.

```python
# 1. Définition de la "recette"
def dire_bonjour():
    print("Bonjour tout le monde !")

# 2. Appel de la fonction (le programme l'exécute ici)
dire_bonjour()
```

> [!important] Ordre de déclaration L'ordinateur lit le fichier de **haut en bas** : il doit connaître une fonction avant qu'on ne l'appelle. Règle d'or : placer toutes les **définitions de fonctions en haut du fichier**. Le reste du code, celui qui s'exécute directement, s'appelle le **programme principal**.

## Les paramètres (les entrées)

Une fonction peut avoir besoin d'informations pour travailler : on place des **paramètres** dans les parenthèses.

```python
def saluer_utilisateur(prenom, age):
    print("Bonjour", prenom, "tu as", age, "ans")

# Lors de l'appel, on donne des "arguments" (les valeurs réelles)
saluer_utilisateur("Nicolas", 15)
saluer_utilisateur("Julie", 22)
```

> [!important] Paramètres vs arguments Les **paramètres** (`prenom`, `age`) sont les noms utilisés dans la définition de la fonction. Les **arguments** (`"Nicolas"`, `15`) sont les valeurs réelles fournies lors de l'appel. Même distinction qu'en C entre paramètres _formels_ et _effectifs_ (voir [Fonctions](../../Language%20C/Cours/Fonctions.md)).

## return vs print — le point le plus important

Il existe deux types de blocs :

- **La procédure** : elle fait une action (comme un `print`) mais ne renvoie rien.
- **La fonction** : elle calcule un résultat et le **renvoie** au programme grâce au mot-clé `return`.

> [!important] Le piège `print()` affiche du texte **pour l'humain**. Mais l'ordinateur ne peut pas "récupérer" ce texte affiché pour faire un autre calcul avec. `return` permet d'**éjecter une valeur** hors de la fonction, pour la stocker dans une variable et la réutiliser ensuite.

```python
def calculer_double(nombre):
    resultat = nombre * 2
    return resultat  # on "éjecte" le résultat

# On stocke le retour dans une variable
mon_score = 10
double_du_score = calculer_double(mon_score)
print("Le double de", mon_score, "est", double_du_score)
```

## Paramètres par défaut

Un paramètre peut avoir une valeur automatique, utilisée si on oublie de la préciser à l'appel.

```python
def presenter_ville(ville="Paris"):
    print("La ville choisie est", ville)

presenter_ville("Lyon")  # affiche : La ville choisie est Lyon
presenter_ville()        # affiche : La ville choisie est Paris
```

> [!important] Spécificité Python Cette syntaxe de valeur par défaut directement dans l'en-tête (`ville="Paris"`) n'existe pas telle quelle en C, qui n'a pas de mécanisme natif de paramètres par défaut.

## La portée des variables (locale vs globale)

> [!important] Variable locale Une variable créée **à l'intérieur** d'une fonction est **locale** : elle n'existe **que dans cette fonction**.

```python
def ma_fonction():
    message_secret = "Ceci est local"
    print(message_secret)

ma_fonction()
# print(message_secret)  # ERREUR ! message_secret n'existe pas ici.
```

Même logique qu'en C (voir la section "Portée des variables locales" de [Fonctions](../../Language%20C/Cours/Fonctions.md)) : chaque fonction a son propre espace de variables, invisible depuis l'extérieur — et une variable locale n'a aucun lien avec une variable globale de même nom.

## Modularité : une fonction dans une fonction

On construit des programmes complexes en assemblant des fonctions, comme des Legos.

```python
def calculer_tva(prix_ht):
    return prix_ht * 0.20

def calculer_total(prix_ht):
    tva = calculer_tva(prix_ht)  # on appelle une fonction ici !
    return prix_ht + tva

prix_article = 100
total = calculer_total(prix_article)
print("Le prix total est de", total, "euros")
```

> [!important] Découpage du problème `calculer_total` ne recalcule pas la TVA elle-même : elle **délègue** ce calcul à `calculer_tva`. C'est exactement le principe "diviser pour mieux régner" déjà vu pour les fonctions en C (voir [Fonctions](../../Language%20C/Cours/Fonctions.md)) : décomposer un problème en sous-problèmes indépendants, chacun confié à sa propre fonction.

## Voir aussi

[Fonctions](../../Language%20C/Cours/Fonctions.md) (version C) · [Variables et Opérateurs](../../Language%20C/Cours/Variables%20et%20Op%C3%A9rateurs.md) · [Structures de Contrôle](../../Language%20C/Cours/Structures%20de%20Contr%C3%B4le.md) · [Tableaux](../../Language%20C/Cours/Tableaux.md)