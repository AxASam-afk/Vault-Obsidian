# Procédures et fonctions

## Différence entre procédure et fonction

### Procédure Sub
- Effectue une action mais ne retourne pas de valeur
- Syntaxe : `Sub NomProcedure([liste paramètres])`
- Appelée avec : `NomProcedure [liste arguments]` ou `Call NomProcedure([liste arguments])`

### Fonction Function
- Effectue une action ET retourne une valeur
- Syntaxe : `Function NomFonction([liste paramètres]) As TypeRetour`
- Appelée comme une variable : `resultat = NomFonction([liste arguments])`

## Paramètres des procédures et fonctions

### Passage par référence (défaut)
```vba
Sub ModifierValeur(ByRef nombre As Integer)
    nombre = nombre * 2  ' Modifie la variable originale
End Sub
```
L'argument passée peut être modifié dans la procédure.

### Passage par valeur
```vba
Sub NePasModifierValeur(ByVal nombre As Integer)
    nombre = nombre * 2  ' Ne modifie qu'une copie locale
End Sub
```
L'argument originale reste inchangée.

### Paramètres optionnels
```vba
Sub AfficherMessage(texte As String, Optional nombreFois As Integer = 1)
    Dim i As Integer
    For i = 1 To nombreFois
        MsgBox texte
    Next i
End Sub
```
Peut être appelé avec 1 ou 2 arguments.

### Paramètres avec valeurs par défaut nommés
```vba
Sub CreerFeuille(nom As String, Optional apres As Worksheet, _
                 Optional typeFeuille As XlSheetType = xlWorksheet)
    ' Implementation
End Sub
```
Appel : `CreerFeuille "Rapport", , xlChart` (spécifie seulement le 3ème paramètre)

## Variables statiques

```vba
Sub CompterAppels()
    Static compteur As Integer
    compteur = compteur + 1
    MsgBox "Cette procédure a été appelée " & compteur & " fois"
End Sub
```
La variable Static garde sa valeur entre les appels à la procédure.

## Fonctions VBA utiles

### Fonctions de conversion
- `CBool(expression)` : Convertit en Boolean
- `CByte(expression)` : Convertit en Byte
- `CChar(expression)` : Convertit en Char
- `CDate(expression)` : Convertit en Date
- `CDbl(expression)` : Convertit en Double
- `CDec(expression)` : Convertit en Decimal
- `CInt(expression)` : Convertit en Integer
- `CLng(expression)` : Convertit en Long
- `CSng(expression)` : Convertit en Single
- `CStr(expression)` : Convertit en String
- `CVar(expression)` : Convertit en Variant

### Fonctions de traitement de chaînes
- `Len(chaine)` : Retourne la longueur d'une chaîne
- `Left(chaine, n)` : Retourne les n premiers caractères
- `Right(chaine, n)` : Retourne les n derniers caractères
- `Mid(chaine, debut, [nb])` : Retourne une partie de la chaîne
- `Trim(chaine)` : Supprime les espaces en début et fin
- `LTrim(chaine)` : Supprime les espaces en début
- `RTrim(chaine)` : Supprime les espaces en fin
- `UCase(chaine)` : Convertit en majuscules
- `LCase(chaine)` : Convertit en minuscules
- `InStr([debut,] chaine1, chaine2)` : Recherche chaine2 dans chaine1
- `Replace(chaine, cherche, remplace[, debut[, nb[, comparaison]]])` : Remplace des occurrences

### Fonctions de traitement des dates
- `Date` : Retourne la date actuelle
- `Time` : Retourne l'heure actuelle
- `Now` : Retourne la date et heure actuelle
- `Year(date)` : Retourne l'année
- `Month(date)` : Retourne le mois (1-12)
- `Day(date)` : Retourne le jour du mois (1-31)
- `Hour(temps)` : Retourne l'heure (0-23)
- `Minute(temps)` : Retourne les minutes (0-59)
- `Second(temps)` : Retourne les secondes (0-59)
- `Weekday(date, [premierJourDeSemaine])` : Retourne le jour de la semaine
- `DateAdd(intervalle, nombre, date)` : Ajoute un intervalle à une date
- `DateDiff(intervalle, date1, date2)` : Retourne la différence entre deux dates
- `DatePart(intervalle, date)` : Retourne une partie spécifique d'une date
- `DateSerial(annee, mois, jour)` : Crée une date à partir de ses composants
- `TimeSerial(heure, minute, seconde)` : Crée une heure à partir de ses composants

### Fonctions financières
- `PX(taux, nbPeriodes, paiement, [valeurFutur, [type]])` : Valeur actuelle d'une annuité
- `VA(taux, nbPeriodes, paiement, [valeurActuel, [type]])` : Valeur future
- `VAN(taux, valeur1, valeur2, ...)` : Valeur actualisée nette
- `TRI(valeur1, valeur2, ... [, estimation])` : Taux de rendement interne

## Gestion des erreurs dans les procédures et fonctions

### Syntaxe de base
```vba
Sub ProcedureAvecGestionErreurs()
    On Error GoTo GestionErreur
    
    ' Code normal
    Exit Sub
    
GestionErreur:
    MsgBox "Erreur " & Err.Number & ": " & Err.Description, vbCritical
    ' Optionnel : enregistrement dans un fichier de log
    Resume Suivant  ' ou Resume Next ou Resume 0
End Sub
```

### Résumé des instructions de gestion d'erreurs
- `On Error GoTo ligne` : Redirige vers l'étiquette spécifiée en cas d'erreur
- `On Error Resume Next` : Continue l'exécution malgré l'erreur
- `On Error GoTo 0` : Désactive la gestion d'erreur personnalisée
- `Err.Number` : Code de l'erreur
- `Err.Description` : Description de l'erreur
- `Err.Source` : Nom du projet ou de l'application ayant généré l'erreur
- `Err.Clear()` : Réinitialise l'objet Err
- `Err.Raise numero, source, description` : Provoque une erreur délibérément

---
*Points clés à retenir :*
- Toujours déclarer explicitement le type de retour d'une fonction
- Utiliser ByVal sauf si vous avez besoin de modifier l'argument original
- Limiter le nombre de paramètres (idéalement ≤ 3) pour la lisibilité
- Prévoir toujours une gestion d'erreur dans vos procédures publiques
- Nommer vos procédures et fonctions avec un verbe d'action clair
- Documenter le but, les paramètres et la valeur de retour de chaque fonction

*Liens utiles :*
- [04-Structure-langage-boucles-conditions](04-Structure-langage-boucles-conditions.md)
- [06-Boites-dialogue-formulaires](06-Boites-dialogue-formulaires.md)
- [INDEX](INDEX.md)