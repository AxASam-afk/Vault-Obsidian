# Déclarations de variables et types de données

## Pourquoi déclarer ses variables ?

La déclaration obligatoire (`Option Explicit` en entête de module) :
- Évite les fautes de frappe dans les noms de variables
- Facilite le débogage
- Améliore les performances
- Rend le code plus lisible et maintenable

## Syntaxe de déclaration

```vba
Dim nomVariable As Type
Dim nomVariable1 As Type1, nomVariable2 As Type2
```

## Types de données principaux en VBA

### Types numériques
- `Byte` : 0 à 255 (1 octet)
- `Integer` : -32 768 à 32 767 (2 octets)
- `Long` : -2 147 483 648 à 2 147 483 647 (4 octets)
- `Single` : Nombre décimal simple précision (4 octets)
- `Double` : Nombre décimal double précision (8 octets)
- `Currency` : Monétaire, 4 décimales (8 octets)
- `Decimal` : Très haute précision (14 octets)

### Types non numériques
- `String` : Chaîne de caractères (variable longueur)
- `String * n` : Chaîne de longueur fixe n
- `Boolean` : Vrai ou Faux
- `Date` : Dates et heures (janvier 100 à décembre 9999)
- `Object` : Référence à un objet quelconque
- `Variant` : Type par défaut, peut contenir n'importe quel type (à éviter si possible)

## Portée des variables

### Niveau procédure
```vba
Sub MaProcedure()
    Dim variableLocale As Integer  ' Disponible seulement dans cette procédure
End Sub
```

### Niveau module
```vba
Dim variableModuleAs Long  ' Disponible dans tout le module
' Généralement déclarée en entête de module, au-dessus des procédures

Sub MaProcedure()
    variableModule = 10  ' Accessible depuis n'importe quelle procédure du module
End Sub
```

### Niveau public (disponible partout)
```vba
Public variableGlobale As String  ' Accessible depuis n'importe quel module
```

## Tableaux

### Déclaration
```vba
Dim tableau(10) As Integer         ' Tableau à une dimension, indices 0 à 10
Dim tableau2D(5, 3) As String      ' Tableau à deux dimensions
Dim tableauDyn() As Double         ' Tableau dynamique (taille modifiable)
```

### Utilisation de Option Base
```vba
Option Base 1  ' Défient l'indice de départ des tableaux à 1 (au lieu de 0 par défaut)
```

### Redimensionnement des tableaux dynamiques
```vba
Represerve tableauDyn(1 to 100)  ' Préserve les valeurs existantes
Represerve tableauDyn(1 to 200)  ' Agrandit le tableau (préserve les valeurs)
Represerve tableauDyn(1 to 50)   ' Réduit le tableau (perd les valeurs au-delà de l'indice 50)
```

## Constantes

```vba
Const TAUX_TVA As Double = 0.20
Const NB_MAX_ESSAIS As Integer = 3
Const CHEMIN_FICHIER As String = "C:\donnees\rapport.pdf"
```

Les constantes sont immuables pendant l'exécution du programme.

---
*Points clés à retenir :*
- Toujours utiliser `Option Explicit` en entête de module
- Choisir le type de donnée le plus adapté à vos besoins
- Initialiser vos variables avant utilisation
- Utiliser des noms explicites en français ou en anglais (mais pas les deux mélangés)
- Préferer les types spécifiques aux Variant quand c'est possible

*Liens utiles :*
- [02-Environnement-et-configuration](02-Environnement-et-configuration.md)
- [04-Structure-langage-boucles-conditions](04-Structure-langage-boucles-conditions.md)
- [INDEX](INDEX.md)