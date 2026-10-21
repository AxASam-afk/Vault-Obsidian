# Structure du langage : Boucles et conditions

## Instructions conditionnelles

### If...Then...Else
```vba
If condition Then
    ' Instructions si condition vraie
ElseIf autreCondition Then
    ' Instructions si autreCondition vraie
Else
    ' Instructions si aucune condition vraie
End If
```

### Select Case (alternative propre à plusieurs If)
```vba
Select Case expression
    Case valeur1
        ' Instructions pour valeur1
    Case valeur2 To valeur5
        ' Instructions pour les valeurs entre 2 et 5 inclus
    Case Is > 10
        ' Instructions si expression > 10
    Case Else
        ' Instructions pour tous les autres cas
End Select
```

## Instructions de boucle

### For...Next (boucle à compteur connu)
```vba
For compteur = debut To fin [Step pas]
    ' Instructions à répéter
Next compteur

' Exemples :
For i = 1 To 10          ' De 1 à 10, pas de 1
For i = 10 To 1 Step -1  ' De 10 à 1, pas de -1 (à rebours)
For i = 0 To 20 Step 2   ' De 0 à 20, pas de 2 (0,2,4,6,...,20)
```

### Do While...Loop et Do Until...Loop
```vba
' Variante 1 : Test au début
Do While condition
    ' Instructions
Loop

Do Until condition
    ' Instructions
Loop

' Variante 2 : Test à la fin (s'exécute au moins une fois)
Do
    ' Instructions
Loop While condition

Do
    ' Instructions
Loop Until condition
```

### For Each...Next (parcours de collections)
```vba
For Each objet In collection
    ' Instructions pour chaque objet
Next objet

' Exemples courants :
For Each cellule In plageCellules
    ' Traitement de chaque cellule
Next cellule

For Each feuille In Classeur.Worksheets
    ' Traitement de chaque feuille
Next feuille

For Each graphique In Feuille.ChartObjects
    ' Traitement de chaque graphique
Next graphique
```

## Instructions de sortie de boucle

### Exit For
Quitte immédiatement la boucle For...Next en cours

### Exit Do
Quitte immédiatement la boucle Do...Loop en cours

### GoTo (à utiliser avec prudence)
Transfert inconditionnel vers une ligne étiquetée
```vba
For i = 1 To 100
    If conditionCritique Then
        GoTo SortieDeBoucle
    End If
    ' Traitement normal
Next i
SortieDeBoucle:
    ' Code après la boucle
```

## Opérateurs de comparaison et logiques

### Comparaison
- `=` : égal à
- `<>` : différent de
- `<` : plus petit que
- `>` : plus grand que
- `<=` : plus petit ou égal à
- `>=` : plus grand ou égal à
- `Like` : comparaison de chaînes avec caractères génériques (* et ?)

### Logiques
- `And` : les deux conditions doivent être vraies
- `Or` : au moins une condition doit être vraie
- `Not` : inverse la vérité d'une condition
- `Xor` : une seule condition doit être vraie (ou exclusif)

## Priorité des opérateurs
1. Opérateurs arithmétiques (^, -, *, /, \ , Mod)
2. Opérateurs de comparaison
3. Opérateurs logiques (Not, And, Or, Xor)

Utiliser des parenthèses pour modifier explicitement l'ordre d'évaluation.

---
*Points clés à retenir :*
- Préférer Select Case à plusieurs If imbriqués quand possible
- Utiliser For Each pour parcourir des collections (plages, feuilles, etc.)
- Toujours prévoir une condition de sortie pour éviter les boucles infinies
- Indenter correctement son code pour améliorer la lisibilité
- Tester les conditions limites de vos boucles

*Liens utiles :*
- [03-Declarations-variables-types](03-Declarations-variables-types.md)
- [05-Procedures-fonctions](05-Procedures-fonctions.md)
- [INDEX](INDEX.md)