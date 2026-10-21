---
tags: [algorithmique, C, ESGI, exercices, corrige]
---

# Corrigé — Exercices Cours 3 à 6 (pointeurs, chaînes, structures, mémoire, fichiers, algorithmes)

Lié à : [Pointeurs](Pointeurs.md) · [Pointeurs et Tableaux](Pointeurs%20et%20Tableaux.md) · [Chaînes de Caractères](Cha%C3%AEnes%20de%20Caract%C3%A8res.md) · [Structures et Types Composés](Structures%20et%20Types%20Compos%C3%A9s.md) · [Mémoire et Allocation Dynamique](M%C3%A9moire%20et%20Allocation%20Dynamique.md) · [Fichiers](../Exo/Fichiers.md) · [Tri et Recherche](Tri%20et%20Recherche.md) · [Récursivité](R%C3%A9cursivit%C3%A9.md)

> [!important] Avant de commencer
> Compiler avec les avertissements : `gcc -Wall -Wextra -std=c17 -g exoX.c -o exoX` (+ `-lm` si `<math.h>`). La suite des exercices de [1ESGI/Language C/Exo/Corrigé Exercices Cours 1-2](../Exo/Corrig%C3%A9%20Exercices%20Cours%201-2.md) est ici.

---

## Exercice 1 — Échanger deux variables

Écrire une fonction `echanger` qui permute les valeurs de deux entiers de l'appelant.

```c
#include <stdio.h>

void echanger(int *a, int *b)
{
    int tmp = *a;
    *a = *b;
    *b = tmp;
}

int main(void)
{
    int x = 3, y = 8;
    echanger(&x, &y);
    printf("x = %d, y = %d\n", x, y);       // x = 8, y = 3
    return 0;
}
```
> [!important] Pourquoi des pointeurs ?
> Avec `void echanger(int a, int b)`, la fonction permuterait des **copies** (passage par valeur, voir [Fonctions](Fonctions.md)). Il faut passer les **adresses** (`&x`, `&y`) pour modifier les originaux ([Pointeurs](Pointeurs.md)).

---

## Exercice 2 — Minimum et maximum d'un tableau

```c
#include <stdio.h>

void minMax(const int t[], int n, int *min, int *max)
{
    *min = *max = t[0];
    for (int i = 1; i < n; i++)
    {
        if (t[i] < *min) *min = t[i];
        if (t[i] > *max) *max = t[i];
    }
}

int main(void)
{
    int tab[] = {12, -4, 27, 3, 18};
    int n = sizeof tab / sizeof tab[0];
    int mn, mx;

    minMax(tab, n, &mn, &mx);
    printf("min = %d, max = %d\n", mn, mx);     // min = -4, max = 27
    return 0;
}
```
> [!important] Retourner deux résultats
> Une fonction C ne renvoie qu'**une** valeur. Les paramètres **pointeurs de sortie** (`*min`, `*max`) permettent d'en renvoyer plusieurs. On réutilise le schéma du « max courant » vu à l'exercice 6 de [1ESGI/Language C/Exo/Corrigé Exercices Cours 1-2](../Exo/Corrig%C3%A9%20Exercices%20Cours%201-2.md).

---

## Exercice 3 — Inverser un tableau en place

```c
#include <stdio.h>

void inverser(int t[], int n)
{
    for (int i = 0, j = n - 1; i < j; i++, j--)
    {
        int tmp = t[i];
        t[i] = t[j];
        t[j] = tmp;
    }
}

int main(void)
{
    int t[] = {1, 2, 3, 4, 5};
    inverser(t, 5);
    for (int i = 0; i < 5; i++) printf("%d ", t[i]);     // 5 4 3 2 1
    printf("\n");
    return 0;
}
```
> [!important] Deux indices qui se rejoignent
> `i` part du début, `j` de la fin, et on s'arrête quand ils se croisent : `n/2` échanges suffisent. Le tableau est modifié **en place** car on passe son adresse ([Pointeurs et Tableaux](Pointeurs%20et%20Tableaux.md)).

