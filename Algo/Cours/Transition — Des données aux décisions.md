
Dans la première partie, nous avons vu les **bases de la programmation** :

- ce qu'est un Algorithme
    
- le cycle **ETS** : Entrée → Traitement → Sortie
    
- les variables
    
- les différents types de données
    
- les opérateur mathématique
    
- les opérations sur les données
    
- la récupération d'informations avec `input()`
    

Jusqu'ici, nos programmes étaient principalement **linéaires** : les instructions étaient exécutées les unes après les autres.

Par exemple :

```python
age = int(input("Quel âge as-tu ? : "))

age_plus_un = age + 1

print(f"Dans un an, tu auras {age_plus_un} ans.")
```

Le programme suit toujours le même chemin :

```text
Entrée
  ↓
Traitement
  ↓
Sortie
```

Mais un programme doit souvent être capable de **réagir différemment selon la situation**.

Par exemple :

```text
Si l'utilisateur a 18 ans ou plus
→ afficher "Vous êtes majeur"

Sinon
→ afficher "Vous êtes mineur"
```

Le programme doit donc être capable de **comparer des valeurs et de prendre une décision**.

C'est ici qu'interviennent les **structures conditionnelles**.

---

# 2. Les structures conditionnelles

Les structures conditionnelles permettent à un programme de **choisir quel code exécuter en fonction d'une condition**.

On passe donc de :

```text
Données → Calcul → Résultat
```

à :

```text
             ┌── Si condition vraie ──→ Action A
Données → Décision
             └── Sinon ───────────────→ Action B
```

Pour prendre ces décisions, nous allons utiliser :

- les **opérateurs de comparaison** : `==`, `!=`, `>`, `<`, `>=`, `<=`
    
- `if` → si
    
- `elif` → sinon si
    
- `else` → sinon
    
- les **opérateurs logiques** : `and`, `or`, `not`
    
- `match / case` pour gérer plusieurs possibilités
    

> [!IMPORTANT]  
> **Partie 1 :** on apprend à **manipuler les données**.
> 
> **Partie 2 :** on apprend à **prendre des décisions à partir de ces données**.
> 
> Les deux parties sont donc directement liées : les conditions utilisent les variables, les types et les opérateurs étudiés précédemment.


liens:
* [Algorithmie — Introduction à Python](Algorithmie%20%E2%80%94%20Introduction%20%C3%A0%20Python.md)