---
tags: [algorithmique, C, ESGI, préprocesseur, makefile, modules]
---

# Préprocesseur et Projets multi-fichiers

Fait suite à : [Fichiers](../Exo/Fichiers.md) · [Chaîne de compilation](Cha%C3%AEne%20de%20compilation.md) · Suite : [Structures de Données](Structures%20de%20Donn%C3%A9es.md)

## Le préprocesseur

Il traite les lignes commençant par `#` **avant** la compilation ([Chaîne de compilation](Cha%C3%AEne%20de%20compilation.md)).

### `#include`
```c
#include <stdio.h>       // en-tête système (cherché dans les dossiers du compilateur)
#include "maths.h"       // en-tête de VOTRE projet (cherché d'abord dans le dossier courant)
```

### `#define` : constantes et macros
```c
#define TAILLE_MAX 100
#define PI 3.14159265

#define CARRE(x)  ((x) * (x))            // macro avec paramètre
#define MAX(a, b) ((a) > (b) ? (a) : (b))
```
> [!warning] Les macros sont du **remplacement de texte**
> - **Toujours parenthéser** chaque paramètre et le tout : `#define CARRE(x) x*x` donne `CARRE(2+1)` = `2+1*2+1` = 5 !
> - **Double évaluation** : `MAX(i++, j)` incrémente `i` deux fois. Préférer une **fonction** (ou `static inline`) quand c'est possible.
> - Pas de typage ni de débogage facile. Pour une constante typée : `const` ou `enum`.

### Compilation conditionnelle
```c
#define DEBUG 1
#ifdef DEBUG
    printf("x = %d\n", x);               // compilé seulement en mode debug
#endif
```
`gcc -DDEBUG prog.c` définit `DEBUG` depuis la ligne de commande. Autres : `#ifndef`, `#if`, `#else`, `#elif`, `#undef`.

## Découper un programme en modules

Un module = **un fichier `.h`** (l'interface : prototypes, types, constantes) + **un fichier `.c`** (l'implémentation).

```text
projet/
├── main.c        programme principal
├── maths.h       déclarations (prototypes)
└── maths.c       définitions (code)
```

```c
/* maths.h */
#ifndef MATHS_H          // garde d'inclusion : évite la double inclusion
#define MATHS_H

int    carre(int x);
double moyenne(const int t[], int n);

#endif
```
```c
/* maths.c */
#include "maths.h"

int carre(int x) { return x * x; }

double moyenne(const int t[], int n)
{
    double s = 0;
    for (int i = 0; i < n; i++) s += t[i];
    return n > 0 ? s / n : 0;
}
```
```c
/* main.c */
#include <stdio.h>
#include "maths.h"

int main(void)
{
    int t[] = {10, 12, 14};
    printf("carre(5) = %d\n", carre(5));
    printf("moyenne  = %.2f\n", moyenne(t, 3));
    return 0;
}
```
Compilation :
```bash
gcc -Wall -Wextra -c maths.c           # → maths.o
gcc -Wall -Wextra -c main.c            # → main.o
gcc main.o maths.o -o programme        # édition de liens
# ou en une commande : gcc -Wall -Wextra main.c maths.c -o programme
```

> [!important] Règles
> - Un `.h` contient des **déclarations** (prototypes, `typedef`, `struct`, `#define`), **jamais de définitions de fonctions ni de variables** (sinon erreur « multiple definition » à l'édition de liens).
> - Les **gardes d'inclusion** (`#ifndef ... #define ... #endif`, ou `#pragma once`) sont obligatoires dans chaque `.h`.
> - Le `.c` inclut son propre `.h` pour que le compilateur vérifie la cohérence.

## `static` et `extern` (portée entre fichiers)

| Mot-clé | Effet |
|---|---|
| `static` (sur une fonction / variable globale) | **privée au fichier** `.c` (invisible des autres modules) |
| `extern int compteur;` | déclare une variable globale **définie ailleurs** |
| (rien) | visible de tous les fichiers (liaison externe) |
Bonne pratique : marquer `static` toute fonction interne à un module, éviter les variables globales.

## Makefile : automatiser la compilation

```make
CC      = gcc
CFLAGS  = -Wall -Wextra -std=c17 -g
OBJ     = main.o maths.o

programme: $(OBJ)
	$(CC) $(OBJ) -o programme -lm

%.o: %.c maths.h
	$(CC) $(CFLAGS) -c $< -o $@

clean:
	rm -f *.o programme
```
> [!warning] Les lignes de commande d'un Makefile commencent par une **tabulation**, pas des espaces.
`make` ne recompile que les fichiers modifiés ; `make clean` nettoie.

## Exercices

1. Crée un module `chaines.c/.h` avec `estPalindrome` et `compterVoyelles` ([Chaînes de Caractères](Cha%C3%AEnes%20de%20Caract%C3%A8res.md)).
2. Pourquoi `#define CARRE(x) x*x` est-il dangereux ?
3. Que se passe-t-il si on oublie la garde d'inclusion et qu'un `.h` est inclus deux fois ?

<details><summary>Corrigés</summary>

2. `CARRE(a+b)` → `a+b*a+b` : priorité des opérateurs.
3. Les `struct`/`typedef` sont redéfinis → erreur de compilation « redefinition ».
</details>

## Voir aussi

[Chaîne de compilation](Cha%C3%AEne%20de%20compilation.md) · [Fonctions](Fonctions.md) · [Projet Carnet de Contacts](Projet%20Carnet%20de%20Contacts.md)
