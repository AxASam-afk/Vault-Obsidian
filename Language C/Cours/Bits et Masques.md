---
tags: [algorithmique, C, ESGI, bits, masques]
---

# Bits et Masques

Fait suite à : [Variables et Opérateurs](Variables%20et%20Op%C3%A9rateurs.md) (opérateurs bit à bit)

## Rappel des opérateurs

| Opérateur | Rôle | Exemple (sur 8 bits) |
|---|---|---|
| `&` | ET | `1100 & 1010 = 1000` |
| `\|` | OU | `1100 \| 1010 = 1110` |
| `^` | XOR | `1100 ^ 1010 = 0110` |
| `~` | NON | `~1100 = ...0011` |
| `<<` n | décalage gauche = × 2ⁿ | `0001 << 3 = 1000` |
| `>>` n | décalage droite = ÷ 2ⁿ | `1000 >> 2 = 0010` |

Pour manipuler des bits, utiliser des types **non signés** (`unsigned`, `uint8_t`).

## Les 4 opérations de base sur le bit n°`n`

```c
x |=  (1u << n);          // mettre à 1   (set)
x &= ~(1u << n);          // mettre à 0   (clear)
x ^=  (1u << n);          // inverser     (toggle)
int b = (x >> n) & 1u;    // lire le bit  (test)
```
Le bit 0 est le bit de **poids faible** (à droite).

## Exemple : drapeaux de permissions (comme `rwx` sous Linux)

```c
#include <stdio.h>
#include <stdint.h>

#define LECTURE   4u     /* 100 */
#define ECRITURE  2u     /* 010 */
#define EXECUTION 1u     /* 001 */

static void afficherBinaire(uint8_t v)
{
    for (int i = 7; i >= 0; i--)
        putchar(((v >> i) & 1u) ? '1' : '0');
    putchar('\n');
}

int main(void)
{
    uint8_t droits = 0;

    droits |= LECTURE | ECRITURE;       // rw-
    afficherBinaire(droits);            // 00000110

    if (droits & ECRITURE)
        printf("Ecriture autorisee\n");

    droits &= ~ECRITURE;                // retire l'écriture
    droits ^= EXECUTION;                // bascule l'exécution
    afficherBinaire(droits);            // 00000101

    return 0;
}
```
`chmod 754` sous Linux (voir le cours d'infrastructure) fonctionne exactement ainsi : 4 + 2 + 1.

## Astuces classiques

```c
n & 1               // 1 si n est impair
(n & (n - 1)) == 0  // vrai si n est une puissance de 2 (n > 0)
a ^= b; b ^= a; a ^= b;   // échange sans variable temporaire (curiosité, peu lisible)
```

## Exercices

1. Écris `afficherBinaire` pour un `unsigned int` de 32 bits.
2. Compte le nombre de bits à 1 dans un entier.
3. Teste si `64`, `100`, `1024` sont des puissances de 2.

<details><summary>Corrigé de 2</summary>

```c
int compterBits(unsigned int x)
{
    int c = 0;
    while (x) { c += x & 1u; x >>= 1; }
    return c;
}
```
</details>

## Voir aussi

[Variables et Opérateurs](Variables%20et%20Op%C3%A9rateurs.md) · [Types et Conversions](Types%20et%20Conversions.md)
