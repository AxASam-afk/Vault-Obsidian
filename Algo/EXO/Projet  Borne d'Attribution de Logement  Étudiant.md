
##tags: [algorithmique, programmation, cours]

# Algorithmique — Traitement séquentiel et structures conditionnelles

## Objectifs du cours

- Comprendre ce qu'est un algorithme à déroulement séquentiel
- Savoir écrire des conditions (Si / Sinon / Sinon si) pour contrôler le déroulement d'un programme
- Savoir mettre en place des points de contrôle qui interrompent le programme en cas d'échec
- Savoir enchaîner calculs conditionnels et décision finale

---

## 1. Rappels : variables, entrées, sorties

Un algorithme manipule des **variables** (des valeurs qu'on stocke, comme l'âge ou le prénom d'un candidat), reçoit des **entrées** (ce que l'utilisateur saisit) et produit des **sorties** (ce qui est affiché).

```
Variable age : Entier
Lire age
Afficher "Vous avez saisi : ", age
```

---

## 2. Le traitement séquentiel

> [!important] Définition Un algorithme **séquentiel** exécute ses instructions **une par une, dans l'ordre**, sans revenir en arrière. Chaque étape dépend souvent de la validité de la précédente.

Dans un cahier des charges métier (comme une borne interactive), le traitement séquentiel correspond en général aux **étapes du parcours utilisateur** : identification → choix → informations complémentaires → décision finale.

---

## 3. Les structures conditionnelles

### Si... Alors... Sinon

```
Si (condition) Alors
    instructions si vraie
Sinon
    instructions si fausse
FinSi
```

### Sinon si (cas à choix multiples)

```
Si (choix = 1) Alors
    ...
SinonSi (choix = 2) Alors
    ...
SinonSi (choix = 3) Alors
    ...
Sinon
    Afficher "Choix invalide"
FinSi
```

> [!important] Définition Une condition s'appuie sur des **opérateurs de comparaison** (`=`, `≠`, `<`, `>`, `≤`, `≥`) et peut être combinée avec des **opérateurs logiques** (`ET`, `OU`, `NON`) pour exprimer un intervalle ou un cas complexe, par exemple : `age ≥ 18 ET age ≤ 28`.

---

## 4. Validation et arrêt anticipé du programme

Beaucoup de cahiers des charges imposent une règle du type : _« Si la condition n'est pas respectée, on arrête immédiatement le programme, sans poser les questions suivantes. »_

> [!important] Définition On appelle cela un **garde-fou** (ou contrôle bloquant) : une condition vérifiée tôt dans l'algorithme qui, si elle échoue, déclenche un message puis une **sortie immédiate** du programme (`Arrêter` / `return` selon le langage), pour ne pas exécuter les étapes suivantes.

```
Si NON (age ≥ 18 ET age ≤ 28) Alors
    Afficher "Désolé ", prenom, ", vous n'êtes pas éligible."
    Arrêter le programme
FinSi
```

Un algorithme avec plusieurs garde-fous ressemble à une suite de portes : on ne passe à l'étape suivante que si la porte précédente s'est ouverte.

---

## 5. Calculs conditionnels et critères de seuil

Certaines règles de gestion combinent un **calcul** (par exemple une déduction) et une **comparaison à un seuil** (par exemple un pourcentage de revenu).

Méthode générale :

1. Calculer la valeur de base
2. Appliquer une réduction ou un ajustement si une condition est vraie
3. Comparer le résultat à un seuil pour prendre la décision finale

```
Si (boursier = "oui") Alors
    deduction ← 100
Sinon
    deduction ← 0
FinSi

resteAPayer ← loyerBase - deduction

Si (resteAPayer ≤ 0.40 * revenus) Alors
    decision ← "accordé"
Sinon
    decision ← "refusé"
FinSi
```

---

## 6. Méthodologie de conception d'un algorithme séquentiel

1. **Lister les étapes métier** dans l'ordre où l'utilisateur les vit (identification, choix, informations, décision…)
2. **Repérer les garde-fous** : à quelles étapes une saisie invalide doit-elle stopper le programme ?
3. **Isoler les calculs** : quelles valeurs sont dérivées d'autres valeurs (déductions, totaux, pourcentages) ?
4. **Définir la condition de décision finale** : quel est le critère exact qui sépare un cas validé d'un cas refusé ?
5. **Lister l'affichage final** attendu (bilan récapitulatif)

---

## 7. Étude de cas — Borne d'attribution de logement étudiant

Application de la méthode ci-dessus à un projet type : une borne qui vérifie l'éligibilité d'un candidat à un logement étudiant.

**Étape 1 — Identification et garde-fou d'âge**

- Entrées : prénom, âge
- Garde-fou : éligibilité réservée aux 18–28 ans inclus → sinon, message de refus personnalisé et arrêt

**Étape 2 — Choix et garde-fou de saisie**

- Trois options de logement, chacune avec un loyer de base
- Garde-fou : si le numéro saisi ne correspond à aucune option → message d'erreur et arrêt

**Étape 3 — Collecte des données financières**

- Entrées : revenus mensuels, statut boursier (oui/non)
- Pas de garde-fou ici : ce sont de simples collectes d'information

**Étape 4 — Calculs et décision finale**

- Déduction forfaitaire si boursier
- Reste à payer = loyer de base − déduction
- Critère de solvabilité : reste à payer ≤ 40 % des revenus
- Affichage du bilan : prénom, loyer de base, déduction, reste à payer, décision

Ce découpage illustre bien la structure générale : **garde-fous en cascade** (étapes 1 et 2) suivis d'une **étape de calcul conditionnel** menant à une **décision finale** (étape 4).

---

## Fiche mémo

- Séquentiel = étapes exécutées dans l'ordre, sans retour arrière
- `Si / SinonSi / Sinon` pour brancher le programme selon une condition
- Un garde-fou = condition de validité + message + arrêt immédiat si échec
- Un calcul conditionnel se fait en 3 temps : valeur de base → ajustement conditionnel → comparaison à un seuil
- Toujours prévoir l'affichage d'un bilan clair à la fin de l'algorithme