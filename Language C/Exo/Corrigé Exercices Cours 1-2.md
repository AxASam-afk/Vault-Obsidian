---
tags: [algorithmique, C, ESGI, exercices, corrige]
---

# Corrigé — Exercices Cours 1 & 2

Lié à : [Variables et Opérateurs](../Cours/Variables%20et%20Op%C3%A9rateurs.md) · [Structures de Contrôle](../Cours/Structures%20de%20Contr%C3%B4le.md) · [Fonctions](../Cours/Fonctions.md)

> [!important] Avant de commencer
> Pour compiler un fichier `exoX.c` en ligne de commande : `gcc exoX.c -o exoX` puis `./exoX` (Linux/Mac) ou `exoX.exe` (Windows). Sous CodeBlocks : F9 compile et exécute directement.
> Pour les exercices utilisant `<math.h>` (ex. 10), il faut **lier la librairie mathématique** à la compilation : `gcc exo10.c -o exo10 -lm`.

---

## Exercice 1 — Afficher "Bonjour !"

Le programme le plus simple possible : un seul `printf`.

```c
#include <stdio.h>

int main()
{
    printf("Bonjour !\n");
    return 0;
}
```

> [!important] Rappel
> `\n` déclenche un retour à la ligne. `return 0;` signale au système que le programme s'est terminé sans erreur (voir [Variables et Opérateurs](../Cours/Variables%20et%20Op%C3%A9rateurs.md)).

---

## Exercice 2 — Somme de deux entiers

```c
#include <stdio.h>

int main()
{
    int a, b;

    printf("Entrez un premier entier : ");
    scanf("%d", &a);
    printf("Entrez un second entier : ");
    scanf("%d", &b);

    printf("La somme est : %d\n", a + b);

    return 0;
}
```

> [!important] Pourquoi `&a` et pas `a` ?
> `scanf` a besoin de **l'adresse mémoire** de la variable pour pouvoir écrire la valeur saisie dedans — pas sa valeur (qui, avant la saisie, n'existe pas encore vraiment). L'opérateur `&` ("adresse de") sert exactement à ça. On retrouvera cette logique d'adresse plus tard avec le passage par référence dans les [Fonctions](../Cours/Fonctions.md).

---

## Exercice 3 — Quotient de deux réels

```c
#include <stdio.h>

int main()
{
    float a, b;

    printf("Entrez un premier reel : ");
    scanf("%f", &a);
    printf("Entrez un second reel : ");
    scanf("%f", &b);

    if (b != 0)
        printf("Le quotient est : %f\n", a / b);
    else
        printf("Division par zero impossible !\n");

    return 0;
}
```

> [!important] int vs float pour une division
> Si `a` et `b` étaient des `int`, `a / b` ferait une **division entière** (partie décimale tronquée). Avec `float`, on obtient bien un résultat à virgule. Le test `b != 0` n'était pas demandé mais évite un comportement indéfini si l'utilisateur saisit 0 — bon réflexe à prendre.

---

## Exercice 4 — Cercle : diamètre, périmètre, surface

```c
#include <stdio.h>

int main()
{
    const float Pi = 3.14159;
    float R = 20;
    float D, P, S;

    D = 2 * R;
    P = 2 * Pi * R;
    S = Pi * R * R;

    printf("Diametre : %f\n", D);
    printf("Perimetre : %f\n", P);
    printf("Surface   : %f\n", S);

    return 0;
}
```

> [!important] Le mot-clé `const`
> `const float Pi = 3.14159;` déclare une variable dont la valeur ne doit **jamais changer** après son initialisation. Toute tentative d'écrire `Pi = 3;` plus loin dans le code provoquerait une **erreur de compilation** — utile pour éviter de modifier une constante par erreur.

---

## Exercice 5 — Âge et message

```c
#include <stdio.h>

int main()
{
    int age;

    printf("Quel est votre age ? ");
    scanf("%d", &age);

    if (age >= 18)
        printf("Vous etes majeur.\n");
    else if (age > 6)
        printf("Vous avez plus de 6 ans, mais vous etes mineur.\n");
    else
        printf("Vous avez 6 ans ou moins.\n");

    return 0;
}
```

> [!important] L'ordre des `else if` compte
> Les conditions sont testées **du haut vers le bas**, et le programme s'arrête à la première qui est vraie. Ici, si `age >= 18` est vrai, les branches suivantes ne sont même pas évaluées — c'est pour ça qu'on doit tester le cas le plus "restrictif" (majeur) en premier, sans avoir besoin d'écrire `age >= 18 && age > 6` : c'est implicite dans l'ordre du `if / else if / else` (voir [Structures de Contrôle](../Cours/Structures%20de%20Contr%C3%B4le.md)).

---

## Exercice 6 — La plus grande de 3 valeurs

