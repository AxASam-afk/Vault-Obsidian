---
tags: [algorithmique, C, ESGI, structures, enum, typedef]
---

# Structures et Types Composés

Fait suite à : [Chaînes de Caractères](Cha%C3%AEnes%20de%20Caract%C3%A8res.md) · Suite : [Mémoire et Allocation Dynamique](M%C3%A9moire%20et%20Allocation%20Dynamique.md)

## Pourquoi des structures ?

Un tableau regroupe des données **du même type**. Une **structure** (`struct`) regroupe des données de **types différents** qui forment un tout (un étudiant, un point, un contact).

```c
struct Etudiant
{
    char  nom[30];
    int   age;
    float moyenne;
};

struct Etudiant e1 = {"Awa", 19, 14.5f};
printf("%s a %d ans\n", e1.nom, e1.age);
e1.age++;                               // accès à un champ avec le point
```

## `typedef` : donner un nom de type

```c
typedef struct
{
    char  nom[30];
    int   age;
    float moyenne;
} Etudiant;                             // désormais : Etudiant e;  (sans "struct")

Etudiant e2 = { .nom = "Moussa", .age = 20, .moyenne = 12.0f };   // initialisation désignée
```

## Structures et fonctions

> [!important] Passage par valeur = copie de toute la structure
> Pour une grosse structure, on passe plutôt un **pointeur** (rapide, et permet de modifier). L'accès par pointeur se fait avec **`->`** : `p->age` est identique à `(*p).age`.

```c
#include <stdio.h>
#include <string.h>

typedef struct { char nom[30]; int age; float moyenne; } Etudiant;

void afficher(const Etudiant *e)                 // lecture seule, par adresse
{
    printf("%-10s %2d ans  moyenne %.2f\n", e->nom, e->age, e->moyenne);
}

void anniversaire(Etudiant *e)                   // modifie l'original
{
    e->age++;
}

Etudiant creer(const char *nom, int age, float moy)   // retour par valeur : OK
{
    Etudiant e;
    snprintf(e.nom, sizeof e.nom, "%s", nom);
    e.age = age;
    e.moyenne = moy;
    return e;
}

int main(void)
{
    Etudiant e = creer("Awa", 19, 14.5f);
    anniversaire(&e);
    afficher(&e);
    return 0;
}
```

## Tableaux de structures

```c
Etudiant classe[3] = {
    {"Awa",    19, 14.5f},
    {"Moussa", 20, 12.0f},
    {"Fatou",  18, 16.0f}
};

float somme = 0;
for (int i = 0; i < 3; i++) somme += classe[i].moyenne;
printf("Moyenne de la classe : %.2f\n", somme / 3);
```
Trier un tableau de structures : voir `qsort` dans [Tri et Recherche](Tri%20et%20Recherche.md).

## Structures imbriquées

```c
typedef struct { int jour, mois, annee; } Date;
typedef struct { char nom[30]; Date naissance; } Personne;

Personne p = {"Awa", {12, 3, 2006}};
printf("%d/%d/%d\n", p.naissance.jour, p.naissance.mois, p.naissance.annee);
```

## Taille d'une structure : l'alignement

```c
struct A { char c; int i; };        // sizeof = 8 (et non 5) : 3 octets de bourrage (padding)
```
Le compilateur **aligne** les champs sur leur taille. Toujours utiliser `sizeof(Etudiant)`, jamais calculer à la main. Ranger les champs du plus grand au plus petit réduit le bourrage.

## `enum` : des constantes nommées

```c
typedef enum { LUNDI, MARDI, MERCREDI, JEUDI, VENDREDI, SAMEDI, DIMANCHE } Jour;
// valeurs 0, 1, 2, ... ; on peut en imposer : enum { ROUGE = 1, VERT = 2, BLEU = 4 };

Jour j = MERCREDI;
switch (j)
{
    case SAMEDI:
    case DIMANCHE: printf("Week-end\n"); break;
    default:       printf("Semaine\n");  break;
}
```
Parfait avec `switch` ([Structures de Contrôle](Structures%20de%20Contr%C3%B4le.md)) pour des états, des menus, des codes d'erreur.

## `union` (aperçu)

Tous les champs **partagent la même zone mémoire** (la taille = celle du plus grand). On ne lit que le champ écrit en dernier. Usage : variantes, protocoles bas niveau.
```c
typedef union { int entier; float reel; unsigned char octets[4]; } Valeur;
```

## Exercices

1. Définis `Point` (x, y) et écris `double distance(Point a, Point b)` (`<math.h>`, `-lm`).
2. Crée un tableau de 3 `Etudiant`, affiche le meilleur.
3. Pourquoi passer `const Etudiant *` plutôt que `Etudiant` à une fonction d'affichage ?

<details><summary>Corrigés</summary>

```c
typedef struct { double x, y; } Point;
double distance(Point a, Point b)
{
    return sqrt((a.x - b.x) * (a.x - b.x) + (a.y - b.y) * (a.y - b.y));
}

const Etudiant *meilleur(const Etudiant t[], int n)
{
    const Etudiant *m = &t[0];
    for (int i = 1; i < n; i++)
        if (t[i].moyenne > m->moyenne) m = &t[i];
    return m;
}
```
3. Évite de **copier** toute la structure, et `const` garantit qu'elle n'est pas modifiée.
</details>

## Voir aussi

[Pointeurs](Pointeurs.md) · [Mémoire et Allocation Dynamique](M%C3%A9moire%20et%20Allocation%20Dynamique.md) · [Structures de Données](Structures%20de%20Donn%C3%A9es.md) · [Fichiers](../Exo/Fichiers.md)
