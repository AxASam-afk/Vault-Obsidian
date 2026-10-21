exo1:LE compte à rebour
objectif utiliser une boucle for 
énoncé: Ecrivez un programe qui affiche les niombre de 10 usqu'a 1
puis affichez décollage 
````python
for i in range(10, 0, -1):

    print("Lancement dans", i)

print("Décollage !")
resultat
````
exo2:tale de multuplication,
objectif: utiliserune boucle for et une saisie utilisateur
énoncé: demandez à l'utilisateur un nombre entier le programe doit lui rendre toute les tables 
````python
nbr = int(input("Entrez un nombre : "))

for i in range(1, 11):

    print(f"{nbr} x {i} = {nbr * i}")
````

exo3:La somme des premiers entiers (accumulateur
objectif : utiliser un accumulateur numérique
énoncé; Demandé à l'utilisateur un nombre entier en N Calculez la sommes de tous les nombres
````python
nbr = int(input("Entrez un nombre : "))
s = 0
i = 1
 
while i <= nbr:
    print(f"Ceci est s:{s} Ceci est i:{i}")
    s = s+i
    i = i + 1
print("La somme est :", s)
````
exo4: Le mots de passe obtinée 
objectif:Utiliser un bouclee while
énoncé:Créez un programe qui demande un mot de passe à l'utilisateru tant que le mdp saisi n'est pas bravo,le programes doit affichier mots de passe incortect
````python
mdp = ""

while mdp != "bravo"
    mdp = input("Entrez le mot de passe : ")
    if mdp != "bravo":
        print("Mot de passe incorrect")
print("Mot de passe correct")
````
exo6: Construction de phrase(Acumulateur de text
Objectif:Accumuler des chaines de cattactères
2noncé: Demandez à l'utilisateur combien de mots il souhaite saisir à l'aide d'une boucle demandez chaque mot un par un et assezmbez les pour fomer une phrase complète 
````python
phrase_finale = ""
nbr_mots = int(input("Combien de mots voulez-vous entrer ?: "))

for i in range(nbr_mots):
    mot = input("Entrez un mot : ")
    phrase_finale = phrase_finale + " " + mot
    
print("Votre phrase est :", phrase_finale)
````
exo:7 LE juste Millieu(avec indices et plus de tentatives 
objectif: combiner boucle while ,compteur et condition avec indicies max 5 essaie 
A chaque tentitive )
```python
import random
  
nbr_secret = random.randint(1, 100)

print("=" * 40)
print("       JEU DU NOMBRE SECRET")
print("=" * 40)
print("Je pense à un nombre entre 1 et 100.")
print("À vous de le deviner !")
print("=" * 40)
essais = 0
while True:
    try:
        nbr = int(input("Votre proposition : "))
        essais += 1
        if essais > 10:
            print("=" * 40)
            print("Désolé, vous avez dépassé le nombre d'essais autorisés.")
            print(f"Le nombre secret était : {nbr_secret}")
            print("=" * 40)
            break
  
        if nbr < 1 or nbr > 100:
            print("Veuillez entrer un nombre entre 1 et 100.")
  
        if nbr < nbr_secret:
          print("Trop petit !")

  
        elif nbr > nbr_secret:
            print("Trop grand !")

        else:
          print("=" * 40)
          print("BRAVO ! Vous avez trouvé le nombre secret.")
          print(f"Le nombre était : {nbr_secret}")
          print(f"Nombre d'essais : {essais}")
            print("=" * 40)
            break

    except ValueError:
        print("Veuillez entrer un nombre valide.")
```