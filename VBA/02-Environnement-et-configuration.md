# Environnement et configuration du VBA

## Accéder à l'éditeur VBA

Dans Excel :
1. Activer l'onglet Développeur : Fichier → Options → Personnaliser le ruban → Cocher "Développeur"
2. Cliquer sur l'onglet Développeur → Visual Basic
   Ou utiliser le raccourci clavier : Alt + F11

## Les fenêtres principales de l'éditeur VBA

1. **Fenetre de Projet** : Liste tous les classeurs, feuilles et modules ouverts
2. **Fenetre de Propriétés** : Affiche les propriétés de l'objet sélectionné
3. **Fenetre de Code** : Où l'on écrit et modifie le code VBA
4. **Fenetre d'Exécution** : Permet de suivre l'exécution pas à pas
5. **Fenetre Espion** : Surveille la valeur de variables spécifiques

## Configuration initiale recommandée

Dans l'éditeur VBA : Outils → Options
- Cocher "Déclaration de variable obligatoire" (force l'utilisation de `Dim`)
- Cocher "Affichage de la syntaxe automatique"
- Cocher "Vérification de la syntaxe lors de la compilation"
- Décocher "Exécution automatique des macros" (pour la sécurité)

## Premier pas : Enregistrement d'une macro

1. Onglet Développeur → Enregistrer une macro
2. Donner un nom à la macro (sans espaces, pas de chiffre en début)
3. Choisir où la stocker (Classeur actif, Nouveau classeur, ou Classeur de macros perso)
4. Effectuer les actions à automatiser
5. Cliquer sur "Arrêter l'enregistrement"

Le code généré peut être vu et modifié dans l'éditeur VBA.

---
*Points clés à retenir :*
- Toujours déclarer ses variables avec `Dim`
- Utiliser des noms explicites pour macros et variables
- Tester les macros sur des données de sauvegarde d'abord
- Commenter son code pour faciliter la maintenance

*Liens utiles :*
- [01-Introduction-au-VBA](01-Introduction-au-VBA.md)
- [03-Declarations-variables-types](03-Declarations-variables-types.md)
- [INDEX](INDEX.md)