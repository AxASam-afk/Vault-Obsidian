# Programmation événementielle en VBA

## Concept d'événement

Un événement est une action reconnue par l'application qui peut déclencher l'exécution automatique du code. En VBA Excel, de nombreux objets génèrent des événements auxquels on peut répondre en écrivant des procédures spéciales appelées procédures événementielles.

## Niveaux d'événements en Excel

### Au niveau de l'application
Événements globaux affectant tous les classeurs ouverts
- Accessible via l'objet `Application`
- Nécessite un module de classe pour être capturés

### Au niveau du classeur
Événements spécifiques à un classeur particulier
- Stockés dans le module `CetteClasseur` du classeur concerné
- Disponibles immédiatement sans configuration spéciale

### Au niveau de la feuille de travail
Événements spécifiques à une feuille particulière
- Stockés dans le module de la feuille concernée (Feuil1, Feuil2, etc.)
- Disponibles immédiatement sans configuration spéciale

### Au niveau des UserForms et contrôles
Événements liés aux interfaces personnalisées
- Stockés dans le module du UserForm concerné
- Disponibles immédiatement

### Au niveau des graphiques
Événements liés aux objets graphiques
- Disponibles pour les graphiques intégrés et les feuilles de graphiques

## Événements de classeur courants (dans le module CetteClasseur)

### Ouverture et fermeture
```vba
Private Sub Workbook_Open()
    ' Exécuté lors de l'ouverture du classeur
    ' Idéal pour : initialiser des variables, vérifier des versions, afficher un accueil
End Sub

Private Sub Workbook_BeforeClose(Cancel As Boolean)
    ' Exécuté juste avant la fermeture
    ' Cancel = True pour annuler la fermeture
    ' Idéal pour : sauvegarder automatiquement, poser des questions de confirmation
End Sub

Private Sub Workbook_BeforeSave(ByVal SaveAsUI As Boolean, Cancel As Boolean)
    ' Exécuté avant chaque sauvegarde
    ' Idéal pour : mettre à jour un journal, vérifier la complétude
End Sub
```

### Activation et désactivation des feuilles
```vba
Private Sub Workbook_SheetActivate(ByVal Sh As Object)
    ' Exécuté lorsqu'une feuille quelconque est activée
    ' Sh représente la feuille activée
End Sub

Private Sub Workbook_SheetDeactivate(ByVal Sh As Object)
    ' Exécuté lorsqu'une feuille quelconque est désactivée
End Sub
```

### Modifications de cellules
```vba
Private Sub Workbook_SheetChange(ByVal Sh As Object, ByVal Target As Range)
    ' Exécuté lorsqu'une ou plusieurs cellules sont modifiées
    ' Sh = feuille modifiée
    ' Target = plage modifiée (peut être multi-cellules)
    
    ' Exemple : suivi des modifications
    If Not Intersect(Target, Sh.Range("A:A")) Is Nothing Then
        ' Une cellule de la colonne A a été modifiée
        FeuilleJournal.Cells(FeuilleJournal.Rows.Count, 1).End(xlUp).Offset(1, 0).Value = _
            Now & " - " & Sh.Name & "!" & Target.Address & " modifié par " & Environ("USERNAME")
    End If
End Sub

Private Sub Workbook_SheetSelectionChange(ByVal Sh As Object, ByVal Target As Range)
    ' Exécuté lorsque la sélection change sur une feuille
    ' Utile pour : afficher des informations contextuelles, mettre à jour une barre d'état personnalisée
End Sub
```

### Événements d'impression
```vba
Private Sub Workbook_BeforePrint(Cancel As Boolean)
    ' Exécuté avant l'impression
    ' Idéal pour : ajuster mise en page, ajouter entête/pied de page dynamique
End Sub
```

## Événements de feuille de travail courants (dans le module de la feuille)

### Modifications de cellules
```vba
Private Sub Worksheet_Change(ByVal Target As Range)
    ' Même principe que Workbook_SheetChange mais limité à cette feuille
End Sub

Private Sub Worksheet_SelectionChange(ByVal Target As Range)
    ' Même principe que Workbook_SheetSelectionChange mais limité à cette feuille
End Sub
```

