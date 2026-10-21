---
tags: [algorithmique, C, ESGI, types, conversions]
---

# Types et Conversions

Fait suite à : [Variables et Opérateurs](Variables%20et%20Op%C3%A9rateurs.md) · Suite : [Entrées Sorties](Entr%C3%A9es%20Sorties.md)

## Tailles : elles dépendent de la plateforme

> [!important] Précision par rapport au cours de base
> Le C ne fixe que des **minimums** : `char` = 1 octet, `short` ≥ 2, `int` ≥ 2, `long` ≥ 4, `long long` ≥ 8. Sur un PC actuel : `int` = **4 octets**, et `long` = 4 octets sous Windows mais **8 octets** sous Linux/Mac 64 bits. Le tableau « `int` = 2 octets » correspond à d'anciens compilateurs 16 bits. **Pour connaître la taille réelle : `sizeof`.**

```c
#include <stdio.h>
#include <limits.h>

int main(void)
{
    printf("char      : %zu octet(s)\n", sizeof(char));
    printf("short     : %zu\n", sizeof(short));
    printf("int       : %zu\n", sizeof(int));
    printf("long      : %zu\n", sizeof(long));
    printf("long long : %zu\n", sizeof(long long));
    printf("float     : %zu\n", sizeof(float));
    printf("double    : %zu\n", sizeof(double));
    printf("INT_MAX   = %d\n", INT_MAX);
    return 0;
}
```
`sizeof` renvoie un `size_t` : on l'affiche avec **`%zu`**.

## Types à taille fixe : `<stdint.h>`

Quand la taille compte (protocoles, fichiers binaires, bits), on utilise des types **portables** :

| Type | Taille | Plage |
|---|---|---|
| `int8_t` / `uint8_t` | 8 bits | −128…127 / 0…255 |
| `int16_t` / `uint16_t` | 16 bits | −32 768…32 767 / 0…65 535 |
| `int32_t` / `uint32_t` | 32 bits | ±2,1×10⁹ / 0…4,29×10⁹ |
| `int64_t` / `uint64_t` | 64 bits | ±9,2×10¹⁸ / 0…1,8×10¹⁹ |
| `size_t` | taille d'un objet en mémoire | non signé (`%zu`) |

`<limits.h>` : `INT_MAX`, `INT_MIN`, `UINT_MAX`, `LONG_MAX`… · `<float.h>` : `FLT_EPSILON`, `DBL_MAX`…

## Booléens : `<stdbool.h>`

```c
#include <stdbool.h>
bool trie = true;      // true = 1, false = 0
```
Sans cet en-tête (C89), on utilise un `int` (0 = faux, tout le reste = vrai). Le tri à bulles de [Tableaux](Tableaux.md) nécessite `#include <stdbool.h>`.

## Dépassement de capacité (overflow)

> [!important] Règle
> - **Entiers non signés** : le dépassement « boucle » (modulo 2ⁿ) : `unsigned char c = 255; c++;` donne `0`.
> - **Entiers signés** : le dépassement est un **comportement indéfini** (le programme peut faire n'importe quoi).

```c
int x = INT_MAX;
x = x + 1;          // ❌ comportement indéfini
```
Prévoir un type plus grand (`long long`) quand les valeurs peuvent grossir (factorielle, puissances : voir [1ESGI/Language C/Exo/Corrigé Exercices Cours 1-2](../Exo/Corrig%C3%A9%20Exercices%20Cours%201-2.md), exercice 11).

## Nombres à virgule

| Type | Taille | Précision | Format `printf` / `scanf` |
|---|---|---|---|
| `float` | 4 octets | ~7 chiffres | `%f` / `%f` |
| `double` | 8 octets | ~15 chiffres | `%f` / **`%lf`** |

Les réels sont **approchés** (représentation binaire) :
```c
double x = 0.1 + 0.2;
printf("%d\n", x == 0.3);          // 0 : FAUX !
```
> [!important] Ne jamais comparer deux réels avec `==`
> On compare avec une **tolérance** : `fabs(a - b) < 1e-9` (`<math.h>`).

## Conversions

### Implicites
Dans une expression, le C convertit automatiquement vers le « plus grand » type :
`char`/`short` → `int` → `unsigned` → `long` → `float` → `double`.

```c
int a = 7, b = 2;
printf("%d\n", a / b);              // 3    division entière
printf("%f\n", a / b);              // ❌ comportement indéfini (mauvais format)
printf("%f\n", (float)a / b);       // 3.500000
printf("%f\n", (float)(a / b));     // 3.000000 (la division a déjà eu lieu !)
```

### Explicites (cast) : `(type) expression`
Le cast s'applique à **l'opérande qui le suit**, pas à toute l'expression.

> [!important] Piège signé / non signé
> `-1 < 1u` est **faux** : `-1` est converti en `unsigned` (très grand nombre). Éviter de mélanger les deux.

## Caractères et ASCII

Un `char` est un **petit entier** : `'A'` vaut 65, `'a'` vaut 97, `'0'` vaut 48.
```c
char c = 'a';
printf("%c %d\n", c, c);            // a 97
char maj = c - 'a' + 'A';           // 'A'  (ou toupper(c))
int chiffre = '7' - '0';            // 7
```
`<ctype.h>` : `isdigit`, `isalpha`, `isspace`, `toupper`, `tolower`.
Séquences d'échappement : `\n` (retour ligne), `\t` (tabulation), `\\`, `\"`, `\'`, `\0` (fin de chaîne).

## Constantes

```c
const double PI = 3.14159265;       // variable en lecture seule (vérifiée par le compilateur)
#define TAILLE 100                  // substitution du préprocesseur (pas de type)
enum { NB_MAX = 50 };               // constante entière
```
Un `const` ne peut pas servir de taille à un tableau en C89 ; `#define` ou `enum` conviennent partout.

## Exercices

1. Que valent `7/2`, `7%2`, `7/2.0`, `(float)7/2`, `(float)(7/2)` ?
2. Écris un programme qui affiche `sizeof` de `char`, `int`, `long`, `double`.
3. Pourquoi `if (0.1 + 0.2 == 0.3)` est-il faux ? Corrige-le avec `fabs`.

<details><summary>Corrigés</summary>

1. `3`, `1`, `3.5`, `3.5`, `3.0`.
2. Voir le programme plus haut.
3. Représentation binaire approchée. `if (fabs(0.1 + 0.2 - 0.3) < 1e-9)`.
</details>

## Voir aussi

[Variables et Opérateurs](Variables%20et%20Op%C3%A9rateurs.md) · [Entrées Sorties](Entr%C3%A9es%20Sorties.md) · [Bits et Masques](Bits%20et%20Masques.md)
