---
tags: [algorithmique, C, ESGI, projet, structures, fichiers, malloc]
---

# Projet — Carnet de contacts

Lié à : [Structures et Types Composés](Structures%20et%20Types%20Compos%C3%A9s.md) · [Mémoire et Allocation Dynamique](M%C3%A9moire%20et%20Allocation%20Dynamique.md) · [Fichiers](../Exo/Fichiers.md) · [Chaînes de Caractères](Cha%C3%AEnes%20de%20Caract%C3%A8res.md) · [Entrées Sorties](Entr%C3%A9es%20Sorties.md)

> [!important] Objectif
> Écrire un programme en C qui réunit **tout le cours** : structures, tableau dynamique (`realloc`), chaînes, fichiers, fonctions, `switch`, boucle `do…while`, gestion d'erreurs.

## Cahier des charges

1. **Ajouter** un contact (nom, téléphone).
2. **Lister** les contacts.
3. **Rechercher** par morceau de nom.
4. **Supprimer** un contact.
5. **Sauvegarder** dans `contacts.txt` (format `nom;telephone`) et **charger** au démarrage.
6. Le nombre de contacts n'est **pas limité** (tableau dynamique).

## Architecture

```text
Carnet { Contact *data; size_t taille; size_t capacite; }
  ├─ ajouter()      realloc (capacité doublée)
  ├─ lister()
  ├─ rechercher()   strstr
  ├─ supprimer()    memmove
  ├─ sauvegarder()  fprintf
  └─ charger()      fgets + strchr
main : menu (switch) dans une boucle do…while
```

## Code complet

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define NOM_MAX  50
#define TEL_MAX  20
#define FICHIER  "contacts.txt"

typedef struct
{
    char nom[NOM_MAX];
    char tel[TEL_MAX];
} Contact;

typedef struct
{
    Contact *data;
    size_t   taille;
    size_t   capacite;
} Carnet;

/* ---------- outils ---------- */

static void lireLigne(const char *invite, char *dest, size_t n)
{
    printf("%s", invite);
    if (fgets(dest, (int)n, stdin) == NULL)
    {
        dest[0] = '\0';
        return;
    }
    dest[strcspn(dest, "\n")] = '\0';
}

/* ---------- opérations ---------- */

static int ajouter(Carnet *c, const char *nom, const char *tel)
{
    if (c->taille == c->capacite)                       /* plein : agrandir */
    {
        size_t nouvelle = c->capacite ? c->capacite * 2 : 4;
        Contact *tmp = realloc(c->data, nouvelle * sizeof *tmp);
        if (tmp == NULL) return 0;
        c->data = tmp;
        c->capacite = nouvelle;
    }
    snprintf(c->data[c->taille].nom, NOM_MAX, "%s", nom);
    snprintf(c->data[c->taille].tel, TEL_MAX, "%s", tel);
    c->taille++;
    return 1;
}

static void lister(const Carnet *c)
{
    if (c->taille == 0)
    {
        printf("(carnet vide)\n");
        return;
    }
    for (size_t i = 0; i < c->taille; i++)
        printf("%2zu. %-20s %s\n", i + 1, c->data[i].nom, c->data[i].tel);
}

static void rechercher(const Carnet *c, const char *motif)
{
    int trouves = 0;
    for (size_t i = 0; i < c->taille; i++)
    {
        if (strstr(c->data[i].nom, motif) != NULL)
        {
            printf("%2zu. %-20s %s\n", i + 1, c->data[i].nom, c->data[i].tel);
            trouves++;
        }
    }
    if (trouves == 0) printf("Aucun resultat.\n");
}

static int supprimer(Carnet *c, size_t numero)          /* numero : 1..taille */
{
    if (numero < 1 || numero > c->taille) return 0;
    size_t i = numero - 1;
    memmove(&c->data[i], &c->data[i + 1], (c->taille - i - 1) * sizeof(Contact));
    c->taille--;
    return 1;
}

/* ---------- fichier ---------- */

static int sauvegarder(const Carnet *c)
{
    FILE *f = fopen(FICHIER, "w");
    if (f == NULL) return 0;
    for (size_t i = 0; i < c->taille; i++)
        fprintf(f, "%s;%s\n", c->data[i].nom, c->data[i].tel);
    fclose(f);
    return 1;
}