### Événements de tableau croisé dynamique
```vba
Private Sub Worksheet_PivotTableUpdate(ByVal Target As PivotTable)
    ' Exécuté lorsqu'un TCD de cette feuille est mis à jour
End Sub

Private Sub Worksheet_PivotTableChangeSelection(ByVal Target As PivotTable)
    ' Exécuté lorsque la sélection dans un TCD change
End Sub
```

## Événements des graphiques

### Sur une feuille de travail (graphiques intégrés)
```vba
Private Sub Chart_SeriesChange(ByVal Series As Object)
    ' Exécuté lorsqu'une série du graphique est modifiée
End Sub

Private Sub Chart_MouseDown(ByVal Button As Long, ByVal Shift As Long, _
                           ByVal x As Long, ByVal y As Long)
    ' Exécuté lors d'un clic souris sur le graphique
End Sub
```

### Sur une feuille de graphiques dédiée
Même principe mais dans le module de la feuille de graphiques.

## Événements des contrôles ActiveX sur les feuilles

Ces événements sont disponibles lorsque vous insérez des contrôles ActiveX directement sur une feuille (pas dans un UserForm) :
```vba
Private Sub CommandButton1_Click()
    ' Bouton cliqué
End Sub

Private Sub ScrollBar1_Change()
    ' Position du curseur modifiée
End Sub

Private Sub SpinButton1_SpinUp()
    ' Flèche du haut cliquée
End Sub

Private Sub SpinButton1_SpinDown()
    ' Flèche du bas cliquée
End Sub
```

## Utilisation de WithEvents pour les objets personnalisés

Pour capturer les événements d'objets que vous créez vous-même (comme des instances de classe personnalisée) :
1. Déclarer la variable avec `WithEvents` en entête de module
2. Initialiser l'objet (généralement dans `Workbook_Open` ou `UserForm_Initialize`)
3. Écrire les procédures événementielles qui apparaissent dans la liste déroulante

```vba
' En entête de module
Private WithEvents maCollection As Collection

' Dans Workbook_Open ou équivalent
Private Sub Workbook_Open()
    Set maCollection = New Collection
    maCollection.Add "Premier élément"
    maCollection.Add "Deuxième élément"
End Sub

' Événements disponibles après déclaration WithEvents
Private Sub maCollection_Add(ByVal Item As String, ByVal Key As String, _
                            ByVal Before As Variant, ByVal After As Variant)
    ' Code exécuté lorsqu'un élément est ajouté à maCollection
End Sub

Private Sub maCollection_Remove(ByVal Index As Variant)
    ' Code exécuté lorsqu'un élément est supprimé
End Sub
```

## Bonnes pratiques pour la programmation événementielle

1. **Garder les procédures courtes** : Déléguer le travail lourd à des procédures standards
2. **Éviter les boucles imbriquées d'événements** : Une procédure événementielle qui déclenche le même événement peut causer une boucle infinie
3. **Utiliser EnableEvents avec précaution** :
   ```vba
   Application.EnableEvents = False
   ' Opérations qui pourraient déclencher des événements indésirables
   Application.EnableEvents = True
   ```
   Toujours rétablir à True même en cas d'erreur (utiliser une gestion d'erreur appropriée)
4. **Documenter clairement** : Commenter quand et pourquoi vous utilisez un événement spécifique
5. **Tester en conditions réelles** : Certains événements se comportent différemment selon le contexte (mode protégé, classeur partagé, etc.)
6. **Limiter la portée** : Préférer les événements de feuille ou de classeur quand possible plutôt qu'au niveau application
7. **Nettoyer les références** : En particulier pour les objetsWithEvents, penser à mettre à Nothing quand fini

---
*Points clés à retenir :*
- Les événements de classeur (CetteClasseur) sont idéaux pour l'initialisation et le nettoyage global
- Les événements de feuille sont parfaits pour réagir à des modifications spécifiques de données
- Utiliser WithEvents permet de créer des objets personnalisés réactifs
- Toujours considérer l'impact sur les performances : certains événements se déclenchent très fréquemment
- Prévoir des conditions de sortie pour éviter les boucles d'événements inadvertées
- Documenter clairement le but de chaque procédure événementielle

*Liens utiles :*
- [06-Boites-dialogue-formulaires](06-Boites-dialogue-formulaires.md)
- [08-Automatisation-other-apps](08-Automatisation-other-apps.md)
- [INDEX](INDEX.md)