# Automatisation avec d'autres applications

## Principes de l'automatisation inter-applications

VBA permet de contrôler d'autres applications Microsoft Office (Word, PowerPoint, Outlook, Access) et même certaines applications tierces via l'automatisation. Cela se fait en créant des instances d'objets de ces applications et en manipulant leurs propriétés et méthodes.

## Deux approches principales

### Liaison précoce (Early Binding)
- Nécessite de définir une référence vers la bibliothèque d'objets de l'application cible
- Avantages : autocomplétion, détection d'erreurs à la compilation, meilleures performances
- Inconvénients : moins portable (référence peut manquer sur d'autres machines), version spécifique

### Liaison tardive (Late Binding)
- Utilise le type générique `Object` et `CreateObject`
- Avantages : totalement portable, fonctionne avec différentes versions
- Inconvénients : pas d'autocomplétion, détection d'erreurs seulement à l'exécution, légèrement plus lent

## Exemple : Automatisation de Word

### Avec liaison précoce (référence requise : Microsoft Word xx.x Object Library)
```vba
Sub ExporterVersWordAvecReference()
    Dim appWord As Word.Application
    Dim docWord As Word.Document
    
    Try
        ' Démarrer Word (ou utiliser l'instance existante)
        On Error Resume Next
        Set appWord = GetObject(, "Word.Application")
        If appWord Is Nothing Then
            Set appWord = CreateObject("Word.Application")
        End If
        On Error GoTo 0
        
        appWord.Visible = True
        
        ' Créer un nouveau document
        Set docWord = appWord.Documents.Add
        
        ' Ajouter du contenu
        With docWord
            .Content.InsertAfter "Rapport généré depuis Excel le " & Format(Now, "dd/mm/yyyy hh:mm")
            .Content.InsertParagraphAfter
            .Content.InsertAfter "Titre du rapport"
            .Paragraphs.Last.Range.Font.Bold = True
            .Paragraphs.Last.Range.Font.Size = 16
            
            ' Insérer un tableau depuis Excel
            Dim plageExcel As Range
            Set plageExcel = ThisWorkbook.Sheets("Données").Range("A1:D10")
            plageExcel.Copy
            .Range(0).PasteSpecial DataType:=wdPasteRTF
            
            ' Sauvegarder
            .SaveAs Filename:="C:\rapports\rapport_" & Format(Now, "yyyymmdd_hhnn") & ".docx"
        End With
        
    Catch e As Exception
        MsgBox "Erreur lors de l'export vers Word : " & e.Message, vbCritical
    Finally
        ' Optionnel : laisser Word ouvert ou le fermer
        ' appWord.Quit SaveChanges:=wdDoNotSaveChanges
    End Try
End Sub
```

### Avec liaison tardive (pas de référence nécessaire)
```vba
Sub ExporterVersWordSansReference()
    Dim appWord As Object
    Dim docWord As Object
    
    Try
        ' Démarrer Word
        On Error Resume Next
        Set appWord = GetObject(, "Word.Application")
        If appWord Is Nothing Then
            Set appWord = CreateObject("Word.Application")
        End If
        On Error GoTo 0
        
        appWord.Visible = True
        
        ' Créer un nouveau document
        Set docWord = appWord.Documents.Add
        
        ' Ajouter du contenu (syntaxe légèrement différente)
        docWord.Content.InsertAfter "Rapport généré depuis Excel le " & Format(Now, "dd/mm/yyyy hh:mm")
        docWord.Content.InsertParagraphAfter
        docWord.Content.InsertAfter "Titre du rapport"
        docWord.Paragraphs.Last.Range.Font.Bold = True
        docWord.Paragraphs.Last.Range.Font.Size = 16
        
        ' Insérer un tableau depuis Excel
        Dim plageExcel As Range
        Set plageExcel = ThisWorkbook.Sheets("Données").Range("A1:D10")
        plageExcel.Copy
        docWord.Range(0).PasteSpecial DataType:=0  ' wdPasteRTF = 0
        
        ' Sauvegarder
        docWord.SaveAs "C:\rapports\rapport_" & Format(Now, "yyyymmdd_hhnn") & ".docx"
        
    Catch e As Exception
        MsgBox "Erreur lors de l'export vers Word : " & e.Message, vbCritical
    Finally
        ' Nettoyage optionnel
        ' appWord.Quit 0
    End Try
End Sub
```

## Exemple : Automatisation de PowerPoint

### Création d'une présentation à partir des données Excel
```vba
Sub CreerPresentationPowerPoint()
    Dim appPP As Object
    Dim presPP As Object
    Dim diapoPP As Object
    Dim i As Integer
    
    Try
        ' Démarrer PowerPoint
        On Error Resume Next
        Set appPP = GetObject(, "PowerPoint.Application")
        If appPP Is Nothing Then
            Set appPP = CreateObject("PowerPoint.Application")
        End If
        On Error GoTo 0
        
        appPP.Visible = True
        
        ' Créer une nouvelle présentation
        Set presPP = appPP.Presentations.Add
        
        ' Diapositive titre
        Set diapoPP = presPP.Slides.Add(1, ppLayoutTitle)
        diapoPP.Shapes.Title.TextFrame.TextRange.Text = "Rapport d'Activité"
        diapoPP.Shapes.Placeholders(2).TextFrame.TextRange.Text = _
            "Période du " & Format(DateDebut, "mmmm yyyy") & " au " & Format(DateFin, "mmmm yyyy")
        
        ' Données à présenter (exemple : un tableau de résultats mensuels)
        Dim donnees As Variant
        donnees = ThisWorkbook.Sheets("RésultatsMensuels").Range("A1:E13").Value
        
        ' Créer une diapositive par ligne de données significatives
        For i = 2 To UBound(donnees, 1)  ' En supposant que la ligne 1 contient les en-têtes
            If Not IsEmpty(donnees(i, 1)) Then  ' Si la première colonne n'est pas vide
                Set diapoPP = presPP.Slides.Add(i, ppLayoutText)
                
                ' Titre
                diapoPP.Shapes.Title.TextFrame.TextRange.Text = _
                    "Résultats pour " & donnees(i, 1)
                
                ' Contenu
                Dim contenu As String
                contenu = "Valeur principale : " & Format(donnees(i, 2), "#,##0") & vbCrLf
                contenu = contenu & "Variation : " & Format(donnees(i, 3), "+0.0%;-0.0%") & vbCrLf
                contenu = contenu & "Objectif : " & Format(donnees(i, 4), "#,##0") & vbCrLf
                contenu = contenu & "Statut : " & donnees(i, 5)
                
                diapoPP.Shapes.Placeholders(2).TextFrame.TextRange.Text = contenu
            End If
        Next i
        
        ' Diapositive de conclusion
        Set diapoPP = presPP.Slides.Add(presPP.Slides.Count + 1, ppLayoutTitle)
        diapoPP.Shapes.Title.TextFrame.TextRange.Text = "Conclusion"
        diapoPP.Shapes.Placeholders(2).TextFrame.TextRange.Text = _
            "Les objectifs ont été atteints à " & Format(TauxReussite, "0.0%") & "."
            
        ' Sauvegarder la présentation
        presPP.SaveAs "C:\presentations\presentation_" & Format(Now, "yyyymmdd_hhnn") & ".pptx"
        
    Catch e As Exception
        MsgBox "Erreur lors de la création de la présentation PowerPoint : " & e.Message, vbCritical
    Finally
        ' Laisser PowerPoint ouvert pour permettre à l'utilisateur de travailler dessus
        ' Optionnel : appPP.Quit
    End Try
End Sub
```

## Exemple : Automatisation d'Outlook

### Envoi d'un email automatique
```vba
Sub EnvoyerRapportParEmail()
    Dim appOutlook As Object
    Dim mail As Object
    
    Try
        ' Démarrer Outlook
        On Error Resume Next
        Set appOutlook = GetObject(, "Outlook.Application")
        If appOutlook Is Nothing Then
            Set appOutlook = CreateObject("Outlook.Application")
        End If
        On Error GoTo 0
        
        ' Créer un nouvel email
        Set mail = appOutlook.CreateItem(0)  ' 0 = olMailItem
        
        With mail
            .To = "destinataire@entreprise.com"
            .CC = "copie@entreprise.com"
            .Subject = "Rapport automatique - " & Format(Now, "dd/mm/yyyy hh:mm")
            .Body = "Bonjour," & vbCrLf & vbCrLf & _
                    "Vous trouverez en pièce jointe le rapport généré automatiquement." & vbCrLf & vbCrLf & _
                    "Cordialement," & vbCrLf & _
                    "Le système d'automatisation"
            
            ' Ajouter une pièce jointe (exemple : le classeur actif ou un PDF généré)
            .Attachments.Add ThisWorkbook.FullName
            ' Ou : .Attachments.Add "C:\rapports\rapport_mensuel.pdf"
            
            .Send  ' ou .Display pour afficher avant envoi
        End With
        
    Catch e As Exception
        MsgBox "Erreur lors de l'envoi de l'email : " & e.Message, vbCritical
    Finally
        ' Outlook reste généralement ouvert pour continuer à travailler
    End Try
End Sub
```

## Références communes à établir (pour la liaison précoce)

Dans l'éditeur VBA : Outils → Références

### Microsoft Word
- Rechercher : "Microsoft Word xx.x Object Library"
- xx.x correspond à la version installée (ex: 16.0 pour Office 2016/2019/365)

### Microsoft Excel
- Généralement référencée par défaut
- "Microsoft Excel xx.x Object Library"

### Microsoft PowerPoint
- Rechercher : "Microsoft PowerPoint xx.x Object Library"

### Microsoft Outlook
- Rechercher : "Microsoft Outlook xx.x Object Library"

### Microsoft Access
- Rechercher : "Microsoft Access xx.x Object Library"

### Autres bibliothèques utiles
- "Microsoft ActiveX Data Objects x.x Library" (ADO pour travail avec les bases de données)
- "Microsoft XML, v6.0" (pour travail avec les fichiers XML et appels web)
- "Microsoft HTML Object Library" (pour manipulation de contenu HTML)
- "Microsoft Internet Controls" (pour contrôler Internet Explorer)

## Bonnes pratiques pour l'automatisation inter-applications

1. **Toujours vérifier l'existence de l'application** : Utiliser GetObject avant CreateObject quand c'est approprié
2. **Gérer proprement les erreurs** : L'automatisation échoue souvent si l'application cible n'est pas installée ou occupée
3. **Libérer les ressources** : Bien que le garbage collector VBA s'en occupe généralement, libérer explicitement quand c'est critique
4. **Respecter l'expérience utilisateur** : Ne pas prendre le contrôle invisible de l'application sans raison valable
5. **Faire attention aux versions** : Ce qui fonctionne avec Office 2016 peut différer avec Office 365 ou 2019
6. **Sécuriser les échanges** : Valider toujours les données reçues d'autres applications
7. **Documenter les dépendances** : Noter clairement quelles références sont nécessaires dans vos commentaires d'entête
8. **Tester sur différentes configurations** : Ce qui fonctionne sur votre machine peut échouer sur celle d'un collègue
9. **Considérer les alternatives** : Parfois, exporter/importer des fichiers (CSV, XML, JSON) est plus simple que l'automatisation directe
10. **Gérer les cas où l'utilisateur refuse l'accès** : Certaines politiques d'entreprise bloquent l'automatisation

---
*Points clés à retenir :*
- Commencer par la liaison tardive pour développer et tester, puis passer à la liaison précoce pour la production si approprié
- Toujours encapsuler votre code d'automatisation dans une gestion d'erreur robuste
- Nettoyer proprement les objets créés (quitter les applications si vous les avez lancées)
- Respecter les préférences de l'utilisateur (visibilité, focus, etc.)
- Utiliser le presse-papiers avec précaution : sauvegarder et restaurer son état précédent
- Documenter clairement quelles versions d'applications cibles sont supportées
- Prévoir des mécanismes de reprise en cas d'échec partiel
- Considérer l'écriture dans le journal d'événements Windows pour le débogage des problèmes en production

*Liens utiles :*
- [07-Evenements-VBA](07-Evenements-VBA.md)
- [09-Travaux-pratiques-exercices](09-Travaux-pratiques-exercices.md)
- [INDEX](INDEX.md)