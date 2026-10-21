# Présentation IHM

Platinum Ops est une application web destinée aux chasseurs de trophées PlayStation, des débutants aux joueurs expérimentés. Synchronisée avec le compte PSN, elle centralise la progression, la rareté mondiale des platines, le temps estimé et les guides, afin de décider quel jeu platiner et dans quel ordre. Le parcours principal va de la bibliothèque mondiale à la fiche d'un jeu (trophées à faire, ratables, très rares), puis à une route optimale cartographiée qui signale les points de non-retour, jusqu'à l'écran de félicitations après un platine.

Un assistant IA tactique accompagne la progression : il répond aux blocages en langage naturel et coche automatiquement les quêtes terminées. Un classement mondial permet de se comparer aux autres joueurs. Les maquettes ont été testées sur un prototype interactif Figma auprès de 5 participants (think-aloud, puis questionnaire SUS) pour mesurer l'utilisabilité et orienter la prochaine itération.

---

# Liens Figma Phase 1

https://www.figma.com/design/COdOEBj6G6MlszESmAibdG/Maquette?node-id=0-1&t=oUALQaGYZrBurEue-1

---

# Questionnaire SUS — Assistant pour chasseurs de trophées

Pour chaque affirmation, indiquez votre degré d'accord de 1 (Pas du tout d'accord) à 5 (Tout à fait d'accord).

|N°|Affirmation|Type|
|---|---|---|
|1|Je pense que j'aimerais utiliser cette application fréquemment pour suivre mes trophées.|Positif|
|2|Je trouve cette application inutilement complexe.|Négatif|
|3|Je pense que cette application est facile à utiliser.|Positif|
|4|Je pense que j'aurais besoin du soutien d'une personne technique pour pouvoir utiliser cette application.|Négatif|
|5|Je trouve que les différentes fonctions de l'application (suivi, assistant, comparaison) sont bien intégrées.|Positif|
|6|Je pense qu'il y a trop d'incohérences dans cette application.|Négatif|
|7|J'imagine que la plupart des chasseurs de trophées apprendraient à utiliser cette application très rapidement.|Positif|
|8|Je trouve cette application très lourde à utiliser.|Négatif|
|9|Je me sentais très en confiance en utilisant cette application.|Positif|
|10|J'ai eu besoin d'apprendre beaucoup de choses avant de pouvoir utiliser cette application.|Négatif|

## Calcul du score

On convertit d'abord chaque note brute, puis on additionne :

- Questions **impaires** (positives) : contribution = note − 1
- Questions **paires** (négatives) : contribution = 5 − note
- **Score SUS /100 = (somme des 10 contributions, soit 0 à 40) × 2,5**

## Saisie des réponses et scores

|Participant|Segment|Q1|Q2|Q3|Q4|Q5|Q6|Q7|Q8|Q9|Q10|Total /40|Score SUS /100|
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
|P1|[À renseigner]|3|1|5|1|5|1|5|1|5|1|38|95,0|
|P2|[À renseigner]|1|1|5|3|5|1|5|2|3|1|31|77,5|
|P3|[À renseigner]|5|2|5|1|5|1|4|2|5|2|36|90,0|
|P4|[À renseigner]|4|1|5|1|5|1|5|2|5|2|37|92,5|
|P5|[À renseigner]|4|1|5|1|5|1|5|1|4|1|38|95,0|
|**Moyenne**||3,4|1,2|5,0|1,4|5,0|1,0|4,8|1,6|4,4|1,4|**36,0**|**90,0**|

## Statistiques

|Indicateur|Valeur|
|---|---|
|Score SUS moyen|**90,0 / 100**|
|Médiane|92,5|
|Minimum / maximum|77,5 (P2) / 95,0 (P1 et P5)|
|Écart-type (échantillon)|7,3|
|Intervalle de confiance à 95 % (loi de Student, n = 5)|[81,0 ; 99,0]|
|Repères|68 = moyenne SUS ; 80,3 = seuil de la note « A » (top 10 %)|
![[sus_scores_participants.png]]
### Contribution moyenne par question (0 à 4, après conversion)

|Question|Thème|Moyenne /4|Visuel|
|---|---|---|---|
|Q1|Usage fréquent|**2,4**|████████████░░░░░░░░|
|Q2|Complexité (inversée)|3,8|███████████████████░|
|Q3|Facilité d'usage|4,0|████████████████████|
|Q4|Besoin d'aide technique (inversée)|3,6|██████████████████░░|
|Q5|Intégration des fonctions|4,0|████████████████████|
|Q6|Incohérences (inversée)|4,0|████████████████████|
|Q7|Apprentissage rapide|3,8|███████████████████░|
|Q8|Lourdeur (inversée)|3,4|█████████████████░░░|
|Q9|Confiance|3,4|█████████████████░░░|
|Q10|Prérequis (inversée)|3,6|██████████████████░░|
![[sus_moyenne_par_question 1.png]]
### Interprétation

- Avec **90,0**, l'application se situe nettement au-dessus de la moyenne SUS (68) et du seuil de la note « A » (80,3) ; même la borne basse de l'intervalle de confiance (81,0) reste au-dessus de 80,3.
- Les points les plus faibles sont **Q1 (envie d'un usage fréquent : 2,4/4)**, puis Q8 (lourdeur) et Q9 (confiance) à 3,4/4. Q1 est tiré vers le bas par P2 (note brute 1/5), qui est aussi le score le plus bas (77,5) et le seul à avoir répondu 3 à Q4 (besoin d'aide technique). **Hypothèse à vérifier** : P2 est un profil casual, ce qui rejoindrait le retour sur la surcharge d'information. La colonne « Segment » vide ne permet pas de le confirmer.
- Les 5 participants ont noté Q6 (incohérences) à 1/5 : les incohérences éventuelles n'ont donc pas pesé dans la perception globale, alors que les retours spontanés pointent des problèmes de parcours et de densité. Le SUS mesure la perception d'ensemble, il ne remplace pas l'analyse des retours qualitatifs.
- **Limites** : 5 participants, prototype Figma partiel (boutons non fonctionnels), segments non renseignés. Le score est indicatif et ne se généralise pas.

---

# Test utilisateur

**Méthode** : test sur prototype interactif Figma, échantillon réduit (5 participants), think-aloud, questionnaire SUS en fin de test.

**Métriques observées** : erreurs rencontrées, commentaires spontanés, score SUS. Time on Task, taux de réussite et nombre de clics : [À collecter].

## Retours test utilisateur

|#|Retour (spontané)|Thème|Écran concerné|
|---|---|---|---|
|1|Surcharge d'information, difficile de comprendre toutes les infos ; moins gênant pour des joueurs casual qui voudraient surtout se mesurer à leurs amis, sans viser le 100 % à tout prix|Densité de l'information|Bibliothèque, détail jeu, classement|
|2|Chemin utilisateur pas toujours clair|Parcours|Tous|
|3|Page 1 : « Accédez au dashboard » pas intuitif pour les non-initiés|Libellé|Accueil|
|4|Ajouter des pages utilisateur avec la possibilité d'ajouter des amis|Fonctionnalité sociale|Nouveau|
|5|Ajouter une catégorie « Amis » dans le classement, et une partie du dashboard pour voir le classement de ses amis|Fonctionnalité sociale|Classement, dashboard|
|6|Boutons fonctionnels manquants sur le prototype Figma|Limite du prototype|Tous|
|7|Relier la page Jeux à l'analyse avant d'arriver directement sur un guide|Parcours|Bibliothèque, détail jeu|
|8|Boutons Annuler / Retour sur chaque page : actuellement on ne peut se déplacer que via le hero|Navigation|Tous|
|9|Inverser le sens de la discussion avec l'IA : message de l'utilisateur à droite en bleu, IA à gauche en gris|Cohérence / convention|Assistant|
|10|Afficher les 3 petits points quand l'IA réfléchit, pour la visibilité de l'état du système|Retour d'état|Assistant|

---

# Retours des experts (revue heuristique)

**Méthode** : analyse des écrans de la phase 1 avec les 10 heuristiques de Nielsen, en suivant les tâches principales de l'application (choisir un jeu, repérer les trophées ratables, suivre la route, utiliser l'assistant). Cette revue est réalisée par l'équipe projet ; un avis externe (enseignant, joueur expérimenté) pourra la compléter : [À collecter].