---

## Exercice 4 — Compter voyelles et mots

```c
#include <stdio.h>
#include <string.h>
#include <ctype.h>

int main(void)
{
    char phrase[100];
    int voyelles = 0, mots = 0, dansMot = 0;

    printf("Entrez une phrase : ");
    if (fgets(phrase, sizeof phrase, stdin) == NULL) return 1;
    phrase[strcspn(phrase, "\n")] = '\0';

    for (int i = 0; phrase[i] != '\0'; i++)
    {
        char c = (char)tolower((unsigned char)phrase[i]);
        if (strchr("aeiouy", c) != NULL) voyelles++;

        if (phrase[i] != ' ' && !dansMot) { mots++; dansMot = 1; }
        else if (phrase[i] == ' ')         dansMot = 0;
    }

    printf("%d voyelle(s), %d mot(s)\n", voyelles, mots);
    return 0;
}
```
> [!important] `fgets` plutôt que `scanf("%s")`
> `scanf("%s")` s'arrête au premier espace : impossible de lire une phrase. `fgets` lit la ligne entière (et on retire le `\n`). Voir [Entrées Sorties](Entr%C3%A9es%20Sorties.md) et [Chaînes de Caractères](Cha%C3%AEnes%20de%20Caract%C3%A8res.md).

---

## Exercice 5 — Palindrome

```c
#include <stdio.h>
#include <string.h>

int estPalindrome(const char *s)
{
    int i = 0, j = (int)strlen(s) - 1;
    while (i < j)
    {
        if (s[i] != s[j]) return 0;
        i++;
        j--;
    }
    return 1;
}

int main(void)
{
    char mot[50];
    printf("Entrez un mot : ");
    if (scanf("%49s", mot) != 1) return 1;

    printf("%s %s un palindrome\n", mot, estPalindrome(mot) ? "est" : "n'est pas");
    return 0;
}
```
> [!important] Même schéma que l'inversion
> Deux indices, un à chaque bout, qui se rapprochent : dès que deux caractères diffèrent, on peut conclure. L'opérateur ternaire `? :` ([Structures de Contrôle](Structures%20de%20Contr%C3%B4le.md)) sert à choisir le mot « est » / « n'est pas ».

---

## Exercice 6 — Gestion d'étudiants (structures)

Saisir 3 étudiants (nom, moyenne), afficher la liste, la moyenne de la classe et le meilleur.

```c
#include <stdio.h>

#define N 3

typedef struct
{
    char  nom[30];
    float moyenne;
} Etudiant;

int main(void)
{
    Etudiant classe[N];
    float somme = 0;
    int meilleur = 0;

    for (int i = 0; i < N; i++)
    {
        printf("Nom et moyenne de l'etudiant %d : ", i + 1);
        if (scanf("%29s %f", classe[i].nom, &classe[i].moyenne) != 2) return 1;
        somme += classe[i].moyenne;
        if (classe[i].moyenne > classe[meilleur].moyenne) meilleur = i;
    }

    for (int i = 0; i < N; i++)
        printf("%-10s %5.2f\n", classe[i].nom, classe[i].moyenne);

    printf("Moyenne de la classe : %.2f\n", somme / N);
    printf("Meilleur : %s (%.2f)\n", classe[meilleur].nom, classe[meilleur].moyenne);
    return 0;
}
```
> [!important] On mémorise l'indice, pas la valeur
> Garder l'**indice** du meilleur (`meilleur`) permet de retrouver à la fois le nom et la moyenne. Le champ `nom` est un tableau : pas de `&` dans `scanf`, contrairement à `&classe[i].moyenne` ([Structures et Types Composés](Structures%20et%20Types%20Compos%C3%A9s.md)).

---

## Exercice 7 — Moyenne avec un tableau dynamique

