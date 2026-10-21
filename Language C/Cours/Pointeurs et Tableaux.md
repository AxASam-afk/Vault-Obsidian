---
tags: [algorithmique, C, ESGI, pointeurs, tableaux]
---

# Pointeurs et Tableaux

Fait suite à : [Pointeurs](Pointeurs.md) · [Tableaux](Tableaux.md) · Suite : [Chaînes de Caractères](Cha%C3%AEnes%20de%20Caract%C3%A8res.md)

## Le nom d'un tableau est une adresse

Rappel ([Tableaux](Tableaux.md)) : le nom d'un tableau représente l'adresse de son premier élément.
`tab` ≡ `&tab[0]` et `tab[i]` ≡ `*(tab + i)`.

```c
int t[4] = {10, 20, 30, 40};
int *p = t;                         // p pointe sur t[0]

printf("%d %d %d\n", *p, *(p + 1), p[2]);   // 10 20 30
```

## Arithmétique des pointeurs

> [!important] `p + 1` avance d'**un élément**, pas d'un octet
> Pour un `int *`, `p + 1` ajoute `sizeof(int)` (4) octets à l'adresse. L'arithmétique tient compte du **type pointé**.

| Opération | Effet |
|---|---|
| `p + n` / `p - n` | adresse n éléments plus loin / plus tôt |
| `p++` | passe à l'élément suivant |
| `q - p` | **nombre d'éléments** entre deux pointeurs du même tableau |
| `p < q`, `p == q` | comparaison d'adresses (même tableau) |

```c
int somme = 0;
for (int *p = t; p < t + 4; p++)    // parcours par pointeur
    somme += *p;
```

## Passer un tableau à une fonction

> [!important] Un tableau passé en paramètre « devient » un pointeur
> `int t[]` et `int *t` sont **identiques** dans la liste des paramètres. La fonction reçoit l'**adresse du premier élément**, pas une copie du tableau : elle **peut le modifier**. Elle ne connaît pas sa taille → **toujours passer la taille**.

```c
#include <stdio.h>

int somme(const int t[], int n)         // const : on ne modifie pas t
{
    int s = 0;
    for (int i = 0; i < n; i++) s += t[i];
    return s;
}

void doubler(int t[], int n)            // modifie le tableau de l'appelant
{
    for (int i = 0; i < n; i++) t[i] *= 2;
}

void afficher(const int t[], int n)
{
    for (int i = 0; i < n; i++) printf("%d ", t[i]);
    printf("\n");
}

int main(void)
{
    int tab[] = {1, 2, 3, 4, 5};
    int n = sizeof tab / sizeof tab[0];     // taille calculée ICI, là où tab est un vrai tableau

    doubler(tab, n);
    afficher(tab, n);                       // 2 4 6 8 10
    printf("somme = %d\n", somme(tab, n));  // 30
    return 0;
}
```

> [!warning] `sizeof` dans une fonction
> Dans `void f(int t[])`, `sizeof t` vaut la taille d'un **pointeur** (8), pas celle du tableau. Calculer la taille **avant** l'appel et la transmettre.

## Tableaux et fonctions : algorithmes courants

```c
int maximum(const int t[], int n)       // schéma du "max courant" (voir exercice 6 du corrigé)
{
    int max = t[0];
    for (int i = 1; i < n; i++)
        if (t[i] > max) max = t[i];
    return max;
}

void inverser(int t[], int n)
{
    for (int i = 0, j = n - 1; i < j; i++, j--)
    {
        int tmp = t[i];
        t[i] = t[j];
        t[j] = tmp;
    }
}

int rechercher(const int t[], int n, int x)     // renvoie l'indice, ou -1
{
    for (int i = 0; i < n; i++)
        if (t[i] == x) return i;
    return -1;
}
```

## Tableaux à deux dimensions en paramètre

Les **colonnes** doivent être connues du compilateur (pour calculer l'adresse de `m[i][j]`) :
```c
void afficherMatrice(int m[][3], int lignes)
{
    for (int i = 0; i < lignes; i++)
    {
        for (int j = 0; j < 3; j++) printf("%4d", m[i][j]);
        printf("\n");
    }
}
// appel :  int M[2][3] = {{1,2,3},{4,5,6}};  afficherMatrice(M, 2);
```
Pour des dimensions variables : tableau 1D simulé (`m[i * nbCol + j]`) ou allocation dynamique ([Mémoire et Allocation Dynamique](M%C3%A9moire%20et%20Allocation%20Dynamique.md)).

## Différences tableau / pointeur

| | Tableau `int t[4]` | Pointeur `int *p` |
|---|---|---|
| Mémoire réservée | 4 `int` consécutifs | une adresse |
| `sizeof` | 16 | 8 |
| Affectation `t = ...` | **interdite** | autorisée |
| `t++` | interdit | autorisé |

## Initialisation partielle

`int t[5] = {1, 2};` → `{1, 2, 0, 0, 0}` : les éléments **non précisés valent 0** (et non « indéterminés » comme dans les notes de base). `int t[5] = {0};` met tout à 0. Un tableau local **sans** initialisation contient des valeurs indéterminées.

## Exercices

1. Écris `int sommeParPointeur(const int *debut, const int *fin)`.
2. Écris `void copier(int dst[], const int src[], int n)` sans `memcpy`.
3. Écris `int estTrie(const int t[], int n)`.

<details><summary>Corrigés</summary>

```c
int sommeParPointeur(const int *debut, const int *fin)
{
    int s = 0;
    for (const int *p = debut; p < fin; p++) s += *p;
    return s;
}

void copier(int dst[], const int src[], int n)
{
    for (int i = 0; i < n; i++) dst[i] = src[i];
}

int estTrie(const int t[], int n)
{
    for (int i = 0; i + 1 < n; i++)
        if (t[i] > t[i + 1]) return 0;
    return 1;
}
```
</details>

## Voir aussi

[Pointeurs](Pointeurs.md) · [Tableaux](Tableaux.md) · [Tri et Recherche](Tri%20et%20Recherche.md) · [Chaînes de Caractères](Cha%C3%AEnes%20de%20Caract%C3%A8res.md)
y