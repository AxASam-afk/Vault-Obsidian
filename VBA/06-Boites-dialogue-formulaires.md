# Boîtes de dialogue et formulaires

## Boîtes de dialogue prédéfinies

### MsgBox - Boîte de message simple
```vba
resultat = MsgBox("Texte du message", [boutons], [titre])

' Boutons disponibles (constantes) :
vbOKOnly                       ' OK seulement
vbOKCancel                     ' OK et Annuler
vbAbortRetryIgnore             ' Abandonner, Réessayer, Ignorer
vbYesNoCancel                  ' Oui, Non, Annuler
vbYesNo                        ' Oui et Non
vbRetryCancel                  ' Réessayer et Annuler

' Icônes disponibles :
vbCritical                     ' Icône d'arrêt critique
vbQuestion                     ' Icône de question
vbExclamation                  ' Icône d'avertissement
vbInformation                  ' Icône d'information

' Valeur de retour (constantes) :
vbOK                           ' 1
vbCancel                       ' 2
vbAbort                        ' 3
vbRetry                        ' 4
vbIgnore                       ' 5
vbYes                          ' 6
vbNo                           ' 7

' Exemple d'utilisation :
reponse = MsgBox("Voulez-vous vraiment supprimer cet élément ?", _
                 vbYesNo + vbQuestion, "Confirmation")
If reponse = vbYes Then
    ' Action de suppression
End If
```

### InputBox - Boîte de saisie simple
```vba
reponse = InputBox("Texte d'invite", [titre], [défaut], [xpos], [ypos])

' Exemple :
nomUtilisateur = InputBox("Entrez votre nom :", "Identification", _
                         "Nom Prénom")
If nomUtilisateur <> "" Then
    Feuille1.Range("A1").Value = nomUtilisateur
End If
```

## Formulaires utilisateurs (UserForm)

### Création d'un UserForm
1. Dans l'éditeur VBA : Insertion → UserForm
2. Utiliser la boîte à outils pour ajouter des contrôles
3. Définir les propriétés via la fenêtre Propriétés (F4)
4. Écrire le code associé dans la fenêtre code du UserForm

### Contrôles courants disponibles

#### Contrôles de saisie
- **TextBox** : Saisie de texte simple ou multiligne
- **ComboBox** : Liste déroulante avec possibilité de saisie
- **ListBox** : Liste de sélection simple ou multiple
- **ToggleButton** : Bouton bascule (activé/désactivé)
- **OptionButton** : Bouton radio (choix exclusif dans un groupe)
- **CheckBox** : Case à cocher (oui/non indépendants)

#### Contrôles de commande
- **CommandButton** : Bouton d'action classique
- **ToggleButton** : Bouton pouvant avoir deux états

#### Contrôles d'affichage
- **Label** : Texte d'information non modifiable
- **Image** : Affichage d'image
- **Frame** : Cadre pour regrouper visuellement des contrôles
- **Multipage** ou **TabStrip** : Organisation en onglets

