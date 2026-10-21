---
tags:
  - algorithmique
  - C
  - ESGI
  - variables
  - operateurs
---

# Variables et Opérateurs

## Algorithme

> [!important] Définition
> Un **algorithme** est une suite finie et non-ambiguë d'opérations, exécutées en temps fini, qui transforme des données d'entrée en données de sortie.

Trois questions pour concevoir un algorithme :
- De quelles **entrées** a-t-on besoin ?
- Comment s'y prendre (l'algorithme lui-même) ?
- Que veut-on obtenir en **sortie** ?

## Compilateur vs interpréteur

Le code source doit être transposé dans une forme lisible par la machine, via un **compilateur** ou un **interpréteur**.

> [!important] Interpréteur
> Traite le code **ligne par ligne**, pendant l'exécution. La conversion se fait à la volée et s'arrête dès qu'une erreur survient — ce qui simplifie le débogage (ligne fautive identifiée immédiatement).
> Langages : BASIC, Perl, Python, Ruby, PHP.

> [!important] Compilateur
> Traduit **tout le code source** en code machine **avant** l'exécution. Les erreurs ne s'affichent qu'à la fin, groupées.
> Langages : C, C++, Pascal.

| | Interpréteurs | Compilateurs |
|---|---|---|
| Moment de traduction | pendant l'exécution | avant l'exécution |
| Méthode | ligne par ligne | ensemble du code |
| Affichage des erreurs | après chaque ligne | groupées, en fin de compilation |
| Vitesse de traduction | élevée | faible |
| Efficacité de la traduction | faible | élevée |
| Effort de développement | faible | élevé |

Le **langage C** a été créé en 1972 par Denis Ritchie. On peut l'exécuter avec CodeBlocks (Windows/Linux/Mac).

## Bases numériques

"Base n" = utilise les chiffres de 0 à n-1.

| Base | Nom | Préfixe en C |
|---|---|---|
| 2 | binaire | `0b` |
| 8 | octale | (rarement utilisée) |
| 10 | décimale | aucun |
| 16 | hexadécimale | `0x` ou `0X` |

- Chaque chiffre = un **digit**. En binaire, un digit = un **bit** (*binary digit*), la plus petite unité d'information.
- L'hexadécimal (0-9 puis A-F) sert à écrire du binaire de façon plus compacte : 1 digit hexa = 4 bits (un *nibble*).

### Décimal → binaire / hexadécimal

Diviser successivement par la base de destination, puis lire les restes de bas en haut.

```
167 (déc) = 0b10100111
689 (déc) = 0x2B1
```

### Binaire / hexadécimal → décimal

Pour un nombre `xyzt` en base b : `xyzt = x·b³ + y·b² + z·b¹ + t·b⁰`

```
0x11010010 = 422 (déc)
0x5DA1     = 23969 (déc)
```

### Binaire ↔ hexadécimal

Regrouper les bits par paquets de 4 (nibbles), de droite à gauche.

```
0b101100100110101010 = 0x2C9AA
0x1B2D = 0b1101100101101
```

## Structure d'un programme C

```c
#include <stdio.h>
#include <stdlib.h>

int main()
{
    printf("Hello World !\n");
    return 0;
}
```

- `#include <stdio.h>` : bibliothèque d'entrées/sorties standard
- `#include <stdlib.h>` : allocation mémoire
- `#include <math.h>` : fonctions mathématiques (cos, sin, pow, sqrt…)
- `main()` : point de départ obligatoire du programme
- `printf(...)` : afficher du texte
- `;` : fin d'une instruction
- `{ }` : début/fin d'un bloc de code
- `return 0;` : le programme s'est terminé sans erreur
- `//` ou `/* */` : commentaire, ignoré par le compilateur

Un fichier source C a l'extension `.c`. Il est **compilé** puis, si besoin, ses liens sont **édités** (plusieurs fichiers) pour créer l'exécutable.

## Variables

> [!important] Définition
> Une variable est un **emplacement mémoire**, de taille limitée, permettant au programme de mémoriser une donnée d'un certain type dont la valeur n'est pas fixe.

