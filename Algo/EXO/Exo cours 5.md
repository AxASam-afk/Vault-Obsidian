Exo1: Accés au donnée 
créez un tableau nomé sport contenant les élements suivants : Football Tennis Basket, Judo
Ecrivez l'instruction êrmettant d'affichez uniqment le mots basket 
```Pyton
sports = ["Football", "Basketball", "Tennis", "Cricket", "Baseball"]
print(sports[1])
```
Exo2: Mise a jour d'un donnée Soit le tablea suivant scores = [150,220,85,300]
le joeur ayant faut yb ke score de 85 a obetenu un bonus de 10 points. Modifier la valeur dans le tablea pour qu'elle debienne 95 puis affcihez le tableau complet

```python
scores = [ 150, 220 , 85, 300]
scores[2] = 95

for n in scores:
    print(n)
```
exo 3:
emplissage dynamique 
Créeez un tableau vide nommé liste de courses s demandez 4 fois a lu'tilisateur d'entre un arcticle et rangez chaque saissie dans le tableau affichez ensuite votre panier continient suivis du tablea

```Python
liste_courses = []
print("Bienvenue dans votre liste de courses !")

for i in range(5):
    item = input(f"Entrez l'article {i + 1} : ")
    liste_courses.append(item)

print("Voici votre liste de courses :")
for item in liste_courses:
    print(f"- {item}")
```

exo 4:
Le duo de tete 
SOit le tableau nombres = [5 10 15 20]
Crée une fonction qui re,voie les preier de tableau grâce au slicing 
```Python
nombre = [5, 10 ,15 , 20] 
def AffichezPremier():
    nombres = nombre[0:2]
    print(nombres)    

AffichezPremier()
```

Exo5 Filtrage de nite 
Soit le tableau note_eleve Parcourer le tableau et affichiez uniquement les notes qui sont supérieur a 10
```Python
note = [14, 9, 18, 10, 7, 12]


def Affichez_note(note):
    for n in note:
        if n >= 10:
            print(f"Note: {n}") 

Affichez_note(note)
```
exo 6;: Suppression d"elements Soit le tableau inventaire = Suprimer l'element potion du tablea Supprimez le tout dernier élement du tablea Affichez le tableau final 
```python
item = ["Epee", "Bouclier", "Potion de soin", "Arc", "Flèche"]

  

def AfficherInventaire():

    for items in item:

        print(items)

  

def SupprimerItem():

    item.pop(2)

  

def SupprimerLastItem():

    item.pop()

  

AfficherInventaire()

print("-----")

SupprimerItem()

AfficherInventaire()

SupprimerLastItem()

AfficherInventaire()
```

Exo 7: Recher d'element crée un tableau coueur demandez a l'utilisateur de sasisir une couleur Verifiez si la coeur saisie est présente dans le table si c'est le cas addichez
```python
couleur = ["rouge", "vert", "bleu", "jaune", "violet"]
gt 

def SearchColor(color):
    if color in couleur:
        return f"{color} est dans la liste."
    else:
        return f"{color} n'est pas dans la liste."

while True:
    ask = input("Entrez une couleur à rechercher: ")
    if ask == "quitter":
        break
    result = SearchColor(ask)
    print(result)
    

```

exo8:
```Python
liste_noms = ["Marc", "Léa", "Paul", "Lola", "Sophie", "Julien", "Emma", "Lucas", "Chloé", "Maxime"]

liste_notes = [12, 16, 9, 18, 15, 14, 20, 11, 17, 13]

liste_final_noms = []

liste_finale_notes = []

  
  
  

def filtrer_candidat(liste_noms, liste_notes):

    for i in range(len(liste_notes)):

        if liste_notes[i] >= 15:

            liste_final.append(liste_noms[i])

    return liste_final

  
  
  

def afficher_candidat(liste_final):

    for nom in liste_final:

        print(nom)

        print("------------")

  
  

filtrer_candidat(liste_noms, liste_notes)

afficher_candidat(liste_final)
```
Exo9
```Python
articles = ["Pommes", "Bannanes", "Oranges"]
quantités = [50,20,10]

  

def AffichezIndex(articles, quantités):
    for n in range(len(articles)):
        print(f"Index: {n}, Article: {articles[n]}, Quantité: {quantités[n]}")

  

def VerrifiezQuantité(quantités, choix, quantite):
    if choix < 0 or choix >= len(quantités):
        print("Choix invalide.")
        return
    if quantités[choix] < quantite:
        print(f"L'article {articles[choix]} n'est pas disponible avec une quantité de {quantite}.")
    else:
        print(f"Vous avez choisi l'article {articles[choix]} avec une quantité de {quantite}.")

  

def choix_article(articles, quantités, choix):
    if choix < 0 or choix >= len(articles):
        print("Choix invalide.")
        return
    else:
        VerrifiezQuantité(quantités, choix, quantite)

while True:
    AffichezIndex(articles, quantités)
    choix = int(input("Choisissez un index d'article: "))
    quantite = int(input("Choisissez une quantité: "))
    choix_article(articles, quantités, choix)
```
exo10: 