#### Contrôles spéciaux
- **SpinButton** : Boutons flèches pour incrémenter/décrémenter
- **ScrollBar** : Barre de défilement horizontale ou verticale
- **RefEdit** : Sélection de plage Excel (retourne l'adresse comme texte)

### Événements courants des UserForms et contrôles

#### Au niveau du UserForm
- `UserForm_Initialize()` : Lors du chargement du formulaire (avant affichage)
- `UserForm_Activate()` : Lorsqu'il devient actif
- `UserForm_Deactivate()` : Lorsqu'il perd le focus
- `UserForm_QueryClose(Cancel As Integer, CloseMode As Integer)` : Avant fermeture
- `UserForm_Terminate()` : Juste avant destruction en mémoire

#### Au niveau des contrôles (exemples)
- `TextBox_Change()` : Lorsque le contenu change
- `TextBox_KeyDown(KeyAscii As Integer, Shift As Integer)` : Touche pressée
- `CommandButton_Click()` : Lorsqu'on clique sur le bouton
- `ComboBox_Change()` : Lorsque la sélection change
- `ComboBox_DropButtonClick()` : Lorsque le bouton déroulant est cliqué
- `ScrollBar_Change()` : Lorsque la position du curseur change

### Exemple complet de UserForm de connexion

**Fenêtre propriétés du UserForm :**
- Name : `frmConnexion`
- Caption : `Connexion requise`
- StartUpPosition : `1 - CenterOwner`

**Contrôles à ajouter :**
- 2 Label : "Identifiant :" et "Mot de passe :"
- 2 TextBox : `txtIdentifiant` (PasswordChar = ""), `txtMotDePasse` (PasswordChar = "*")
- 1 CommandButton : `cmdOK` (Caption = "OK", Default = True)
- 1 CommandButton : `cmdAnnuler` (Caption = "Annuler", Cancel = True)

**Code du UserForm :**
```vba
Private Sub UserForm_Initialize()
    txtIdentifiant.SetFocus
    txtIdentifiant.Text = ""
    txtMotDePasse.Text = ""
End Sub

Private Sub cmdOK_Click()
    If ValiderIdentifiants(txtIdentifiant.Text, txtMotDePasse.Text) Then
        Me.Hide  ' Cache plutôt que décharge pour garder les valeurs si besoin
    Else
        MsgBox "Identifiant ou mot de passe incorrect.", _
               vbExclamation, "Erreur d'authentification"
        txtIdentifiant.SetFocus
        txtIdentifiant.SelectAll
    End If
End Sub

Private Sub cmdAnnuler_Click()
    Unload Me  ' Déscharge complètement le formulaire de la mémoire
End Sub

Private Function ValiderIdentifiants(id As String, pwd As String) As Boolean
    ' Ici, votre logique de validation (base de données, feuille Excel, etc.)
    ValiderIdentifiants = (id = "admin" And pwd = "secret123")
End Function
```

**Pour appeler le UserForm depuis une procédure :**
```vba
Sub LancerConnexion()
    Dim frm As New frmConnexion
    frm.Show  ' Modal par défaut (bloque jusqu'à fermeture)
    ' Ou : frm.Show vbModeless pour non-modal
    
    ' Après fermeture, les valeurs sont accessibles si le formulaire est seulement caché
    If Not frm Is Nothing Then
        If frm.txtIdentifiant.Text <> "" Then
            ' Utiliser les identifiants...
        End If
    End If
End Sub
```

## Bonnes pratiques pour les formulaires

1. **Limiter le nombre de contrôles** : Un formulaire surchargé est difficile à utiliser
2. **Grouper logiquement** : Utiliser des Frame pour regrouper des contrôles liés
3. **Ordre de tabulation logique** : Définir l'ordre avec la touche Tab dans l'éditeur
4. **Touches d'accès rapides** : Utiliser & dans les Caption pour Alt+lettre (ex: &OK)
5. **Validation en temps réel** : Utiliser les événements Change pour valider pendant la saisie
6. **Fournir du feedback** : Changer la couleur d'arrière-plan ou afficher des messages d'aide
7. **Gérer l'échappement** : Toujours prévoir un moyen d'annuler ou de fermer proprement
8. **Libérer la mémoire** : Utiliser Unload quand le formulaire n'est plus nécessaire, ou bien le cacher si vous devez conserver ses valeurs

---
*Points clés à retenir :*
- Commencer toujours par des boîtes MsgBox et InputBox simples avant de passer aux UserForms
- Nommer vos contrôles avec des préfixes explicites (txtNom, cmdValider, lstChoix)
- Utiliser les propriétés TabIndex et TabStop pour contrôler la navigation au clavier
- Prévoir systématiquement une gestion d'erreur dans les événements des contrôles
- Tester vos formulaires avec différentes tailles d'écran et résolutions
- Documenter l'objectif et l'utilisation de chaque UserForm dans ses commentaires d'entête

*Liens utiles :*
- [05-Procedures-fonctions](05-Procedures-fonctions.md)
- [07-Evenements-VBA](07-Evenements-VBA.md)
- [INDEX](INDEX.md)