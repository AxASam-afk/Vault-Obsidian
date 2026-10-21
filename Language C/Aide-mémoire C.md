---
tags: [algorithmique, C, ESGI, aide-memoire, revision]
---

# Aide-mémoire C

Fiche de révision rapide. Détails dans les notes liées.

## Squelette
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int main(void)
{
    /* ... */
    return 0;
}
```
Compiler : `gcc -Wall -Wextra -std=c17 -g prog.c -o prog -lm` → [Chaîne de compilation](Cours/Cha%C3%AEne%20de%20compilation.md)

## Types et formats → [Types et Conversions](Cours/Types%20et%20Conversions.md) · [Entrées Sorties](Cours/Entr%C3%A9es%20Sorties.md)
| Type                 | `printf`       | `scanf`             |
| -------------------- | -------------- | ------------------- |
| `int`                | `%d`           | `%d`                |
| `unsigned`           | `%u`           | `%u`                |
| `long` / `long long` | `%ld` / `%lld` | idem                |
| `float`              | `%f`           | `%f`                |
| `double`             | `%f`           | **`%lf`**           |
| `char`               | `%c`           | `" %c"`             |
| chaîne               | `%s`           | `%19s` (pas de `&`) |
| `size_t`             | `%zu`          | `%zu`               |
| adresse              | `%p`           | —                   |
`%5d` largeur · `%-5d` à gauche · `%05d` zéros · `%.2f` 2 décimales

## Opérateurs → [Variables et Opérateurs](Cours/Variables%20et%20Op%C3%A9rateurs.md)
`+ - * / %` · `== != < > <= >=` · `&& || !` · `& | ^ ~ << >>` · `? :` · `++ -- += -=`
`=` affectation ≠ `==` comparaison · division entière si deux `int` · `(float)a / b`

## Contrôle → [Structures de Contrôle](Cours/Structures%20de%20Contr%C3%B4le.md)
```c
if (c) {...} else if (c2) {...} else {...}
switch (x) { case 1: ...; break; default: ...; }
for (int i = 0; i < n; i++) {...}
while (c) {...}        do {...} while (c);
break;  continue;
```

## Fonctions → [Fonctions](Cours/Fonctions.md)
```c
type nom(type1 a, type2 b) { ...; return v; }     // prototype : type nom(type1, type2);
void f(int *p)  { *p = 3; }                       // modification via adresse : f(&x);
```
Passage **par valeur** (copie). Tableau en paramètre = pointeur (passer la taille).

## Tableaux → [Tableaux](Cours/Tableaux.md) · [Pointeurs et Tableaux](Cours/Pointeurs%20et%20Tableaux.md)
```c
int t[5] = {0};         taille = sizeof t / sizeof t[0];
int m[3][4];            m[i][j]
memcpy(dst, src, n * sizeof *dst);
```
Indices de **0** à n−1. Pas de `t1 = t2`.

## Pointeurs → [Pointeurs](Cours/Pointeurs.md)
| | |
|---|---|
| `int *p = &x;` | p contient l'adresse de x |
| `*p` | valeur pointée |
| `p->champ` | `(*p).champ` |
| `p + 1` | élément suivant |
| `NULL` | « ne pointe sur rien » |
| `const int *p` | valeur non modifiable |

## Chaînes → [Chaînes de Caractères](Cours/Cha%C3%AEnes%20de%20Caract%C3%A8res.md)
Tableau de `char` terminé par `'\0'`.
`strlen` · `strcmp(a,b)==0` · `strcpy` / `snprintf` · `strcat` · `strstr` · `strchr`
Lecture : `fgets(s, sizeof s, stdin); s[strcspn(s, "\n")] = '\0';`

## Structures → [Structures et Types Composés](Cours/Structures%20et%20Types%20Compos%C3%A9s.md)
```c
typedef struct { char nom[30]; int age; } Personne;
Personne p = {"Awa", 19};   p.age   (pp->age si pp est un pointeur)
```

## Mémoire dynamique → [Mémoire et Allocation Dynamique](Cours/M%C3%A9moire%20et%20Allocation%20Dynamique.md)
```c
int *t = malloc(n * sizeof *t);  if (!t) {...}
int *tmp = realloc(t, m * sizeof *tmp);   if (tmp) t = tmp;
free(t); t = NULL;
```
1 `malloc` ↔ 1 `free`. Pas d'usage après `free`.

## Fichiers → [Fichiers](Exo/Fichiers.md)
```c
FILE *f = fopen("a.txt", "r");  if (!f) { perror("a.txt"); return 1; }
while (fgets(l, sizeof l, f)) {...}      fprintf(f, ...);      fclose(f);
```
Modes : `r` lecture · `w` écrase · `a` ajoute · `b` binaire.

## Algorithmes → [Tri et Recherche](Cours/Tri%20et%20Recherche.md) · [Récursivité](Cours/R%C3%A9cursivit%C3%A9.md)
| Algorithme | Complexité |
|---|---|
| Recherche séquentielle | O(n) |
| Recherche dichotomique (trié) | O(log n) |
| Tri bulles / sélection / insertion | O(n²) |
| Tri fusion / rapide / `qsort` | O(n log n) |
Récursion = cas de base + appel plus petit.

## Les 10 pièges
1. `=` au lieu de `==` · 2. `;` après `if`/`for` · 3. `break` oublié dans `switch`
4. `t[n]` hors limites · 5. `scanf` sans `&` · 6. `==` pour comparer des chaînes
7. variable/pointeur non initialisé · 8. `malloc` non testé / `free` oublié
9. division entière · 10. ignorer les warnings → [Débogage et Bonnes Pratiques](Cours/D%C3%A9bogage%20et%20Bonnes%20Pratiques.md)

## Bits → [Bits et Masques](Cours/Bits%20et%20Masques.md)
set `x |= 1u<<n` · clear `x &= ~(1u<<n)` · toggle `x ^= 1u<<n` · test `(x>>n)&1`
