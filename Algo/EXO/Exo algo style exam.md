exo1:Le gestionaire de vente au détails
Fonction Conditions boucle while type de donnée
Crée une fonction calculer_total qui recois un prix_unitaire et une quantite
lafonction calcule le montant de base Si ce montant est supérieur a 100euro elle applique une remise de 10%
Elle doit renvoyer (return) le montant final après remis
Le programe principale:
Demandez le nom de l'article de lu'tilisateur
Demandez le prix de l'article 
Utilisez une boucle while pour demandez la quantité Si l'utilisateur saisi un nombre supérieur ou égal à 0 affichez un message d'erreur et redemandez la saisie jusqu'a qu'elle soit correcte Appelez la fonction avec les données sasisos affichez le resusltat final sous la forme "Pour l'article, nom, le prix final est de resultat en euro"
```python
def calculer_total(prix_unitaire,quantité):
    total=prix_unitaire * quantité
    if total > 100:
        return total * 0.9
    else:
        return total
nom = str(input("Nom de l'article "))
prix_unitaire = int(input("Prix de l'article: "))
quantite = 0

 

while quantite <= 0:
    quantite = int(input("Nombre d'article voulue: "))

print(f"Pour l'article {nom}, acheté en {quantite} exemplaires, "f"le total s'élève à {calculer_total(prix_unitaire, quantite)} €")
```
exo2: Le simultaeur de Guichet automatique
Match case condition boucle 
1.Initialisation Crée un variable solde initialisé a 500e
2. la boucle de mneu affichez un menu dans une boucle tant que l'utilisateur ne chosit pas de quitter 
Option 1 afficher solde actuel 
Option 2 déposer de l'argent 
Option 3 Retiure de l'argent 
Option 4 Quitter le programe

3Logique de retrait Pour l'option 3vous devez verifier si le solde est suffisant SI l'utilisateur deamnde plus que ce qu'il ne possede affichez Fonds insufisant Dsinon effectuer le retirer 

```python
  

  

solde = 500

  

while True:

    print("\n--- MENU BANCAIRE ---")

    print("1. Afficher le solde actuel")

    print("2. Déposer de l'argent")

    print("3. Retirer de l'argent")

    print("4. Quitter le programme")

  

    choix = int(input("Choisissez une option : "))

  

    match choix:

        case 1:

            print("\n")

            print("="*40)

            print(f"Votre solde actuel est de {solde} €")

            print("="*40)

  

        case 2:

            montant = float(input("Montant à déposer : "))

  

            if montant > 0:

                solde += montant

                print("\n")

                print("="*40)

                print(f"Dépôt effectué. Nouveau solde : {solde} €")

                print("="*40)

            else:

                print("Le montant doit être positif.")

  

        case 3:

            print("\n")

            print("="*40)

            montant = float(input("Montant à retirer : "))

  

            if montant > solde:

                print("\n")

                print("="*40)

                print("Fonds insuffisants.")

                print("="*40)

            elif montant <= 0:

                print("\n")

                print("="*40)

                print("Le montant doit être positif.")

                print("="*40)

            else:

                solde -= montant

                print("\n")

                print("="*40)

                print(f"Retrait effectué. Nouveau solde : {solde} €")

                print("="*40)

  

        case 4:

            print("\n")

            print("="*40)

            print("Merci d'avoir utilisé notre service bancaire.")

            print("="*40)

            break

  

        case _:

            print("Option invalide. Veuillez choisir entre 1 et 4.")
```