---
tags: [algorithmique, C, ESGI, compilation]
---

# Chaîne de compilation

Fait suite à : [Variables et Opérateurs](Variables%20et%20Op%C3%A9rateurs.md) · Suite : [Types et Conversions](Types%20et%20Conversions.md)

## De `.c` à l'exécutable

> [!important] Les 4 étapes
> Un programme C passe par **quatre étapes** avant de s'exécuter. `gcc` les enchaîne automatiquement, mais on peut les lancer une par une pour comprendre ce qui se passe.

```text
main.c ──► [1 Préprocesseur] ──► main.i ──► [2 Compilation] ──► main.s
                                                                   │
exécutable ◄── [4 Édition de liens] ◄── main.o ◄── [3 Assemblage] ◄┘
                      ▲
              bibliothèques (libc, libm…)
```

| Étape | Rôle | Commande | Résultat |
|---|---|---|---|
| 1. **Préprocesseur** | traite les lignes `#` : `#include` (copie les en-têtes), `#define` (remplacement), `#ifdef` | `gcc -E main.c -o main.i` | code C « étendu » |
| 2. **Compilation** | traduit le C en **assembleur** (vérifie la syntaxe et les types) | `gcc -S main.c -o main.s` | `main.s` |
| 3. **Assemblage** | traduit l'assembleur en **code machine** | `gcc -c main.c -o main.o` | fichier objet `.o` |
| 4. **Édition de liens** (*link*) | assemble les `.o` et les bibliothèques en un seul exécutable | `gcc main.o -o main` | exécutable |

## Commande de compilation recommandée

```bash
gcc -Wall -Wextra -Wpedantic -std=c17 -g main.c -o main -lm
./main            # Linux / Mac      (Windows : main.exe)
```

| Option | Rôle |
|---|---|
| `-Wall -Wextra -Wpedantic` | active **les avertissements** (à toujours utiliser) |
| `-std=c17` | impose la norme C17 (ou `c11`, `c99`) |
| `-g` | ajoute les infos de débogage (pour `gdb`, `valgrind`) |
| `-O0` / `-O2` | pas d'optimisation (debug) / optimisation (production) |
| `-o nom` | nom de l'exécutable |
| `-lm` | lie la bibliothèque mathématique (`sqrt`, `pow`…) |
| `-c` | compile sans lier (produit un `.o`) |

> [!important] Un warning est un bug en attente
> Le compilateur signale des erreurs **probables** (variable non initialisée, mauvais format dans `printf`…). **Ne jamais ignorer un warning** : on compile « propre », sans aucun avertissement.

## Les 4 familles d'erreurs

| Type | Quand ? | Exemple de message | Cause typique |
|---|---|---|---|
| **Syntaxe** | compilation | `error: expected ';' before ...` | `;` oublié, accolade manquante |
| **Typage / déclaration** | compilation | `error: 'x' undeclared` | variable non déclarée, mauvais `#include` |
| **Édition de liens** | link | `undefined reference to 'sqrt'` | oubli de `-lm`, fonction jamais définie |
| **Exécution** (*runtime*) | exécution | `Segmentation fault` | pointeur invalide, débordement de tableau, division par 0 |
| **Logique** | exécution | résultat faux, sans message | erreur d'algorithme (aucun outil ne la détecte automatiquement) |

## Signature de `main`

```c
int main(void)                          // sans arguments (forme recommandée)
int main(int argc, char *argv[])        // avec arguments de la ligne de commande
```
La valeur retournée est le **code de sortie** : `0` = succès, autre valeur = erreur (`EXIT_SUCCESS` / `EXIT_FAILURE` dans `<stdlib.h>`). Sous Linux : `echo $?` affiche ce code.

## Voir aussi

[Variables et Opérateurs](Variables%20et%20Op%C3%A9rateurs.md) · [Préprocesseur et Projets multi-fichiers](Pr%C3%A9processeur%20et%20Projets%20multi-fichiers.md) · [Débogage et Bonnes Pratiques](D%C3%A9bogage%20et%20Bonnes%20Pratiques.md)
