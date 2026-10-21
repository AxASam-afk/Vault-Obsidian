---
tags: [algorithmique, C, ESGI, corrections, erratum]
---

# Corrections et précisions sur les notes de base

Relecture technique des notes [Variables et Opérateurs](Variables%20et%20Op%C3%A9rateurs.md), [Structures de Contrôle](Structures%20de%20Contr%C3%B4le.md), [Tableaux](Tableaux.md), [Fonctions](Fonctions.md) et [1ESGI/Language C/Exo/Corrigé Exercices Cours 1-2](../Exo/Corrig%C3%A9%20Exercices%20Cours%201-2.md). Ces notes sont bonnes ; voici les **points à rectifier ou à nuancer**. Chaque ligne indique où corriger.

| # | Note | Texte actuel | Correction |
|---|---|---|---|
| 1 | [Variables et Opérateurs](Variables%20et%20Op%C3%A9rateurs.md) | `0x11010010 = 422 (déc)` | Ce nombre est en **binaire** : `0b11010010 = 210`. (Un `0x11010010` hexadécimal vaudrait 285 278 224.) |
| 2 | [Variables et Opérateurs](Variables%20et%20Op%C3%A9rateurs.md) | `unsigned long` : « 0 à 4 294 967 296 » | Maximum = **4 294 967 295** (2³² − 1). |
| 3 | [Variables et Opérateurs](Variables%20et%20Op%C3%A9rateurs.md) | `sizeof(int)` → 2 octets | Sur PC actuel : **4 octets** (2 sur d'anciens compilateurs 16 bits). Voir [Types et Conversions](Types%20et%20Conversions.md). |
| 4 | [Variables et Opérateurs](Variables%20et%20Op%C3%A9rateurs.md) | `long` = 4 octets | Vrai sous Windows ; **8 octets** sous Linux/Mac 64 bits. Utiliser `<stdint.h>` si la taille compte. |
| 5 | [Variables et Opérateurs](Variables%20et%20Op%C3%A9rateurs.md) | « Denis Ritchie » | **Dennis** Ritchie. |
| 6 | [Variables et Opérateurs](Variables%20et%20Op%C3%A9rateurs.md) | « doit commencer par une lettre » | Peut aussi commencer par `_` (`_compteur`), mais pas par un chiffre. |
| 7 | [Variables et Opérateurs](Variables%20et%20Op%C3%A9rateurs.md) | « on ne peut initialiser qu'avec des constantes » | Vrai pour les variables **globales/static**. Pour une variable locale : `int b = a * 2;` est permis. |
| 8 | [Variables et Opérateurs](Variables%20et%20Op%C3%A9rateurs.md) | `%f` « float / double » | `printf("%f")` convient aux deux ; `scanf` exige `%f` (float) et **`%lf`** (double). |
| 9 | [Variables et Opérateurs](Variables%20et%20Op%C3%A9rateurs.md) | priorités | La liste omet les comparaisons et logiques : `* / %` > `+ -` > `<< >>` > `< <= > >=` > `== !=` > `&` > `^` > `\|` > `&&` > `\|\|` > `? :` > `=`. En cas de doute : parenthèses. |
| 10 | [Variables et Opérateurs](Variables%20et%20Op%C3%A9rateurs.md) | `int main()` | Forme recommandée : `int main(void)`. |
| 11 | [Tableaux](Tableaux.md) | « les composantes restantes contiennent une valeur indéterminée » | Avec une initialisation partielle (`int t[5] = {1,2};`), les composantes restantes valent **0**. Sans initialisation du tout (tableau local), elles sont indéterminées. |
| 12 | [Tableaux](Tableaux.md) | `scanf("%19s", &prenom);` | `prenom` est déjà une adresse : **`scanf("%19s", prenom);`** (sans `&`). |
| 13 | [Tableaux](Tableaux.md) | tri à bulles avec `bool` | Ajouter `#include <stdbool.h>`. |
| 14 | [Fonctions](Fonctions.md) | `factorielle_rec` : `if (n == 1)` | Préférer `if (n <= 1)` : `factorielle_rec(0)` ne s'arrête jamais avec `n == 1`. |
| 15 | [Fonctions](Fonctions.md) | « tableaux (pointeurs) » à voir plus tard | Le passage d'un tableau à une fonction transmet son **adresse** : voir [Pointeurs et Tableaux](Pointeurs%20et%20Tableaux.md). |
| 16 | [Structures de Contrôle](Structures%20de%20Contr%C3%B4le.md) | boucle `for` de 1 à 1 000 000 | Exemple valide ; penser à `<=` vs `<` (erreur *off-by-one* fréquente, voir [Débogage et Bonnes Pratiques](D%C3%A9bogage%20et%20Bonnes%20Pratiques.md)). |
| 17 | [1ESGI/Language C/Exo/Corrigé Exercices Cours 1-2](../Exo/Corrig%C3%A9%20Exercices%20Cours%201-2.md) | ex. 11 : `long resultat` | `x^n` déborde vite (`long` peut faire 4 octets) : prévoir `long long` et un contrôle. |
| 18 | [1ESGI/Language C/Exo/Corrigé Exercices Cours 1-2](../Exo/Corrig%C3%A9%20Exercices%20Cours%201-2.md) | ex. 3, 10 : `scanf("%f")` + `printf("%f")` | Correct. Pour des `double`, utiliser `%lf` en saisie. |

> [!important] Lecture
> Les corrections 1 à 5 sont des **erreurs factuelles** ; les autres sont des **précisions** pour éviter des malentendus ou des bugs sur d'autres plateformes.

## Voir aussi

[1ESGI/Language C/C](../C.md) · [Types et Conversions](Types%20et%20Conversions.md) · [Aide-mémoire C](../Aide-m%C3%A9moire%20C.md)