```c
#include <stdio.h>
#include <stdlib.h>

int main(void)
{
    int n;
    printf("Combien de notes ? ");
    if (scanf("%d", &n) != 1 || n <= 0) return 1;

    double *notes = malloc(n * sizeof *notes);
    if (notes == NULL)
    {
        fprintf(stderr, "Memoire insuffisante\n");
        return 1;
    }

    double somme = 0;
    for (int i = 0; i < n; i++)
    {
        printf("Note %d : ", i + 1);
        if (scanf("%lf", &notes[i]) != 1) { free(notes); return 1; }
        somme += notes[i];
    }

    printf("Moyenne = %.2f\n", somme / n);
    free(notes);
    return 0;
}
```
> [!important] Le cycle allouer → tester → utiliser → libérer
> La taille n'est connue qu'à l'exécution : un tableau statique ne convient pas. Sans `free`, la mémoire n'est jamais rendue. Noter `%lf` dans `scanf` pour un `double` ([Mémoire et Allocation Dynamique](M%C3%A9moire%20et%20Allocation%20Dynamique.md)).

---

## Exercice 8 — Sauvegarder et relire des notes dans un fichier

```c
#include <stdio.h>

int main(void)
{
    double notes[] = {12.5, 14.0, 9.75, 17.0};
    int n = sizeof notes / sizeof notes[0];

    FILE *f = fopen("notes.txt", "w");
    if (f == NULL) { perror("notes.txt"); return 1; }
    for (int i = 0; i < n; i++)
        fprintf(f, "%.2f\n", notes[i]);
    fclose(f);

    f = fopen("notes.txt", "r");
    if (f == NULL) { perror("notes.txt"); return 1; }

    double v, somme = 0;
    int nb = 0;
    while (fscanf(f, "%lf", &v) == 1)
    {
        somme += v;
        nb++;
    }
    fclose(f);

    if (nb > 0) printf("%d note(s), moyenne = %.2f\n", nb, somme / nb);
    return 0;
}
```
> [!important] Tester le retour de `fscanf`, pas `feof`
> `fscanf` renvoie le nombre de valeurs lues : la boucle s'arrête naturellement à la fin du fichier ou à la première donnée invalide ([Fichiers](../Exo/Fichiers.md)).

---

## Exercice 9 — Liste chaînée d'entiers

```c
#include <stdio.h>
#include <stdlib.h>

typedef struct Noeud
{
    int valeur;
    struct Noeud *suivant;
} Noeud;

Noeud *ajouterDebut(Noeud *tete, int v)
{
    Noeud *n = malloc(sizeof *n);
    if (n == NULL) return tete;
    n->valeur = v;
    n->suivant = tete;
    return n;
}

void afficher(const Noeud *tete)
{
    for (const Noeud *p = tete; p != NULL; p = p->suivant)
        printf("%d -> ", p->valeur);
    printf("NULL\n");
}

void liberer(Noeud *tete)
{
    while (tete != NULL)
    {
        Noeud *suivant = tete->suivant;
        free(tete);
        tete = suivant;
    }
}

int main(void)
{
    Noeud *liste = NULL;
    for (int i = 1; i <= 5; i++)
        liste = ajouterDebut(liste, i * 10);

    afficher(liste);            // 50 -> 40 -> 30 -> 20 -> 10 -> NULL
    liberer(liste);
    return 0;
}
```
> [!important] Les nouveaux éléments arrivent **en tête**
> D'où l'ordre inversé à l'affichage. `ajouterDebut` **retourne** la nouvelle tête (on ne peut pas modifier `liste` de l'appelant sans pointeur sur pointeur). Voir [Structures de Données](Structures%20de%20Donn%C3%A9es.md).

---

## Exercice 10 — Tri par insertion puis recherche dichotomique

