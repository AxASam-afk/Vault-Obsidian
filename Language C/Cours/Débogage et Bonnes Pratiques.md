---
tags: [algorithmique, C, ESGI, débogage, gdb, valgrind, bonnes-pratiques]
---

# Débogage et Bonnes Pratiques

Fait suite à : [Récursivité](R%C3%A9cursivit%C3%A9.md) · [Chaîne de compilation](Cha%C3%AEne%20de%20compilation.md) · Suite : [Exercices Cours 3-6](Exercices%20Cours%203-6.md)

## Le comportement indéfini (UB)

> [!important] Définition
> En C, certaines erreurs (débordement de tableau, déréférencement de `NULL`, dépassement d'entier signé, variable non initialisée…) provoquent un **comportement indéfini** : le standard n'impose **rien**. Le programme peut planter, donner un résultat faux, ou **sembler fonctionner** puis échouer ailleurs.

C'est pourquoi un programme qui « marche chez moi » peut échouer ailleurs. On se protège avec les warnings, les outils ci-dessous et une écriture défensive.

## Les erreurs classiques (checklist)

| Erreur | Exemple | Symptôme |
|---|---|---|
| `=` au lieu de `==` | `if (a = 5)` | condition toujours vraie |
| `;` en trop | `if (x > 0);` · `for (...);` | le bloc suivant s'exécute toujours |
| `break` oublié dans `switch` | | exécution « en cascade » |
| Débordement d'indice | `t[5]` pour `int t[5]` | corruption / plantage |
| *Off-by-one* | `i <= n` au lieu de `i < n` | idem |
| `scanf` sans `&` | `scanf("%d", n)` | plantage |
| Variable non initialisée | `int s; s += 3;` | valeur aléatoire |
| Division entière | `1 / 2` vaut 0 | résultat faux |
| Comparer des chaînes avec `==` | `if (a == "oui")` | toujours faux |
| Format `printf` incorrect | `%d` pour un `double` | affichage absurde |
| Oublier `free` / double `free` | | fuite / plantage |
| Boucle infinie | condition qui ne change jamais | blocage |

## Étape 1 : les avertissements du compilateur

```bash
gcc -Wall -Wextra -Wpedantic -std=c17 -g prog.c -o prog
```
Traiter **chaque** warning. Lire les messages **dans l'ordre** : la première erreur est souvent la cause des suivantes.

## Étape 2 : « printf debugging »

Afficher les valeurs aux endroits stratégiques (sur `stderr` pour ne pas polluer la sortie) :
```c
fprintf(stderr, "DEBUG i=%d somme=%d\n", i, somme);
```
Simple et efficace, mais à retirer ensuite (ou à entourer de `#ifdef DEBUG`, voir [Préprocesseur et Projets multi-fichiers](Pr%C3%A9processeur%20et%20Projets%20multi-fichiers.md)).

## Étape 3 : `assert`

```c
#include <assert.h>
assert(n > 0);                // arrête le programme avec un message si la condition est fausse
```
Documente et vérifie les **hypothèses** du code (compilé avec `-DNDEBUG`, les `assert` disparaissent).

## Étape 4 : le débogueur `gdb`

Compiler avec **`-g`**, puis :
```bash
gdb ./prog
```
| Commande | Rôle |
|---|---|
| `break main` / `break fichier.c:42` | point d'arrêt |
| `run` (ou `run arg1`) | lancer |
| `next` (`n`) | ligne suivante (sans entrer dans les fonctions) |
| `step` (`s`) | entrer dans la fonction |
| `print x` / `print *p` / `print t[2]@5` | afficher une variable / 5 éléments |
| `info locals` | variables locales |
| `backtrace` (`bt`) | **pile d'appels** (très utile après un crash) |
| `watch x` | s'arrêter quand `x` change |
| `continue` (`c`) / `quit` (`q`) | reprendre / quitter |

Après un `Segmentation fault`, `gdb` + `run` + `bt` indique **la ligne exacte** du plantage.

## Étape 5 : détecter les erreurs mémoire

```bash
# AddressSanitizer + UndefinedBehaviorSanitizer (gcc/clang)
gcc -g -fsanitize=address,undefined prog.c -o prog && ./prog

# Valgrind (Linux)
valgrind --leak-check=full --track-origins=yes ./prog
```
Ils repèrent : débordements, *use-after-free*, fuites, variables non initialisées. Un programme « propre » ne doit signaler **aucune** erreur.

## Écrire du code robuste

1. **Initialiser** toutes les variables et tous les pointeurs (`NULL`).
2. **Vérifier les retours** : `scanf`, `fopen`, `malloc`, `fgets`.
3. Utiliser des **tailles** (`sizeof`, constantes nommées) plutôt que des nombres magiques.
4. **Limiter** les lectures de chaînes (`fgets`, `%19s`, `snprintf`) : jamais `gets`, `strcpy` sans contrôle.
5. Marquer `const` ce qui ne doit pas changer.
6. **Fonctions courtes**, un rôle chacune, nommées par un **verbe** (`calculerMoyenne`).
7. Éviter les variables globales et les `goto`.
8. Réduire la portée des variables (`for (int i = 0; ...)`).
9. **Commenter le pourquoi**, pas le quoi ; nommer clairement (`nbEleves`, pas `n1`).
10. Tester les **cas limites** : tableau vide, un seul élément, valeur négative, 0, valeur maximale.

## Conventions de style

- Indentation cohérente (4 espaces ou tabulation), une instruction par ligne, accolades systématiques même pour une seule instruction.
- Constantes en `MAJUSCULES`, types en `MajusculesInitiales`, variables/fonctions en `camelCase` ou `snake_case` (rester cohérent).
- Un programme = des **modules** ([Préprocesseur et Projets multi-fichiers](Pr%C3%A9processeur%20et%20Projets%20multi-fichiers.md)).

## Méthode de résolution d'un bug

1. **Reproduire** le bug de façon fiable.
2. **Réduire** : trouver le plus petit programme/entrée qui le provoque.
3. Formuler une **hypothèse**, la **tester** (print, débogueur).
4. Corriger **la cause**, pas le symptôme.
5. **Retester** et ajouter un cas de test.

## Exercices

Trouve l'erreur dans chaque extrait :
```c
int t[5];
for (int i = 0; i <= 5; i++) t[i] = i;           // (a)

char *s = "abc";
s[0] = 'X';                                      // (b)

int *p = malloc(10 * sizeof(int));
p[0] = 1;                                        // (c)
```
<details><summary>Corrigés</summary>

(a) `i <= 5` écrit `t[5]` hors du tableau (indices 0 à 4). (b) Modification d'un littéral de chaîne. (c) `malloc` non testé (risque de déréférencer `NULL`) et aucun `free` (fuite).
</details>

## Voir aussi

[Chaîne de compilation](Cha%C3%AEne%20de%20compilation.md) · [Mémoire et Allocation Dynamique](M%C3%A9moire%20et%20Allocation%20Dynamique.md) · [Récursivité](R%C3%A9cursivit%C3%A9.md) · [Aide-mémoire C](../Aide-m%C3%A9moire%20C.md)
