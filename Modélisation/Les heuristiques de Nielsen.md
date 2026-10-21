

## tags: [ihm, ergonomie, ux, ui, cours]

# Étape 2 — Spécifier les exigences utilisateur

## Définitions

> [!important] Définition — Ergonomie Discipline qui vise à adapter les systèmes, outils et interfaces aux capacités, besoins et limites physiques et cognitives de l'utilisateur, afin de rendre leur usage efficace, sûr et confortable.

> [!important] Définition — UI Design (User Interface Design) Conception de l'interface visuelle et interactive d'un produit : l'agencement des éléments graphiques (boutons, couleurs, typographie, icônes, mise en page) avec lesquels l'utilisateur interagit directement.

> [!important] Définition — UX Design (User Experience Design) Conception de l'expérience globale vécue par l'utilisateur lors de l'usage d'un produit ou service. Englobe l'utilité, la facilité d'utilisation, l'accessibilité et la satisfaction ressentie — au-delà de la seule interface visuelle.

---

## Les heuristiques de Nielsen

Les 10 heuristiques d'utilisabilité de Jakob Nielsen sont des règles générales d'évaluation de l'ergonomie d'une interface, utilisées pour identifier les problèmes d'utilisabilité lors d'un audit ou d'une conception.

### Heuristique 1 — Visibilité de l'état du système

Le système doit toujours tenir l'utilisateur informé de ce qui se passe, grâce à un retour d'information approprié et dans un délai raisonnable.

- Barres de progression et indicateurs de chargement
- Message de confirmation après une action ("Message envoyé")
- Indicateur visuel pendant un traitement en cours

### Heuristique 2 — Correspondance entre le système et le monde réel

Le système doit parler le langage de l'utilisateur, avec des mots, phrases et concepts qui lui sont familiers, plutôt que du jargon technique, et respecter les conventions du monde réel.

- Icône de corbeille pour la suppression
- Ordre logique des informations, comme dans la vie réelle
- Utiliser "Panier" plutôt qu'un terme technique comme "Buffer d'articles"

### Heuristique 3 — Contrôle et liberté de l'utilisateur

Les utilisateurs font parfois des choix par erreur : le système doit offrir une "sortie de secours" clairement identifiée pour quitter une action indésirable sans avoir à suivre une procédure longue.

- Bouton "Annuler" ou fonction "Précédent"
- Possibilité d'annuler une suppression (Undo)
- Fermeture d'une fenêtre ou d'un formulaire sans validation forcée

### Heuristique 4 — Cohérence et standards

Les utilisateurs ne doivent pas avoir à se demander si des mots, des situations ou des actions différentes signifient la même chose. Le système doit respecter les conventions de sa plateforme.

- Mêmes couleurs et icônes pour les mêmes actions dans toute l'application
- Bouton "Valider" toujours placé au même endroit
- Respect des standards du système d'exploitation ou du framework utilisé

### Heuristique 5 — Prévention des erreurs

Il est préférable de concevoir un système qui anticipe et évite les erreurs plutôt que de se contenter d'afficher, après coup, des messages d'erreur bien conçus.

- Désactiver ou masquer les fonctionnalités incompatibles avec les actions ou choix précédents de l'utilisateur
- Anticiper les fautes courantes : proposer des requêtes prédéfinies, valider le format des données saisies en temps réel (ex : numéro de téléphone)
- Demander une confirmation avant une action critique ou irréversible (suppression de compte, paiement d'une facture)
- Structurer l'information de manière logique (ex : formatage de date) pour faciliter la détection visuelle des erreurs de frappe

### Heuristique 6 — Reconnaissance plutôt que mémorisation

Le système doit rendre les objets, actions et options visibles, afin de minimiser la charge cognitive de l'utilisateur, plutôt que de lui demander de se souvenir d'informations d'une étape à l'autre. La reconnaissance visuelle est un processus plus naturel et moins coûteux en effort mental que la mémorisation.

- Menus déroulants : afficher les options pour choisir sans avoir à se souvenir du libellé exact
- Historique et récapitulatifs : afficher le panier ou les étapes précédentes d'un processus de commande
- Icônes et labels : utiliser des symboles familiers et des textes descriptifs plutôt que du jargon technique ou des acronymes obscurs

> [!important] À retenir Contrairement à la mémoire, limitée et fatigante, la reconnaissance permet à l'utilisateur de retrouver ses repères rapidement et de progresser sans frustration.

### Heuristique 7 — Flexibilité et efficacité d'utilisation

Le système doit s'adapter aussi bien aux utilisateurs novices qu'aux utilisateurs experts, en proposant des raccourcis ou fonctionnalités avancées invisibles pour les débutants mais accessibles aux utilisateurs expérimentés.

- Raccourcis clavier
- Personnalisation de l'interface (favoris, préréglages)
- Actions rapides destinées aux utilisateurs réguliers

### Heuristique 8 — Esthétique et design minimaliste

Les interfaces ne doivent pas contenir d'informations superflues ou rarement utiles : chaque élément additionnel dilue la visibilité des éléments réellement importants.

- Éviter la surcharge visuelle
- Hiérarchiser l'information (titres, priorités visuelles)
- Ne montrer que l'essentiel par défaut, avec les options avancées repliées

### Heuristique 9 — Aide à la reconnaissance, au diagnostic et à la récupération des erreurs

Les messages d'erreur doivent être exprimés dans un langage clair (sans code technique), indiquer précisément le problème et proposer une solution concrète.

- Message explicite ("Le mot de passe doit contenir au moins 8 caractères") plutôt qu'un code d'erreur ("Erreur 403")
- Mise en évidence visuelle du champ concerné par l'erreur
- Proposition d'une action corrective directement dans le message

### Heuristique 10 — Aide et documentation

Idéalement, un système doit pouvoir s'utiliser sans documentation. Quand une aide est nécessaire, elle doit être facilement accessible, centrée sur la tâche de l'utilisateur, et présenter des étapes concrètes à suivre.

- FAQ et tooltips contextuels
- Tutoriels d'onboarding
- Documentation consultable en cas de besoin, organisée par tâche plutôt que par fonctionnalité

---

## Fiche mémo

- Ergonomie = adapter le système à l'utilisateur · UI = l'interface visuelle · UX = l'expérience globale
- Les 10 heuristiques de Nielsen servent de grille d'évaluation de l'utilisabilité d'une interface
- Prévention des erreurs (H5) : anticiper plutôt que corriger
- Reconnaissance plutôt que mémorisation (H6) : rendre visible plutôt que faire retenir


Liens:
	[IHM](IHM.md)
	