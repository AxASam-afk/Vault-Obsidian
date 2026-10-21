---
tags: [algorithmique, C, ESGI, tri, recherche, complexité]
---

# Tri et Recherche

Fait suite à : [Structures de Données](Structures%20de%20Donn%C3%A9es.md) · [Tableaux](Tableaux.md) (tri à bulles) · Suite : [Récursivité](R%C3%A9cursivit%C3%A9.md)

## Complexité : mesurer un algorithme

> [!important] Idée
> On compte le **nombre d'opérations** en fonction de la taille *n* des données, en ne gardant que le terme dominant : c'est la notation **O( )** (« grand O »).

| Complexité | Nom | n = 1 000 → opérations ≈ | Exemple |
|---|---|---|---|
| O(1) | constante | 1 | accès `t[i]` |
| O(log n) | logarithmique | 10 | recherche dichotomique |
| O(n) | linéaire | 1 000 | parcours, recherche séquentielle |
| O(n log n) | quasi-linéaire | 10 000 | tri rapide / fusion, `qsort` |
| O(n²) | quadratique | 1 000 000 | tri à bulles / sélection / insertion |
| O(2ⁿ) | exponentielle | astronomique | Fibonacci récursif naïf |

## Recherche

### Séquentielle : O(n)
Parcourir les éléments un à un ([Pointeurs et Tableaux](Pointeurs%20et%20Tableaux.md), `rechercher`). Fonctionne sur un tableau **non trié**.

### Dichotomique : O(log n)
Condition : le tableau est **trié**. On compare avec l'élément du **milieu** et on élimine la moitié à chaque étape.
```text
t = [2 5 8 12 16 23 38 56]    cherche 23
 g=0, d=7, m=3 → 12 < 23 → g=4
 g=4, d=7, m=5 → 23       → trouvé à l'indice 5
```

```c
#include <stdio.h>

int dichotomie(const int t[], int n, int x)         // renvoie l'indice ou -1
{
    int g = 0, d = n - 1;
    while (g <= d)
    {
        int m = g + (d - g) / 2;                    // évite le dépassement de (g+d)
        if (t[m] == x) return m;
        if (t[m] < x)  g = m + 1;
        else           d = m - 1;
    }
    return -1;
}

int main(void)
{
    int t[] = {2, 5, 8, 12, 16, 23, 38, 56};
    printf("%d\n", dichotomie(t, 8, 23));           // 5
    printf("%d\n", dichotomie(t, 8, 7));            // -1
    return 0;
}
```
Sur 1 million d'éléments : au plus ~20 comparaisons (contre 1 000 000 en séquentiel).

## Tris simples : O(n²)

### Tri à bulles
Voir [Tableaux](Tableaux.md) : on échange les voisins mal ordonnés, jusqu'à ce qu'il n'y ait plus d'échange.

### Tri par sélection
À chaque tour, on cherche le **minimum** du reste du tableau et on le place en tête. Au plus *n − 1* échanges.
```c
void triSelection(int t[], int n)
{
    for (int i = 0; i < n - 1; i++)
    {
        int min = i;
        for (int j = i + 1; j < n; j++)
            if (t[j] < t[min]) min = j;
        if (min != i)
        {
            int tmp = t[i];
            t[i] = t[min];
            t[min] = tmp;
        }
    }
}
```

### Tri par insertion
On maintient une partie gauche **déjà triée** et on **insère** chaque nouvel élément à sa place (comme trier des cartes en main). Très efficace sur des données **presque triées** (O(n)).
```c
void triInsertion(int t[], int n)
{
    for (int i = 1; i < n; i++)
    {
        int cle = t[i];
        int j = i - 1;
        while (j >= 0 && t[j] > cle)
        {
            t[j + 1] = t[j];                // décaler vers la droite
            j--;
        }
        t[j + 1] = cle;
    }
}
```

| Tri | Pire cas | Meilleur cas | Stable ? | Remarque |
|---|---|---|:-:|---|
| Bulles | O(n²) | O(n) (avec témoin) | oui | pédagogique |
| Sélection | O(n²) | O(n²) | non | peu d'échanges |
| Insertion | O(n²) | O(n) | oui | bon sur petits/presque triés |
| Fusion | O(n log n) | O(n log n) | oui | mémoire supplémentaire |
| Rapide (quick) | O(n²) | O(n log n) | non | le plus utilisé en pratique |

## Tri rapide et fusion : O(n log n)

- **Tri fusion** : on **divise** le tableau en deux moitiés, on trie chacune **récursivement**, puis on **fusionne** deux tableaux triés ([Récursivité](R%C3%A9cursivit%C3%A9.md)).
- **Tri rapide** : on choisit un **pivot**, on place à gauche les plus petits, à droite les plus grands, puis on trie chaque côté récursivement.

En pratique, on utilise la fonction standard **`qsort`** de `<stdlib.h>` :

```c
#include <stdio.h>
#include <stdlib.h>

int comparerInt(const void *a, const void *b)
{
    int x = *(const int *)a;
    int y = *(const int *)b;
    return (x > y) - (x < y);               // -1, 0 ou 1 (évite x - y qui peut déborder)
}

int main(void)
{
    int t[] = {42, 7, 19, 3, 25, 11};
    int n = sizeof t / sizeof t[0];

    qsort(t, n, sizeof t[0], comparerInt);  // (tableau, nb éléments, taille d'un élément, comparateur)

    for (int i = 0; i < n; i++) printf("%d ", t[i]);   // 3 7 11 19 25 42
    printf("\n");
    return 0;
}
```
`qsort` fonctionne avec **n'importe quel type** grâce à `void *` et à la fonction de comparaison ([Pointeurs](Pointeurs.md)). Pour trier des chaînes ou des structures, on écrit un comparateur adapté :
```c
int comparerMoyenne(const void *a, const void *b)
{
    const Etudiant *x = a, *y = b;
    return (x->moyenne < y->moyenne) - (x->moyenne > y->moyenne);   // ordre décroissant
}
```
(voir [Structures et Types Composés](Structures%20et%20Types%20Compos%C3%A9s.md)). De même, `bsearch` fait une recherche dichotomique générique.

## Exercices

1. Trie `{5, 2, 9, 1, 7}` à la main avec le tri par insertion en écrivant le tableau à chaque étape.
2. Écris un tri par sélection **décroissant**.
3. Combien de comparaisons au maximum pour chercher dans 1 024 éléments triés par dichotomie ?

<details><summary>Corrigés</summary>

1. `[5 2 9 1 7]` → `[2 5 9 1 7]` → `[2 5 9 1 7]` → `[1 2 5 9 7]` → `[1 2 5 7 9]`.
2. Chercher le **maximum** du reste du tableau (`if (t[j] > t[max]) max = j;`) et le placer en position `i`.
3. log₂(1024) = 10 (soit 11 au plus en comptant le dernier test).
</details>

## Voir aussi

[Tableaux](Tableaux.md) · [Pointeurs et Tableaux](Pointeurs%20et%20Tableaux.md) · [Récursivité](R%C3%A9cursivit%C3%A9.md) · [Structures de Données](Structures%20de%20Donn%C3%A9es.md)
