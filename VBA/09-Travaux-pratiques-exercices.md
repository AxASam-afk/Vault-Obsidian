# Travaux pratiques et exercices

## Exercice 1 : Gestionnaire de dépenses personnelles

**Objectif** : Créer une petite application pour suivre ses dépenses mensuelles

### Cahier des charges
- Feuille "Paramètres" : catégories de dépenses (Alimentation, Transport, Loisirs, etc.) et budgets mensuels
- Feuille "Saisie" : formulaire pour ajouter une dépense (date, catégorie, montant, description)
- Feuille "Historique" : tableau récapitulatif de toutes les dépenses saisies
- Feuille "Rapport" : graphiques et synthèse par catégorie avec comparaison au budget

### Fonctionnalités à implémenter
1. Bouton sur la feuille Saisie pour ajouter la dépense à l'historique
2. Validation des données (date valide, catégorie existante, montant positif)
3. Mise à jour automatique du total des dépenses par catégorie
4. Alerte quand le budget d'une catégorie est dépassé
5. Génération mensuel d'un rapport de synthèse
6. Bouton de remise à zéro mensuelle (avec archivage)

### Concepts mis en pratique
- Saisie de données avec validation
- Manipulation de plages et de tableaux
- Boucles de recherche et de calcul
- Gestion d'erreurs personnalisée
- Création de graphiques VBA
- Utilisation de dates et de fonctions financières
- Structure modulaire avec procédures spécialisées

### Étapes suggérées
1. Créer la structure des feuilles
2. Développer le module de saisie et validation
3. Implémenter la mise à jour de l'historique
4. Ajouter les calculs de totaux et comparaisons budgétaires
5. Créer la fonction de génération de rapports
6. Tester avec des données factices
7. Ajouter l'interface utilisateur (boutons, éventuellement un UserForm)

## Exercice 2 : Générateur de factures

**Objectif** : Automatiser la création de factures clients à partir d'un devis ou d'une commande

### Cahier des charges
- Feuille "Produits" : catalogue des produits/services avec prix unitaires
- Feuille "Clients" : coordonnées des clients (nom, adresse, contact, etc.)
- Feuille "Commandes" : enregistrement des commandes (client, date, produits, quantités)
- Feuille "Facture" : modèle de facture à remplir et imprimer

### Fonctionnalités à implémenter
1. Sélection d'un client et d'une date pour générer une nouvelle commande
2. Ajout de lignes de commande avec recherche dans le catalogue produits
3. Calcul automatique des sous-totaux, taxes et totaux
4. Génération d'un numéro de facture séquentiel
5. Mise en forme automatique du modèle de facture
6. Option d'impression directe ou de sauvegarde en PDF
7. Archivage des factures générées

### Concepts mis en pratique
- Recherche dans des tableaux (VLOOKUP, INDEX/EQUIV en VBA)
- Gestion de données maîtres-détails
- Calculs composés et application de règles de gestion
- Génération de séquences avec persistance
- Mise en forme conditionnelle et manipulation de mise en page
- Utilisation des fonctions de date et de texte
- Intégration avec l'impression Excel ou export PDF

### Étapes suggérées
1. Créer les tables de référence (produits, clients)
2. Développer le moteur de calcul des commandes
3. Implémenter la génération du numéro de facture
4. Créer la procédure de remplissage du modèle de facture
5. Ajouter les fonctions d'impression et de sauvegarde
6. Tester le cycle complet de commande à facturation
7. Optimiser les performances avec de grands catalogues

## Exercice 3 : Outil d'analyse de données

**Objectif** : Créer un petit outil d'exploration et de nettoyage de jeux de données importés

### Cahier des charges
- Capable d'importer des fichiers CSV ou texte avec délimiteur personnalisable
- Fonctions de nettoyage : suppression doublons, correction de formats, gestion des valeurs manquantes
- Outils d'analyse de base : statistiques descriptives, répartition de fréquences, détection d'outliers
- Export des résultats vers de nouveaux fichiers ou mise à jour du classeur actif

### Fonctionnalités à implémenter
1. Interface de sélection de fichier avec aperçu des premières lignes
2. Détection automatique ou spécification manuelle du délimiteur
3. Conversion des types de données (texte vers nombre, date, etc.)
4. Identification et traitement des valeurs manquantes
5. Détection et suppression des enregistrements dupliqués
6. Filtrage basé sur des critères personnalisables
7. Calcul de moyennes, médianes, écarts-types, percentiles
8. Détection d'outliers par la méthode des écarts interquartiles ou par z-score
9. Export en plusieurs formats (CSV, texte, classeur Excel)

### Concepts mis en pratique
- Lecture et écriture de fichiers séquentiels
- Parsing de chaînes et manipulation de types
- Algorithmes de déduplication et de filtrage
- Statistiques descriptives de base
- Gestion dynamique de tableaux en mémoire
- Création d'interfaces utilisateur simples
- Export et import de données dans différents formats
- Gestion d'erreurs robuste pour les entrées utilisateur imprévisibles

### Étapes suggérées
1. Créer la fonction d'import de base avec sélection de fichier
2. Implémenter la détection de délimiteur et le parsage des lignes
3. Développer les fonctions de conversion de types
4. Ajouter les outils de nettoyage (doublons, valeurs manquantes)
5. Créer les fonctions d'analyse statistique de base
6. Implémenter les méthodes de détection d'outliers
7. Développer l'interface de prévisualisation et de filtrage
8. Ajouter les options d'export dans différents formats
9. Tester avec divers types de fichiers réels