```c
#include <stdio.h>

int main()
{
    int a, b, c, max;

    printf("Entrez 3 entiers : ");
    scanf("%d %d %d", &a, &b, &c);

    max = a;
    if (b > max)
        max = b;
    if (c > max)
        max = c;

    printf("Le plus grand est : %d\n", max);

    return 0;
}
```

> [!important] Éviter la combinatoire
> On pourrait être tenté d'écrire un `if` avec toutes les combinaisons (`a>b && a>c`, `b>a && b>c`, ...) — ça fonctionne mais devient vite illisible à mesure que le nombre de valeurs augmente. La technique du **"max courant"** (on part d'une hypothèse, puis on la corrige à chaque nouvelle valeur trouvée plus grande) est bien plus propre, et c'est celle qu'on réutilise pour trouver un maximum dans un [tableau](../Cours/Tableaux.md).

---

## Exercice 7 — Pair ou impair

```c
#include <stdio.h>

int main()
{
    int n;

    printf("Entrez un entier : ");
    scanf("%d", &n);

    if (n % 2 == 0)
        printf("%d est pair.\n", n);
    else
        printf("%d est impair.\n", n);

    return 0;
}
```

> [!important] Le modulo `%`
> `n % 2` renvoie le **reste** de la division entière de `n` par 2 : soit 0 (nombre pair), soit 1 (nombre impair). C'est le test standard de parité en C.

---

## Exercice 8 — Triangle valide

```c
#include <stdio.h>

int main()
{
    float a, b, c;

    printf("Entrez les 3 cotes du triangle : ");
    scanf("%f %f %f", &a, &b, &c);

    if ((a + b > c) && (a + c > b) && (b + c > a))
        printf("Triangle valide.\n");
    else
        printf("Triangle invalide.\n");

    return 0;
}
```

> [!important] Pourquoi les 3 conditions avec `&&` ?
> L'énoncé demande que "la somme de deux côtés soit toujours supérieure au troisième" — le mot "toujours" implique qu'il faut vérifier l'inégalité **dans les 3 sens possibles** (on ne sait pas d'avance quel côté est le plus grand). D'où les 3 comparaisons reliées par `&&` (ET) : les **trois** doivent être vraies simultanément.

---

## Exercice 9 — Activité sportive

```c
#include <stdio.h>

int main()
{
    int age, accompagne;

    printf("Quel est votre age ? ");
    scanf("%d", &age);
    printf("Etes-vous accompagne d'un adulte ? (1 = oui, 0 = non) ");
    scanf("%d", &accompagne);

    if (age >= 18 || accompagne == 1)
        printf("Vous pouvez participer a l'activite.\n");
    else
        printf("Vous ne pouvez pas participer a l'activite.\n");

    return 0;
}
```

> [!important] `&&` vs `||`
> Contrairement à l'exercice 8, ici une **seule** des deux conditions suffit ("soit... soit...") : on utilise donc `||` (OU) et non `&&` (ET). Bon réflexe pour choisir l'opérateur : traduire l'énoncé mot à mot ("et" → `&&`, "ou" → `||`).

---

## Exercice 10 — Équation du second degré

```c
#include <stdio.h>
#include <math.h>

int main()
{
    float a, b, c, delta, x1, x2;

    printf("Entrez a, b et c : ");
    scanf("%f %f %f", &a, &b, &c);

    delta = b * b - 4 * a * c;

    if (delta > 0)
    {
        x1 = (-b + sqrt(delta)) / (2 * a);
        x2 = (-b - sqrt(delta)) / (2 * a);
        printf("Deux solutions : x1 = %f, x2 = %f\n", x1, x2);
    }
    else if (delta == 0)
    {
        x1 = -b / (2 * a);
        printf("Une solution double : x = %f\n", x1);
    }
    else
    {
        printf("Aucune solution reelle.\n");
    }

    return 0;
}
```

> [!important] Correction par rapport à l'énoncé
> L'énoncé du sujet contient une coquille : il indique "Si Δ = 0, pas de solution", ce qui est **mathématiquement faux**. La vraie règle est :
> - **Δ > 0** → deux solutions distinctes : x1 = (-b + √Δ)/2a, x2 = (-b - √Δ)/2a
> - **Δ = 0** → une solution double : x = -b/2a
> - **Δ < 0** → aucune solution réelle
>
> C'est cette version correcte qui est implémentée ci-dessus. Ne pas oublier `#include <math.h>` pour `sqrt()`, et de compiler avec `-lm`.

---

## Exercice 11 — Puissance x^n

```c
#include <stdio.h>

int main()
{
    int x, n, i;
    long resultat = 1;

    printf("Entrez x : ");
    scanf("%d", &x);
    printf("Entrez n : ");
    scanf("%d", &n);

    for (i = 0; i < n; i++)
        resultat *= x;

    printf("%d ^ %d = %ld\n", x, n, resultat);

    return 0;
}
```

