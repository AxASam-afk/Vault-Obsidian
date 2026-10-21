---
tags: [algorithmique, C, ESGI, structures-controle]
---

# Structures de Contrôle

Fait suite à : [Variables et Opérateurs](Variables%20et%20Op%C3%A9rateurs.md)

## 1. Structures conditionnelles

### if … else

```c
if (condition)
    Instruction à exécuter si la condition est vraie
else                        // else n'est pas obligatoire
    Instruction à exécuter si la condition est fausse
```

```c
if (a == 10)
    printf("a vaut 10");
else
    printf("a est différent de 10");
```

### Opérateur ternaire

`condition ? valeur_si_vrai : valeur_si_faux`

```c
short result;
result = (a == 10 ? 100 : 200);   // équivalent compact du if/else ci-dessous
```

> [!important] Équivalence
> ```c
> if (a == 10) result = 100;
> else         result = 200;
> ```
> est strictement équivalent à `result = (a == 10 ? 100 : 200);`

### switch

Compare une variable à plusieurs valeurs **entières**.

```c
switch (choix)
{
    case 1:
        printf("Option 1\n");
        break;
    case 2:
        printf("Option 2\n");
        break;
    default:               // optionnel
        printf("Option invalide\n");
        break;
}
```

> [!important] Ne pas oublier `break`
> Sans `break`, l'exécution continue ("tombe") dans le `case` suivant au lieu de s'arrêter.

## 2. Structures répétitives (boucles, itérations)

Les boucles permettent de **répéter plusieurs fois** les mêmes instructions. Trois types : `for`, `while`, `do … while`.

### for

Répète une liste d'instructions un **nombre défini** de fois.

```c
for (initialisation ; test ; incrémentation)
{
    Liste d'instructions à exécuter.
}
```

```c
int i;
for (i = 1; i <= 1000000; i++)
{
    printf("Bonjour \n");
}
```

`for ( ; ; )` sans paramètres = boucle infinie volontaire.

### while

Répète les mêmes instructions **tant qu'une condition est vraie** — utile quand on ne connaît pas le nombre d'itérations à l'avance (ex : demander un mot de passe jusqu'à ce qu'il soit correct).

> [!important] Attention
> Il faut être certain que l'expression booléenne puisse devenir fausse à un moment donné, sous peine de rester "coincé" dans une boucle infinie.

```c
while (condition)
{
    // bloc d'instructions à répéter
}
```

```c
char reponse = 'n';
while (reponse != 'o')
{
    printf("Avez-vous compris ? ");
    scanf(" %c", &reponse);
}
```

### do … while

Identique à `while`, sauf que le bloc est **exécuté au moins une fois**.

```c
do
{
    // instructions à exécuter tant que la condition est vraie
} while (condition);
```

```c
int compteur = 0;
do
{
    printf("Bienvenue !\n");
    compteur++;
} while (compteur < 10);
```

### Boucles imbriquées

Une boucle peut en contenir une autre (typiquement pour parcourir un [tableau](Tableaux.md) à deux dimensions, ligne par ligne puis colonne par colonne).

```c
for (i = 1; i <= 5; i++) {
    for (j = 1; j <= 10; j++) {
        printf("*");
    }
    printf("\n");
}
```

### continue et break

> [!important] continue
> Passe directement à l'**itération suivante**, sans exécuter la fin de la boucle pour ce tour-ci.

```c
for (a = 0; a <= 5; a++)
{
    if (a == 3)
        continue;        // le "printf" ci-dessous est sauté quand a == 3
    printf("a=%d\n", a);
}
```

> [!important] break
> Met fin **complètement** au parcours de la boucle, dès qu'il est rencontré.

```c
for (a = 0; a <= 5; a++)
{
    if (a == 3)
        break;            // arrête la boucle ; n'affiche que 0, 1, 2
    printf("a=%d\n", a);
}
```

⚠️ `continue` et `break` rompent la logique naturelle de la boucle et rendent le code plus difficile à suivre : à éviter autant que possible.

## Voir aussi

[Variables et Opérateurs](Variables%20et%20Op%C3%A9rateurs.md) · [Tableaux](Tableaux.md) · [Fonctions](Fonctions.md) · [Algo/Les boucles.md](../../Algo/Cours/Les%20boucles.md)
