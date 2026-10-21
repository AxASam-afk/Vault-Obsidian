---
tags: [algorithmique, C, ESGI, malloc, mémoire, tas, pile]
---

# Mémoire et Allocation Dynamique

Fait suite à : [Structures et Types Composés](Structures%20et%20Types%20Compos%C3%A9s.md) · [Pointeurs](Pointeurs.md) · Suite : [Fichiers](../Exo/Fichiers.md)

## Organisation de la mémoire d'un programme

```text
 adresses hautes
 ┌───────────────────────┐
 │ PILE (stack)          │ variables locales, paramètres, appels de fonctions
 │  ↓ croît vers le bas  │ → gérée automatiquement, taille limitée (~8 Mo)
 ├───────────────────────┤
 │      (libre)          │
 ├───────────────────────┤
 │  ↑ croît vers le haut │
 │ TAS (heap)            │ mémoire allouée avec malloc/calloc/realloc
 ├───────────────────────┤ → gérée par le programmeur (free)
 │ DONNÉES               │ variables globales et static
 ├───────────────────────┤
 │ CODE                  │ instructions du programme
 └───────────────────────┘
 adresses basses
```

| Durée de vie | Où ? | Libération |
|---|---|---|
| **Automatique** (variable locale) | pile | à la sortie du bloc/fonction |
| **Statique** (globale, `static`) | zone de données | à la fin du programme |
| **Dynamique** (`malloc`) | tas | par `free`, **à la main** |

`static` dans une fonction : la variable **conserve sa valeur** entre deux appels.
```c
int compteur(void) { static int n = 0; return ++n; }   // 1, 2, 3, ...
```

## Pourquoi l'allocation dynamique ?

La taille d'un tableau doit être **connue à la compilation** ([Tableaux](Tableaux.md)). Avec `malloc`, on choisit la taille **à l'exécution** et on garde les données au-delà de la fonction qui les crée.

## Les fonctions de `<stdlib.h>`

| Fonction | Rôle |
|---|---|
| `malloc(octets)` | alloue un bloc **non initialisé** ; renvoie `NULL` en cas d'échec |
| `calloc(n, taille)` | alloue `n` éléments **initialisés à 0** |
| `realloc(ptr, nouvelle_taille)` | agrandit/réduit un bloc (peut le déplacer) |
| `free(ptr)` | libère le bloc |

### Schéma à toujours respecter : allouer → tester → utiliser → libérer

```c
#include <stdio.h>
#include <stdlib.h>

int main(void)
{
    int n;
    printf("Combien de notes ? ");
    if (scanf("%d", &n) != 1 || n <= 0) return 1;

    int *notes = malloc(n * sizeof *notes);     // sizeof *notes = sizeof(int)
    if (notes == NULL)                          // TOUJOURS tester
    {
        fprintf(stderr, "Memoire insuffisante\n");
        return 1;
    }

    double somme = 0;
    for (int i = 0; i < n; i++)
    {
        printf("Note %d : ", i + 1);
        scanf("%d", &notes[i]);                 // un pointeur s'utilise comme un tableau
        somme += notes[i];
    }
    printf("Moyenne = %.2f\n", somme / n);

    free(notes);                                // libérer
    notes = NULL;                               // éviter un pointeur "fantôme"
    return 0;
}
```
`malloc(n * sizeof *notes)` est préférable à `malloc(n * sizeof(int))` : le code reste juste si le type change.

## `realloc` : tableau qui grandit

> [!important] Ne jamais écrire `p = realloc(p, ...)`
> Si `realloc` échoue, il renvoie `NULL` et **l'ancien bloc est perdu** (fuite). On passe par un pointeur temporaire.

```c
#include <stdio.h>
#include <stdlib.h>

int main(void)
{
    int *t = NULL;
    size_t taille = 0, capacite = 0;

    for (int v = 1; v <= 10; v++)
    {
        if (taille == capacite)                         // plein : doubler la capacité
        {
            size_t nouvelle = capacite ? capacite * 2 : 2;
            int *tmp = realloc(t, nouvelle * sizeof *tmp);
            if (tmp == NULL) { free(t); return 1; }
            t = tmp;
            capacite = nouvelle;
        }
        t[taille++] = v * v;
    }

    for (size_t i = 0; i < taille; i++) printf("%d ", t[i]);
    printf("\n(capacite finale : %zu)\n", capacite);
    free(t);
    return 0;
}
```
Doubler la capacité rend l'ajout **amorti en O(1)**.

## Matrice dynamique

Version **contiguë** (simple, rapide, une seule libération) :
```c
int lignes = 3, cols = 4;
int *m = calloc(lignes * cols, sizeof *m);
if (m == NULL) return 1;
m[i * cols + j] = 5;            // élément (i, j)
free(m);
```
Version **tableau de pointeurs** (permet `m[i][j]`) :
```c
int **m = malloc(lignes * sizeof *m);
for (int i = 0; i < lignes; i++) m[i] = calloc(cols, sizeof **m);
// ... utilisation m[i][j] ...
for (int i = 0; i < lignes; i++) free(m[i]);
free(m);
```
(Vérifier chaque allocation en code réel.)

## Les bugs mémoire (à connaître absolument)

| Bug | Exemple | Conséquence |
|---|---|---|
| **Fuite mémoire** (*leak*) | `malloc` sans `free` | la mémoire s'épuise (programmes longs) |
| **Double libération** | `free(p); free(p);` | plantage / corruption |
| **Utilisation après libération** (*use-after-free*) | `free(p); p[0] = 1;` | comportement indéfini |
| **Débordement de tampon** (*buffer overflow*) | `t[n]` sur un tableau de `n` éléments | corruption, faille de sécurité |
| **Pointeur non initialisé / NULL** | `*p` | `Segmentation fault` |
| **Mauvaise taille** | `malloc(n)` au lieu de `malloc(n * sizeof(int))` | débordement |

Règles : **un `free` pour chaque `malloc`**, mettre le pointeur à `NULL` après `free`, ne jamais retourner l'adresse d'une variable locale.

### Détecter les erreurs
```bash
gcc -g -fsanitize=address,undefined prog.c -o prog && ./prog     # AddressSanitizer
valgrind --leak-check=full ./prog                                # Linux
```
Voir [Débogage et Bonnes Pratiques](D%C3%A9bogage%20et%20Bonnes%20Pratiques.md).

> [!note] VLA
> `int t[n];` avec `n` variable (*VLA*, C99) alloue sur la **pile** : risque de dépassement pour de grandes tailles, optionnel depuis C11. Préférer `malloc` pour les tailles non bornées.

## Exercices

1. Lis `n`, alloue un tableau d'entiers, remplis-le avec les carrés de 1 à `n` et affiche-le.
2. Écris `char *dupliquer(const char *s)` qui renvoie une copie allouée (comme `strdup`).
3. Que se passe-t-il si on oublie de tester le retour de `malloc` ?

<details><summary>Corrigés</summary>

```c
char *dupliquer(const char *s)
{
    size_t n = strlen(s) + 1;               // +1 pour le '\0'
    char *copie = malloc(n);
    if (copie != NULL) memcpy(copie, s, n);
    return copie;                           // l'appelant doit faire free()
}
```
3. Si l'allocation échoue, on déréférence `NULL` → `Segmentation fault`.
</details>

## Voir aussi

[Pointeurs](Pointeurs.md) · [Structures de Données](Structures%20de%20Donn%C3%A9es.md) · [Fichiers](../Exo/Fichiers.md) · [Débogage et Bonnes Pratiques](D%C3%A9bogage%20et%20Bonnes%20Pratiques.md)
