---
tags: [algorithmique, C, ESGI, pointeurs]
---

# Pointeurs

Fait suite à : [Fonctions](Fonctions.md) · [Entrées Sorties](Entr%C3%A9es%20Sorties.md) · Suite : [Pointeurs et Tableaux](Pointeurs%20et%20Tableaux.md)

> [!important] Définition
> Un **pointeur** est une variable qui contient une **adresse mémoire** (en général l'adresse d'une autre variable). C'est ce qui permet à une fonction de modifier une variable de l'appelant, de manipuler des tableaux, des chaînes et de la mémoire dynamique.

## Adresse et déréférencement

| Notation | Lecture | Rôle |
|---|---|---|
| `&x` | « adresse de x » | donne l'adresse de `x` |
| `int *p;` | « p est un pointeur sur int » | déclaration |
| `*p` | « contenu pointé par p » | **déréférencement** (lecture/écriture) |

```c
int a = 10;
int *p = &a;                // p contient l'adresse de a

printf("%d\n", a);          // 10
printf("%d\n", *p);         // 10   (valeur pointée)
printf("%p\n", (void *)p);  // adresse de a, ex. 0x7ffd5c3a

*p = 20;                    // modifie a !
printf("%d\n", a);          // 20
```

```text
Mémoire :
 Adresse     Nom   Contenu
 0x1000      a     20
 0x1008      p     0x1000   ──► pointe vers a
```

- Un pointeur occupe la taille d'une adresse (8 octets sur un système 64 bits), **quel que soit le type pointé**.
- Le type pointé sert à savoir **combien d'octets lire** et comment les interpréter.
- `NULL` est l'adresse « nulle » : un pointeur qui ne pointe sur rien. Toujours **initialiser** un pointeur (`int *p = NULL;`) et **tester** avant de déréférencer (`if (p != NULL)`).

> [!warning] Piège de déclaration
> `int* a, b;` déclare `a` pointeur **et** `b` simple `int`. Écrire `int *a, *b;` pour deux pointeurs (ou une déclaration par ligne).

## Pourquoi des pointeurs ? Le passage par adresse

Rappel ([Fonctions](Fonctions.md)) : en C, les paramètres sont **copiés** (passage par valeur). Pour modifier la variable de l'appelant, on passe son **adresse**.

```c
#include <stdio.h>

void changeValeur(int v)       { v *= 100; }       // modifie la copie
void changeAdresse(int *v)     { *v *= 100; }      // modifie l'original

void echanger(int *a, int *b)
{
    int tmp = *a;
    *a = *b;
    *b = tmp;
}

int main(void)
{
    int x = 5, y = 7;

    changeValeur(x);
    printf("%d\n", x);          // 5   (inchangé)

    changeAdresse(&x);
    printf("%d\n", x);          // 500

    echanger(&x, &y);
    printf("x=%d y=%d\n", x, y);   // x=7 y=500
    return 0;
}
```
C'est la raison du `&` dans `scanf("%d", &n)` : `scanf` reçoit l'**adresse** pour y écrire la valeur.

### Retourner plusieurs résultats
Une fonction ne retourne qu'une valeur ; on passe donc des **pointeurs de sortie** :
```c
void minMax(const int t[], int n, int *min, int *max)
{
    *min = *max = t[0];
    for (int i = 1; i < n; i++)
    {
        if (t[i] < *min) *min = t[i];
        if (t[i] > *max) *max = t[i];
    }
}
// appel : int mn, mx;  minMax(tab, 5, &mn, &mx);
```

## `const` et pointeurs

| Déclaration | Signification |
|---|---|
| `const int *p` | on **ne peut pas modifier la valeur pointée** (`*p = 3` interdit) |
| `int *const p` | on **ne peut pas changer l'adresse** contenue dans `p` |
| `const int *const p` | ni l'un ni l'autre |
Bonne pratique : déclarer `const` tout paramètre pointeur qu'une fonction **ne modifie pas** (`const int t[]`, `const char *s`).

## Pointeur générique `void *`
Pointe vers « n'importe quoi » (retour de `malloc`, paramètres de `qsort`). Il faut le **convertir** (cast) vers un type précis avant de déréférencer. Pas d'arithmétique sur `void *`.

## Pointeur de pointeur
`int **pp = &p;` : `pp` pointe sur `p`, qui pointe sur un `int`. Utile pour modifier un **pointeur** dans une fonction (ex. tête de liste) et pour les tableaux de chaînes ([Chaînes de Caractères](Cha%C3%AEnes%20de%20Caract%C3%A8res.md)).

## Erreurs classiques

| Erreur | Conséquence |
|---|---|
| Pointeur **non initialisé** : `int *p; *p = 3;` | écriture n'importe où → plantage |
| Déréférencer `NULL` | `Segmentation fault` |
| Retourner l'adresse d'une **variable locale** : `int *f(void){ int x=5; return &x; }` | la variable disparaît au retour (*dangling pointer*) |
| Confondre `p` et `*p` | comparer/afficher une adresse au lieu d'une valeur |
| Pointer sur de la mémoire libérée (`free`) | comportement indéfini ([Mémoire et Allocation Dynamique](M%C3%A9moire%20et%20Allocation%20Dynamique.md)) |

## Exercices

1. Écris `void incrementer(int *n)` qui ajoute 1 à la variable pointée.
2. Écris `void diviser(int a, int b, int *q, int *r)` qui renvoie quotient et reste.
3. Que affiche : `int a = 3; int *p = &a; *p = *p * 2; printf("%d", a);` ?

<details><summary>Corrigés</summary>

```c
void incrementer(int *n) { (*n)++; }                     // (*n)++ et non *n++ !
void diviser(int a, int b, int *q, int *r) { *q = a / b; *r = a % b; }
```
3. `6`.
</details>

> [!warning] Priorité : `*n++` ≠ `(*n)++`
> `*n++` incrémente le **pointeur** (puis lit la valeur) ; `(*n)++` incrémente la **valeur pointée**.

## Voir aussi

[Fonctions](Fonctions.md) · [Pointeurs et Tableaux](Pointeurs%20et%20Tableaux.md) · [Mémoire et Allocation Dynamique](M%C3%A9moire%20et%20Allocation%20Dynamique.md) · [Structures et Types Composés](Structures%20et%20Types%20Compos%C3%A9s.md)
