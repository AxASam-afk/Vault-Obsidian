---
tags: [algorithmique, C, ESGI, liste-chainee, pile, file]
---

# Structures de Données

Fait suite à : [Préprocesseur et Projets multi-fichiers](Pr%C3%A9processeur%20et%20Projets%20multi-fichiers.md) · [Mémoire et Allocation Dynamique](M%C3%A9moire%20et%20Allocation%20Dynamique.md) · [Structures et Types Composés](Structures%20et%20Types%20Compos%C3%A9s.md) · Suite : [Tri et Recherche](Tri%20et%20Recherche.md)

## Tableau ou liste chaînée ?

| | Tableau | Liste chaînée |
|---|---|---|
| Mémoire | bloc contigu | maillons dispersés reliés par pointeurs |
| Accès au i-ᵉ élément | **O(1)** | O(n) |
| Insertion/suppression en tête | O(n) (décalage) | **O(1)** |
| Taille | fixe (ou `realloc`) | dynamique |
| Surcoût mémoire | aucun | 1 pointeur par élément |

## Liste simplement chaînée

Un **maillon** contient une valeur et un pointeur vers le **maillon suivant**. La liste est repérée par un pointeur de **tête** ; le dernier maillon pointe sur `NULL`.

```text
tete ──► [ 3 | ●─]──► [ 7 | ●─]──► [ 9 | NULL ]
```

```c
#include <stdio.h>
#include <stdlib.h>

typedef struct Noeud
{
    int valeur;
    struct Noeud *suivant;          // "struct Noeud" car le typedef n'existe pas encore ici
} Noeud;

Noeud *ajouterDebut(Noeud *tete, int v)
{
    Noeud *n = malloc(sizeof *n);
    if (n == NULL) return tete;     // échec : liste inchangée
    n->valeur = v;
    n->suivant = tete;
    return n;                       // nouvelle tête
}

Noeud *ajouterFin(Noeud *tete, int v)
{
    Noeud *n = malloc(sizeof *n);
    if (n == NULL) return tete;
    n->valeur = v;
    n->suivant = NULL;
    if (tete == NULL) return n;
    Noeud *p = tete;
    while (p->suivant != NULL) p = p->suivant;
    p->suivant = n;
    return tete;
}

void afficher(const Noeud *tete)
{
    for (const Noeud *p = tete; p != NULL; p = p->suivant)
        printf("%d -> ", p->valeur);
    printf("NULL\n");
}

Noeud *rechercher(Noeud *tete, int v)
{
    for (Noeud *p = tete; p != NULL; p = p->suivant)
        if (p->valeur == v) return p;
    return NULL;
}

Noeud *supprimer(Noeud *tete, int v)           // supprime la 1re occurrence
{
    Noeud **pp = &tete;                        // pointeur sur le pointeur à modifier
    while (*pp != NULL)
    {
        if ((*pp)->valeur == v)
        {
            Noeud *a = *pp;
            *pp = a->suivant;
            free(a);
            break;
        }
        pp = &(*pp)->suivant;
    }
    return tete;
}

void liberer(Noeud *tete)
{
    while (tete != NULL)
    {
        Noeud *suivant = tete->suivant;        // sauvegarder AVANT free
        free(tete);
        tete = suivant;
    }
}

int main(void)
{
    Noeud *liste = NULL;
    liste = ajouterDebut(liste, 3);
    liste = ajouterDebut(liste, 2);
    liste = ajouterFin(liste, 9);
    afficher(liste);                           // 2 -> 3 -> 9 -> NULL

    liste = supprimer(liste, 3);
    afficher(liste);                           // 2 -> 9 -> NULL

    printf("%s\n", rechercher(liste, 9) ? "trouve" : "absent");
    liberer(liste);
    return 0;
}
```
> [!important] Les fonctions qui modifient la tête **retournent la nouvelle tête** (ou reçoivent un `Noeud **`). Sinon la modification se perdrait (passage par valeur !).

## Pile (LIFO : dernier entré, premier sorti)

Opérations : `empiler` (push), `depiler` (pop), `sommet` (peek). Usages : annuler/rétablir, parenthésage, **pile d'appels** ([Récursivité](R%C3%A9cursivit%C3%A9.md)).

```c
#include <stdio.h>
#include <stdbool.h>

#define CAPACITE 100

typedef struct
{
    int donnees[CAPACITE];
    int sommet;                     // nombre d'éléments
} Pile;

void initPile(Pile *p)           { p->sommet = 0; }
bool pileVide(const Pile *p)     { return p->sommet == 0; }
bool pilePleine(const Pile *p)   { return p->sommet == CAPACITE; }

bool empiler(Pile *p, int v)
{
    if (pilePleine(p)) return false;
    p->donnees[p->sommet++] = v;
    return true;
}

bool depiler(Pile *p, int *v)
{
    if (pileVide(p)) return false;
    *v = p->donnees[--p->sommet];
    return true;
}

int main(void)
{
    Pile p;
    initPile(&p);
    empiler(&p, 1); empiler(&p, 2); empiler(&p, 3);

    int v;
    while (depiler(&p, &v)) printf("%d ", v);   // 3 2 1
    printf("\n");
    return 0;
}
```

## File (FIFO : premier entré, premier sorti)

Opérations : `enfiler` (enqueue), `defiler` (dequeue). Usages : files d'impression, tampons réseau, parcours en largeur. Implémentation par **tableau circulaire** :
```c
typedef struct
{
    int donnees[CAPACITE];
    int tete;                       // indice du prochain élément à sortir
    int taille;
} File;

bool enfiler(File *f, int v)
{
    if (f->taille == CAPACITE) return false;
    f->donnees[(f->tete + f->taille) % CAPACITE] = v;
    f->taille++;
    return true;
}

bool defiler(File *f, int *v)
{
    if (f->taille == 0) return false;
    *v = f->donnees[f->tete];
    f->tete = (f->tete + 1) % CAPACITE;
    f->taille--;
    return true;
}
```

## Pour aller plus loin
Liste **doublement chaînée**, **arbre binaire de recherche** (insertion/recherche en O(log n) en moyenne), **table de hachage**, **tas** (file de priorité).

## Exercices

1. Ajoute `int longueur(const Noeud *tete)` et `Noeud *inverser(Noeud *tete)`.
2. Vérifie qu'une expression est bien parenthésée avec une pile.
3. Pourquoi sauvegarde-t-on `tete->suivant` avant `free(tete)` dans `liberer` ?

<details><summary>Corrigés</summary>

```c
int longueur(const Noeud *t) { int n = 0; for (; t; t = t->suivant) n++; return n; }

Noeud *inverser(Noeud *tete)
{
    Noeud *prec = NULL;
    while (tete != NULL)
    {
        Noeud *suiv = tete->suivant;
        tete->suivant = prec;
        prec = tete;
        tete = suiv;
    }
    return prec;
}
```
3. Après `free`, lire `tete->suivant` serait un *use-after-free*.
</details>

## Voir aussi

[Mémoire et Allocation Dynamique](M%C3%A9moire%20et%20Allocation%20Dynamique.md) · [Récursivité](R%C3%A9cursivit%C3%A9.md) · [Tri et Recherche](Tri%20et%20Recherche.md)