> [!important] Un accumulateur
> `resultat` part de **1** (élément neutre de la multiplication) et se fait multiplier par `x` à chaque tour de boucle, `n` fois. C'est le schéma classique de l'**accumulateur** dans une boucle `for` (voir [Structures de Contrôle](../Cours/Structures%20de%20Contr%C3%B4le.md)). Une fois le chapitre sur les [Fonctions](../Cours/Fonctions.md) vu, cet algorithme correspond exactement à `puissance_iter()` du cours — sa version récursive, `puissance_rec()`, y est également détaillée.

---

## Exercice 12 — Multiplication par additions successives

```c
#include <stdio.h>

int main()
{
    int a, b, i;
    long produit = 0;

    printf("Entrez deux entiers positifs : ");
    scanf("%d %d", &a, &b);

    for (i = 0; i < b; i++)
        produit += a;

    printf("%d x %d = %ld\n", a, b, produit);

    return 0;
}
```

> [!important] Même logique que l'exercice 11
> On retrouve exactement le même schéma d'accumulateur que pour la puissance, sauf qu'on **additionne** `a` au lieu de le **multiplier** : `a × b`, c'est `a` additionné à lui-même `b` fois, tout comme `x^n` c'est `x` multiplié à lui-même `n` fois. Repérer ces schémas répétitifs (le "pattern" de l'accumulateur) aide à écrire des boucles rapidement.

---

## Exercice 13 — Division entière par soustractions successives

```c
#include <stdio.h>

int main()
{
    int a, b, grand, petit, quotient = 0, reste;

    printf("Entrez deux entiers positifs a et b : ");
    scanf("%d %d", &a, &b);

    grand = (a > b) ? a : b;
    petit = (a > b) ? b : a;

    reste = grand;
    while (reste >= petit)
    {
        reste -= petit;
        quotient++;
    }

    printf("Quotient = %d, Reste = %d\n", quotient, reste);

    return 0;
}
```

> [!important] Pourquoi une boucle `while` et non `for` ?
> On ne connaît **pas à l'avance** le nombre de soustractions nécessaires (ça dépend des valeurs saisies) : c'est exactement le cas d'usage du `while`, "tant que le reste est encore assez grand pour qu'on puisse retrancher `petit`" (voir [Structures de Contrôle](../Cours/Structures%20de%20Contr%C3%B4le.md)). L'opérateur **ternaire** `(a > b) ? a : b` (vu dans [Variables et Opérateurs](../Cours/Variables%20et%20Op%C3%A9rateurs.md)) permet de déterminer `grand`/`petit` en une ligne, sans passer par un `if/else`.

---

## Exercice 14 — Conversion °F → °C (for, while, do…while)

> [!important] Formule utilisée
> L'énoncé donne `°C = 0,55 × (°F − 32)`. C'est une version arrondie de la formule exacte `°C = (5/9) × (°F − 32) ≈ 0,5556 × (°F − 32)` — on utilise volontairement 0,55 pour rester fidèle à l'énoncé.

### Version `for`

```c
#include <stdio.h>

int main()
{
    float f, c;

    for (f = 0; f <= 300; f += 10)
    {
        c = 0.55 * (f - 32);
        printf("%.0f F = %.2f C\n", f, c);
    }

    return 0;
}
```

### Version `while`

```c
#include <stdio.h>

int main()
{
    float f = 0, c;

    while (f <= 300)
    {
        c = 0.55 * (f - 32);
        printf("%.0f F = %.2f C\n", f, c);
        f += 10;
    }

    return 0;
}
```

### Version `do…while`

```c
#include <stdio.h>

int main()
{
    float f = 0, c;

    do
    {
        c = 0.55 * (f - 32);
        printf("%.0f F = %.2f C\n", f, c);
        f += 10;
    } while (f <= 300);

    return 0;
}
```

> [!important] Comparer les 3 boucles
> Les trois versions produisent exactement le **même résultat**, mais illustrent bien la différence de structure :
> - `for` : initialisation, test et incrémentation sont regroupés sur une seule ligne — on voit d'un coup d'œil les bornes de la boucle. À privilégier quand le nombre d'itérations est connu à l'avance (ici : de 0 à 300 par pas de 10).
> - `while` : il faut penser à écrire `f += 10;` **soi-même** à l'intérieur du bloc — si on l'oublie, boucle infinie (voir l'avertissement dans [Structures de Contrôle](../Cours/Structures%20de%20Contr%C3%B4le.md)).
> - `do…while` : ici le résultat est identique au `while` car la condition (`f <= 300`) est de toute façon vraie au premier passage ; la différence ne se verrait que si la condition de départ était déjà fausse (le bloc s'exécuterait quand même une fois).

---

## Voir aussi

[Variables et Opérateurs](../Cours/Variables%20et%20Op%C3%A9rateurs.md) · [Structures de Contrôle](../Cours/Structures%20de%20Contr%C3%B4le.md) · [Tableaux](../Cours/Tableaux.md) · [Fonctions](../Cours/Fonctions.md)