## Points forts

- **Alerte de non-retour claire** : couleur rouge, libellé « NON-RETOUR » et temps requis affiché (visibilité de l'état du système, prévention des erreurs).
- **Trophées ratables identifiés** par la couleur et par un texte, donc lisibles sans dépendre de la couleur seule.
- **Boutons d'action adaptés au contexte** : « Analyser » pour un nouveau jeu, « Reprendre » pour un jeu en cours.
- **Durées cohérentes** sur la route : la somme des étapes correspond au temps total annoncé (1 h 09).
- **Progression toujours visible** : barre de progression, étape 2/6, trophées obtenus / total.

## Points à améliorer

|Heuristique de Nielsen|Constat|Gravité (0–4)|
|---|---|---|
|Contrôle et liberté de l'utilisateur|Pas de bouton retour ou annuler ; l'auto-cochage de l'IA n'a ni confirmation ni annulation visibles|4|
|Cohérence et standards|Bulles de chat inversées entre l'écran Route et l'écran Assistant ; l'onglet « Dashboard » reste actif sur l'Assistant|3|
|Visibilité de l'état du système|Aucun état d'erreur si la synchronisation PSN échoue ; pas d'indicateur quand l'IA répond|3|
|Cohérence des données|41/54 trophées affichés avec « 64 % complété » ; un trophée de 64 h alors que 48 h restantes sont annoncées|3|
|Design minimaliste|Beaucoup d'informations par écran, textes très petits et peu contrastés (à mesurer avec un outil)|3|
|Langage proche de l'utilisateur|Vocabulaire peu expliqué : « ratable », « non-retour », « route », « dashboard »|2|

## Recommandations prioritaires

1. Rendre l'auto-cochage confirmable et annulable, avec une indication de la source (PSN ou assistant) — effort M.
2. Ajouter un bouton retour sur chaque page et corriger l'onglet actif — effort M.
3. Corriger les chiffres incohérents entre les écrans — effort S.

## Lien avec les tests utilisateurs

Trois retours des tests confirment cette revue : l'inversion du chat, l'absence de boutons retour/annuler et le libellé « Accéder au dashboard » peu clair.

---

# Liste des changements pour la prochaine itération

## Issus des tests utilisateurs

|#|Changement|Retour(s)|Priorité|Effort|
|---|---|---|---|---|
|C1|Inverser le sens du chat (utilisateur à droite en bleu, IA à gauche en gris)|9|P1|S|
|C2|Boutons Retour / Annuler sur chaque page, onglet actif correct|8, 2|P1|M|
|C3|Reformuler le bouton d'accueil « Accéder à mon dashboard » avec un libellé compréhensible par les non-initiés|3|P1|S|
|C4|Rendre fonctionnels les boutons manquants du prototype avant le prochain test|6|P1|S/M|
|C5|Réduire la surcharge d'information (hiérarchie, infobulles, vue allégée par défaut pour les casual)|1|P2|M|
|C6|Ajouter une étape d'analyse entre la bibliothèque et le guide|7, 2|P2|M|
|C7|Ajouter l'indicateur « l'IA écrit… » (3 points)|10|P2|S|
|C8|Pages utilisateur et ajout d'amis|4|P3|L|
|C9|Catégorie « Amis » dans le classement + bloc classement des amis sur le dashboard (dépend de C8)|5|P3|M|

## Issus de la revue d'expert (à valider lors du prochain test)

|#|Changement|Pourquoi|Priorité|Effort|
|---|---|---|---|---|
|C10|Auto-cochage de l'IA confirmable et annulable ; distinguer « validé PSN » et « coché via l'assistant »|Une quête est cochée « via la conversation » sans confirmation ni annulation visible|P1|M|
|C11|Corriger les incohérences de chiffres (41/54 trophées vs 64 %, 64 h vs 48 h restantes, filtre < 150 h vs 120–160 h, 560 000 joueurs vs 2,8 M)|Perte de confiance dans les données|P1|S|
|C12|Contrôler et corriger contrastes et tailles de texte (WCAG AA)|Micro-libellés gris sur fond sombre, à mesurer|P2|M|
|C13|Concevoir les états d'erreur : échec de synchro PSN, profil privé, aucun résultat, chargement|Seul « SYNC PSN • EN LIGNE » est prévu|P2|M|

---

# Liens Figma Phase 2

https://www.figma.com/design/COdOEBj6G6MlszESmAibdG/Maquette?node-id=44-300&t=oUALQaGYZrBurEue-1