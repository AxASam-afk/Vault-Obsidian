---
tags: [algorithmique, C, ESGI, tableaux]
---

# Tableaux

Lié à : [Variables et Opérateurs](Variables%20et%20Op%C3%A9rateurs.md) · [Structures de Contrôle](Structures%20de%20Contr%C3%B4le.md)

## Définition

> [!important] Définition
> Un tableau est une série d'**emplacements mémoire consécutifs**, servant à stocker des données **homogènes** (du même type). Le **nom** d'un tableau représente l'**adresse de son premier élément** ; les adresses des autres cases sont calculées automatiquement à partir de là.

- Tableau à **une dimension** (liste, vecteur) : une seule ligne, plusieurs colonnes.
- Tableau à **deux dimensions** (matrice) : plusieurs lignes, plusieurs colonnes.
- Le nombre de composantes **N** doit être **fixé à la compilation** : la taille d'un tableau ne peut pas être variable.
- Si une composante occupe M octets, un tableau de N composantes occupe **N × M octets**.
- **En C, les indices commencent toujours à 0.**

Application classique : le **tri de données** (placer des données en désordre dans un tableau, puis leur appliquer un algorithme de tri).

### Taille d'un tableau

```c
int taille = sizeof(tableau) / sizeof(tableau[0]);
```

## Tableaux à une dimension

### Déclaration

```c
Type NomTableau[Dimension];
Type NomTableau[Dimension] = {valeur1, valeur2, valeur3, ...};
```

```c
int A[25];       // 1er élément A[0], dernier élément A[24], N = 25
int A[] = {10, 20, 30, 40, 50};   // taille déduite : 5 éléments

int JOURS[12] = {31,28,31,30,31,30,31,31,30,31,30,31};

char mot[8] = "Bonjour";   // contient aussi le caractère de fin de chaîne '\0'
```

> [!important] Débordement d'indice
> Il faut toujours éviter le débordement d'indice, pour ne pas sortir du tableau.

Si la liste d'initialisation ne fournit pas assez de valeurs, les composantes restantes contiennent une **valeur indéterminée**.

### Remplissage et affichage (boucle for)

```c
int A[5];
int i;

for (i = 0; i < 5; i++)
    scanf("%d", &A[i]);

for (i = 0; i < 5; i++)
    printf("%d ", A[i]);
```

### Chaînes de caractères

```c
char prenom[20];
scanf("%19s", &prenom);
printf("Bonjour %s !", prenom);
```

## Tableaux à deux dimensions

Le C ne définit pas nativement les notions de "ligne" et de "colonne" : c'est au programmeur de s'en souvenir. Une matrice est en mémoire un tableau (1D) de dimension L dont chaque composante est elle-même un tableau (1D) de dimension C.

### Déclaration

```c
Type NomTableau[DimLigne][DimCol];
```

```c
int A[10][10];

float A[3][10] = {{ 0,10,20,30,40,50,60,70,80,90},
                   {10,11,12,13,14,15,16,17,18,19},
                   { 1,12,23,34,45,56,67,78,89,90}};
```

Indices : de 0 à L-1 (lignes) et 0 à C-1 (colonnes). Premier élément : `A[0][0]`. Dernier : `A[N-1][M-1]`.

### Accès à un élément

`NomTableau[Ligne][Colonne]`

### Remplissage et affichage

```c
int A[5][10];
int i, j;

for (i = 0; i < 5; i++)         // pour chaque ligne
    for (j = 0; j < 10; j++)    // pour chaque colonne
        scanf("%d", &A[i][j]);

for (i = 0; i < 5; i++)
{
    for (j = 0; j < 10; j++)
        printf("%7d", A[i][j]);
    printf("\n");                // retour à la ligne
}
```

## Algorithme de tri à bulles

> [!important] Principe
> On parcourt le tableau **plusieurs fois**, de la première à l'avant-dernière case. Chaque fois qu'une valeur est plus grande que sa voisine suivante, on **permute** les deux (via une variable de stockage intermédiaire) : les grandes valeurs "remontent" progressivement vers la fin, comme des bulles.

Une variable **témoin** (initialisée à chaque passage) permet de détecter que le tableau est trié : si aucune permutation n'a eu lieu lors d'un passage complet, le tri est terminé.

```c
int n, v[100];
// initialiser v[] avec n éléments ...
bool trie;

do
{
    trie = true;
    for (int i = 0; i < n - 1; i++)
        if (v[i] > v[i+1])
        {
            int aux = v[i];
            v[i] = v[i+1];
            v[i+1] = aux;
            trie = false;
        }
} while (!trie);
```

## Copier un tableau

`memcpy(destination, origine, nombre_octets);` — nécessite `#include <string.h>`.

```c
short tab1[5]  = {1, 2, 3, 4, 5};
short tab2[10] = {10, 20, 30, 40, 50, 60, 70, 80, 90, 100};

memcpy(tab2, tab1, 5 * sizeof(short));
// copie les 5 premières cases de tab1 vers les 5 premières cases de tab2

memcpy(&tab2[5], &tab1[2], 2 * sizeof(short));
// copie les 3e et 4e cases de tab1 vers les 6e et 7e cases de tab2
```

> [!important] Erreur classique
> `tab2 = tab1;` est **incorrect** : le nom d'un tableau n'est pas une variable affectable directement. Il faut passer par `memcpy`.

## Voir aussi

[Variables et Opérateurs](Variables%20et%20Op%C3%A9rateurs.md) · [Structures de Contrôle](Structures%20de%20Contr%C3%B4le.md) · [Fonctions](Fonctions.md) · [Algo/Cours 5 - Les Tableaux en Algorithmie.md](../../Algo/Cours/Cours%205%20-%20Les%20Tableaux%20en%20Algorithmie.md)