Chaque variable possède :
- un **type** (le genre de valeur qu'elle contient)
- un **nom** (identifiant)
- une **valeur** (contenu actuel)

**Déclarer** une variable = lui réserver de l'espace mémoire (espace vide). **Initialiser** = lui attribuer une valeur.

### Types les plus utilisés

| Écriture complète | Écriture courante | Taille | Intervalle |
|---|---|---|---|
| signed char | char | 1 octet | -128 à 127 |
| unsigned char | unsigned char | 1 octet | 0 à 255 |
| signed short int | short | 2 octets | -32 768 à 32 767 |
| unsigned short int | unsigned short | 2 octets | 0 à 65 535 |
| signed long int | long | 4 octets | -2 147 483 648 à 2 147 483 647 |
| unsigned long int | unsigned long | 4 octets | 0 à 4 294 967 296 |
| long long int | long long | 8 octets | -9,2×10¹⁸ à 9,2×10¹⁸ |

`sizeof(type)` renvoie la taille en octets d'un type (ex : `sizeof(int)` → 2 octets).

### Règles de nommage

- lettres A-Z / a-z, chiffres 0-9, underscore `_`
- doit **commencer par une lettre**
- le C est **sensible à la casse** : `mavar`, `MAVAR`, `mAvAr` sont 3 variables différentes
- une variable **ne peut pas être déclarée deux fois**
- le **type est fixé** à la déclaration et ne change plus
- on ne peut initialiser qu'avec des **constantes**

```c
int age = 32;   // déclaration + initialisation
int nombre;     // déclaration seule
nombre = 10;    // initialisation ultérieure
```

### Variables locales vs globales

> [!important] Portée
> Une **variable locale** est déclarée dans une fonction ou un bloc `{ }` : elle n'existe que dans ce bloc et ses blocs enfants (après sa déclaration). Une variable de bloc enfant masque une variable de même nom du bloc parent.
> Une **variable globale** est déclarée hors de tout bloc : utilisable partout après sa définition, mais "risquée" car elle peut être masquée par une variable locale de même nom.

## Entrées / sorties

`printf("texte %format", variable);` → afficher
`scanf("%format", &variable);` → saisir au clavier (attention au `&`)

| Format | Type attendu |
|---|---|
| %d | int |
| %u | unsigned int |
| %ld | long |
| %f | float / double |
| %c | caractère |
| %s | chaîne de caractères |
| %X | hexadécimal |

```c
int nombreDeVies = 5, niveau = 2;
printf("Vous avez %d vies et vous êtes au niveau %d", nombreDeVies, niveau);
```

## Opérateurs

### Arithmétiques et affectation composée

`+  -  *  /  %` (modulo)

```c
Nombre += x;   // Nombre = Nombre + x
Nombre -= x;
Nombre *= x;
Nombre /= x;
Nombre %= x;
```

> [!important] Pré vs post-incrémentation
> `a++` (postfixé) : la valeur **actuelle** est utilisée, **puis** la variable est incrémentée.
> `++a` (préfixé) : la variable est **d'abord** incrémentée, **puis** sa nouvelle valeur est utilisée.
> ⚠️ À éviter : combiner plusieurs `++`/`--` dans une même expression (`c = a++ + ++b;`) — l'ordre d'évaluation n'est pas garanti. Utiliser des parenthèses en cas de doute.

### Logiques / comparaison (booléens)

| Symbole | Signification |
|---|---|
| == | est égal à |
| != | est différent de |
| > / < | supérieur / inférieur à |
| >= / <= | supérieur ou égal / inférieur ou égal |

- `=` sert à l'**affectation**, `==` à la **comparaison** — ne pas confondre.
- TRUE = toute valeur non nulle, FALSE = 0.
- `&&` (ET), `||` (OU inclusif), `!` (NON)
- Évaluation **de gauche à droite**, en court-circuit : `TRUE || x` n'évalue pas `x`, `FALSE && x` non plus.
- Il n'existe pas d'opérateur XOR booléen en C.

### Bit à bit

```c
c = a | b;   // OR
c = a & b;   // AND
c = a ^ b;   // XOR
c = ~a;      // NOT
b = a << 2;  // décalage gauche = × 2²
b = a >> 4;  // décalage droite = ÷ 2⁴
```

Priorité des opérateurs (du plus au moins prioritaire) : `()` → `~ ++ -- + - sizeof` (unaires) → `* / %` → `+ -` → `<< >>` → `&` → `^` → `|` → `= += -= ...`

## Voir aussi

[Structures de Contrôle](Structures%20de%20Contr%C3%B4le.md) · [Tableaux](Tableaux.md) · [Fonctions](Fonctions.md)