## Exercices rapides pour renforcer les acquis

### Manipulation de chaînes
1. Écrire une fonction qui inverse une chaîne de caractères
2. Créer une procédure qui met en majuscule la première lettre de chaque mot d'une phrase
3. Développer une fonction qui extrait toutes les lignes d'une chaîne multiligne contenant un mot spécifique

### Tableaux et collections
1. Créer une procédure qui élimine les doublons d'un tableau à une dimension
2. Développer une fonction qui fusionne deux tableaux triés en préservant l'ordre
3. Écrire une procédure qui transforme une plage Excel en tableau de objets personnalisés

### Dates et temps
1. Créer une fonction qui retourne le dernier jour ouvré d'un mois donné
2. Développer une procédure qui génère un planning hebdomadaire à partir d'une liste de tâches
3. Écrire une fonction qui calcule l'âge précis à partir d'une date de naissance

### Fichiers et dossiers
1. Créer une procédure qui liste tous les classeurs Excel dans un dossier et ses sous-dossiers
2. Développer une fonction qui vérifie si un fichier est verrouillé par une autre application
3. Écrire une procédure qui compresse un dossier en ZIP en utilisant l'objet Shell

### API Windows
1. Créer une fonction qui récupère le nom de l'utilisateur Windows actuel
2. Développer une procédure qui verrouille la session de travail
3. Écrire une fonction qui retourne la résolution d'écran principale

## Conseils pour la réalisation des exercices

### Méthodologie
1. **Commencer petit** : Implémenter une fonctionnalité de base avant d'ajouter les refinements
2. **Tester régulièrement** : Vérifier chaque étape avec des jeux de données simples
3. **Documenter en cours** : Commenter ses décisions et difficultés rencontrées
4. **Réviser son code** : Après chaque fonctionnalité, relire et améliorer ce qui a été fait
5. **Demander du feedback** : Faire tester son travail par quelqu'un d'autre si possible

### Gestion de la complexité
- **Diviser pour régler** : Séparer les préoccupations (interface, logique, stockage)
- **Réutiliser** : Créer des fonctions utilitaires plutôt que dupliquer du code
- **Abstraire** : Masquer les détails d'implémentation derrière des interfaces simples
- **Itérer** : Améliorer progressivement plutôt que viser la perfection immédiate

### Qualité du code
- **Nommage explicite** : Pas de noms comme `x`, `temp`, `data2` sauf pour des boucles très locales
- **Fonctionsunitaires** : Chaque procédure devrait avoir une seule responsabilité clairement définie
- **Gestion des erreurs** : Prévoir les cas d'erreur raisonnables et y répondre de manière informative
- **Performance** : Éviter les boucles imbriquées inutiles, privilégier les opérations sur tableaux quand possible
- **Lisibilité** : Indentation cohérente, longueur de ligne raisonnable, utilisation d'espaces pour aérer

### Développement durable
- **Réversibilité** : Pouvoir revenir en arrière facilement (versions, commentaires)
- **Extensibilité** : Facile à ajouter de nouvelles fonctionnalités sans réécrire l'existant
- **Maintenabilité** : Un autre développeur devrait pouvoir comprendre et modifier le code
- **Portabilité** : Fonctionner sur différentes versions d'Excel et Windows quand possible
- **Éthique** : Respecter la confidentialité des données et ne pas créer d'outils malveillants

## Ressources pour aller plus loin

### Livres de référence
- "Excel Power Programming with VBA" par John Walkenbach
- "Professional Excel Development" par Stephen Bullen, Rob Bovey, John Green
- "VBA and Macros for Microsoft Excel" par Bill Jelen & Tracy Syrstad
- "Learning VBA in Excel" par Mark Gardner et Rod Stephens

### Ressources en ligne
- Microsoft Docs : https://docs.microsoft.com/office/vba/api/overview/
- Stack Overflow : Rechercher les balises [excel-vba] et [vba]
- Chandoo.org : Excellents tutoriels pratiques
- MrExcel.com : Forum actif et bibliothèque de macros
- Excel-Easy.com : Tutoriels clairs pour débutants et intermédiaires

### Communautés
- Groupe LinkedIn : "VBA Developers"
- Subreddit : r/excel
- Forums Microsoft Tech Community
- Communauté Developpez.com section Office & VBA

### Outils complémentaires
- Rubberduck VBA : Outil open source pour l'analyse, le refactoring et les tests unitaires de code VBA
- MZ-Tools : Complément payant avec de nombreuses fonctionnalités de productivité
- AutoMacro : Générateur de code VBA basé sur des modèles

---
*Points clés à retenir :*
- Commencer par des exercices simples pour assimiler les concepts de base
- Progresser progressivement vers des projets plus intégrés
- Toujours penser à la maintenabilité et à la lisibilité du code
- Documenter non seulement le *comment* mais aussi le *pourquoi*
- Partager son code et demander des retours pour améliorer sa pratique
- Penser à la réutilisabilité : vos fonctions d'aujourd'hui peuvent devenir votre bibliothèque de demain
- L'objectif n'est pas de connaître toutes les fonctions VBA par cœur, mais de savoir où chercher l'information et comment l'appliquer à un problème donné

*Liens utiles :*
- [08-Automatisation-other-apps](08-Automatisation-other-apps.md)
- [INDEX](INDEX.md)