```c
#include <stdio.h>

void triInsertion(int t[], int n)
{
    for (int i = 1; i < n; i++)
    {
        int cle = t[i];
        int j = i - 1;
        while (j >= 0 && t[j] > cle)
        {
            t[j + 1] = t[j];
            j--;
        }
        t[j + 1] = cle;
    }
}

int dichotomie(const int t[], int n, int x)
{
    int g = 0, d = n - 1;
    while (g <= d)
    {
        int m = g + (d - g) / 2;
        if (t[m] == x) return m;
        if (t[m] < x) g = m + 1;
        else          d = m - 1;
    }
    return -1;
}

int main(void)
{
    int t[] = {42, 7, 19, 3, 25, 11};
    int n = sizeof t / sizeof t[0];

    triInsertion(t, n);
    for (int i = 0; i < n; i++) printf("%d ", t[i]);     // 3 7 11 19 25 42
    printf("\n");

    int pos = dichotomie(t, n, 19);
    if (pos >= 0) printf("19 trouve a l'indice %d\n", pos);
    else          printf("19 absent\n");
    return 0;
}
```
> [!important] La dichotomie exige un tableau trié
> C'est pourquoi on trie **d'abord**. Sur un tableau non trié, le résultat serait faux. Voir [Tri et Recherche](Tri%20et%20Recherche.md).

---

## Exercice 11 — Récursivité : PGCD, factorielle, Fibonacci

```c
#include <stdio.h>

int pgcd(int a, int b)
{
    return b == 0 ? a : pgcd(b, a % b);
}

long factorielleRec(int n)
{
    if (n <= 1) return 1;
    return n * factorielleRec(n - 1);
}

long factorielleIter(int n)
{
    long r = 1;
    for (int i = 2; i <= n; i++) r *= i;
    return r;
}

long long fibIter(int n)
{
    long long a = 0, b = 1;
    for (int i = 0; i < n; i++)
    {
        long long suivant = a + b;
        a = b;
        b = suivant;
    }
    return a;
}

int main(void)
{
    printf("pgcd(48, 18) = %d\n", pgcd(48, 18));                  // 6
    printf("10! = %ld / %ld\n", factorielleRec(10), factorielleIter(10));   // 3628800
    printf("fib(40) = %lld\n", fibIter(40));                      // 102334155
    return 0;
}
```
> [!important] Récursif vs itératif
> Les deux factorielles donnent le même résultat. Pour Fibonacci, la version récursive naïve serait en O(2ⁿ) ; la boucle est en O(n). Voir [Récursivité](R%C3%A9cursivit%C3%A9.md). Attention au dépassement : `long` peut ne faire que 4 octets (Windows) → `13!` y déborde.

---

## Exercice 12 — Compter les occurrences d'un caractère dans un fichier

```c
#include <stdio.h>

int main(int argc, char *argv[])
{
    if (argc != 3)
    {
        fprintf(stderr, "Usage : %s fichier caractere\n", argv[0]);
        return 1;
    }

    FILE *f = fopen(argv[1], "r");
    if (f == NULL) { perror(argv[1]); return 1; }

    int c, compte = 0;
    while ((c = fgetc(f)) != EOF)
        if (c == argv[2][0]) compte++;

    fclose(f);
    printf("'%c' apparait %d fois\n", argv[2][0], compte);
    return 0;
}
```
> [!important] `int c` et non `char c`
> `fgetc` renvoie un **`int`** pour pouvoir distinguer `EOF` (−1) d'un vrai caractère. Utiliser `char` peut rendre la boucle infinie ou la stopper trop tôt. `argv` illustre le passage d'arguments en ligne de commande ([Chaînes de Caractères](Cha%C3%AEnes%20de%20Caract%C3%A8res.md)).

---

## Voir aussi

[Pointeurs](Pointeurs.md) · [Chaînes de Caractères](Cha%C3%AEnes%20de%20Caract%C3%A8res.md) · [Structures et Types Composés](Structures%20et%20Types%20Compos%C3%A9s.md) · [Mémoire et Allocation Dynamique](M%C3%A9moire%20et%20Allocation%20Dynamique.md) · [Fichiers](../Exo/Fichiers.md) · [Projet Carnet de Contacts](Projet%20Carnet%20de%20Contacts.md)