static int charger(Carnet *c)
{
    FILE *f = fopen(FICHIER, "r");
    if (f == NULL) return 0;                            /* premier lancement : pas de fichier */

    char ligne[NOM_MAX + TEL_MAX + 4];
    while (fgets(ligne, sizeof ligne, f) != NULL)
    {
        ligne[strcspn(ligne, "\n")] = '\0';
        char *sep = strchr(ligne, ';');
        if (sep == NULL) continue;                      /* ligne invalide : on l'ignore */
        *sep = '\0';
        ajouter(c, ligne, sep + 1);
    }
    fclose(f);
    return 1;
}

/* ---------- programme ---------- */

int main(void)
{
    Carnet carnet = {NULL, 0, 0};
    char nom[NOM_MAX], tel[TEL_MAX], saisie[16];
    int choix;

    charger(&carnet);

    do
    {
        printf("\n=== CARNET DE CONTACTS (%zu) ===\n", carnet.taille);
        printf("1. Ajouter\n2. Lister\n3. Rechercher\n4. Supprimer\n5. Sauvegarder\n0. Quitter\n");
        lireLigne("Choix : ", saisie, sizeof saisie);
        choix = atoi(saisie);

        switch (choix)
        {
            case 1:
                lireLigne("Nom : ", nom, sizeof nom);
                lireLigne("Telephone : ", tel, sizeof tel);
                if (nom[0] == '\0') printf("Nom vide : annule.\n");
                else if (!ajouter(&carnet, nom, tel)) printf("Memoire insuffisante.\n");
                break;
            case 2:
                lister(&carnet);
                break;
            case 3:
                lireLigne("Rechercher : ", nom, sizeof nom);
                rechercher(&carnet, nom);
                break;
            case 4:
                lister(&carnet);
                lireLigne("Numero a supprimer : ", saisie, sizeof saisie);
                if (!supprimer(&carnet, (size_t)atoi(saisie))) printf("Numero invalide.\n");
                break;
            case 5:
                printf(sauvegarder(&carnet) ? "Sauvegarde OK.\n" : "Echec de la sauvegarde.\n");
                break;
            case 0:
                break;
            default:
                printf("Choix invalide.\n");
        }
    } while (choix != 0);

    sauvegarder(&carnet);
    free(carnet.data);
    return 0;
}
```

## Points d'attention (relecture)

| Concept | Où dans le code | Cours |
|---|---|---|
| Structures imbriquées | `Contact`, `Carnet` | [Structures et Types Composés](Structures%20et%20Types%20Compos%C3%A9s.md) |
| Tableau qui grandit | `ajouter` (`realloc` + pointeur temporaire) | [Mémoire et Allocation Dynamique](M%C3%A9moire%20et%20Allocation%20Dynamique.md) |
| Lecture sûre | `lireLigne` (`fgets` + `strcspn`) | [Entrées Sorties](Entr%C3%A9es%20Sorties.md) |
| Copie sûre | `snprintf` | [Chaînes de Caractères](Cha%C3%AEnes%20de%20Caract%C3%A8res.md) |
| Décalage de tableau | `memmove` (zones qui se chevauchent : pas `memcpy`) | [Tableaux](Tableaux.md) |
| Fichier texte | `sauvegarder` / `charger` | [Fichiers](../Exo/Fichiers.md) |
| Fonctions `static` | privées au fichier | [Préprocesseur et Projets multi-fichiers](Pr%C3%A9processeur%20et%20Projets%20multi-fichiers.md) |
| Menu | `switch` dans `do … while` | [Structures de Contrôle](Structures%20de%20Contr%C3%B4le.md) |

## Évolutions proposées

1. **Trier** les contacts par nom avec `qsort` ([Tri et Recherche](Tri%20et%20Recherche.md)).
2. Éviter les **doublons**.
3. Découper en modules `carnet.c/.h` et `main.c`, avec un `Makefile`.
4. Passer à une **liste chaînée** ([Structures de Données](Structures%20de%20Donn%C3%A9es.md)).
5. Ajouter un champ `email` et gérer le format du fichier (séparateur `;` interdit dans les noms ?).
6. Version **binaire** avec `fwrite`/`fread`.
7. Vérifier avec `valgrind` : aucune fuite attendue.

## Voir aussi

[Exercices Cours 3-6](Exercices%20Cours%203-6.md) · [Débogage et Bonnes Pratiques](D%C3%A9bogage%20et%20Bonnes%20Pratiques.md) · [Aide-mémoire C](../Aide-m%C3%A9moire%20C.md)
