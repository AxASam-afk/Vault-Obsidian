---
tags: [algorithmique, C, ESGI, récursivité]
---

# Récursivité

Fait suite à : [Tri et Recherche](Tri%20et%20Recherche.md) · [Fonctions](Fonctions.md) (notions de base) · Suite : [Débogage et Bonnes Pratiques](D%C3%A9bogage%20et%20Bonnes%20Pratiques.md)

> [!important] Rappel
> Une fonction **récursive** s'appelle elle-même. Elle comporte toujours :
> 1. un **cas de base** (condition d'arrêt) qui ne s'appelle pas ;
> 2. un **appel récursif** sur un problème **plus petit**, qui se rapproche du cas de base.

## La pile d'appels

Chaque appel crée un **cadre** (variables locales, paramètres, adresse de retour) sur la **pile** ([Mémoire et Allocation Dynamique](M%C3%A9moire%20et%20Allocation%20Dynamique.md)).

```c
long factorielle(int n)
{
    if (n <= 1) return 1;               // cas de base (n <= 1 couvre aussi n = 0)
    return n * factorielle(n - 1);
}
```
```text
factorielle(3)
 └─ 3 * factorielle(2)
        └─ 2 * factorielle(1)
               └─ 1                   ← cas de base
        ← 2 * 1 = 2
 ← 3 * 2 = 6
```
> [!warning] Le cours de base teste `n == 1`
> Avec `if (n == 1)`, `factorielle(0)` ne s'arrête jamais (n devient négatif) → **dépassement de pile** (*stack overflow*). `n <= 1` est plus sûr.

Sans cas de base atteignable : récursion infinie → `Segmentation fault`.

## Récursif ou itératif ?

| | Récursif | Itératif |
|---|---|---|
| Lisibilité | souvent plus proche de la définition mathématique | parfois plus long |
| Performance | surcoût des appels, consommation de pile | généralement plus rapide |
| Quand le préférer | structures **récursives** (arbres), diviser pour régner | simples boucles, grandes profondeurs |

Toute récursion peut s'écrire avec une boucle (et une pile explicite si besoin).

## Exemples classiques

### PGCD (algorithme d'Euclide)
```c
int pgcd(int a, int b)
{
    return b == 0 ? a : pgcd(b, a % b);
}
// pgcd(48, 18) → pgcd(18, 12) → pgcd(12, 6) → pgcd(6, 0) → 6
```

### Puissance rapide : O(log n) au lieu de O(n)
```c
long long puissance(long long a, int n)
{
    if (n == 0) return 1;
    long long moitie = puissance(a, n / 2);
    return (n % 2 == 0) ? moitie * moitie : moitie * moitie * a;
}
```
Comparer avec `puissance_rec` du cours ([Fonctions](Fonctions.md)) qui fait *n* appels.

### Suite de Fibonacci : le piège de l'exponentiel
```c
long long fibNaif(int n)                // O(2^n) : recalcule les mêmes valeurs !
{
    if (n < 2) return n;
    return fibNaif(n - 1) + fibNaif(n - 2);
}

long long fibMemo(int n, long long memo[])      // mémoïsation : O(n)
{
    if (n < 2) return n;
    if (memo[n] != 0) return memo[n];
    memo[n] = fibMemo(n - 1, memo) + fibMemo(n - 2, memo);
    return memo[n];
}
```
`fibNaif(40)` prend plusieurs secondes ; `fibMemo(40, memo)` est instantané. **Mémoïsation** = mémoriser les résultats déjà calculés.

### Les tours de Hanoï
Déplacer *n* disques de `src` vers `dst` en utilisant `aux`, sans jamais poser un grand disque sur un petit.
```c
#include <stdio.h>

void hanoi(int n, char src, char aux, char dst)
{
    if (n == 0) return;
    hanoi(n - 1, src, dst, aux);                    // 1) n-1 disques vers l'auxiliaire
    printf("%c -> %c\n", src, dst);                 // 2) le plus grand vers la destination
    hanoi(n - 1, aux, src, dst);                    // 3) n-1 disques sur la destination
}

int main(void)
{
    hanoi(3, 'A', 'B', 'C');                        // 7 déplacements (2^n - 1)
    return 0;
}
```

### Inverser une chaîne
```c
void inverserRec(char *s, int g, int d)
{
    if (g >= d) return;
    char t = s[g]; s[g] = s[d]; s[d] = t;
    inverserRec(s, g + 1, d - 1);
}
// inverserRec(mot, 0, strlen(mot) - 1);
```

### Recherche dichotomique récursive
```c
int dichoRec(const int t[], int g, int d, int x)
{
    if (g > d) return -1;
    int m = g + (d - g) / 2;
    if (t[m] == x) return m;
    return (t[m] < x) ? dichoRec(t, m + 1, d, x) : dichoRec(t, g, m - 1, x);
}
```

## Méthode pour écrire une fonction récursive

1. Définir **ce que fait** la fonction (son contrat) en une phrase.
2. Trouver le **cas de base** le plus simple.
3. Exprimer le problème de taille *n* à partir de taille *n − 1* (ou *n/2*).
4. **Faire confiance** à l'appel récursif (hypothèse de récurrence).
5. Vérifier que chaque appel **se rapproche** du cas de base.

## Exercices

1. Écris `int sommeChiffres(int n)` récursive (`1234` → `10`).
2. Écris `int nbChiffres(int n)`.
3. Combien de déplacements pour Hanoï avec 5 disques ?

<details><summary>Corrigés</summary>

```c
int sommeChiffres(int n) { return n < 10 ? n : (n % 10) + sommeChiffres(n / 10); }
int nbChiffres(int n)    { return n < 10 ? 1 : 1 + nbChiffres(n / 10); }
```
3. 2⁵ − 1 = **31**.
</details>

## Voir aussi

[Fonctions](Fonctions.md) · [Tri et Recherche](Tri%20et%20Recherche.md) · [Structures de Données](Structures%20de%20Donn%C3%A9es.md) · [Mémoire et Allocation Dynamique](M%C3%A9moire%20et%20Allocation%20Dynamique.md)
