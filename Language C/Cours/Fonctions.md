---
tags: [algorithmique, C, ESGI, fonctions]
---

# Fonctions

Lié à : [Variables et Opérateurs](Variables%20et%20Op%C3%A9rateurs.md) · [Structures de Contrôle](Structures%20de%20Contr%C3%B4le.md) · [Tableaux](Tableaux.md)

## Pourquoi des fonctions ?

> [!important] Principe : diviser pour mieux régner
> Un problème trop complexe doit être décomposé en sous-problèmes moins complexes, indépendants les uns des autres et plus simples à résoudre chacun.

> [!important] Définition
> Une **fonction** est un sous-programme : elle possède ses propres **entrées** (paramètres) et sa **sortie** (valeur de retour), et peut être appelée autant de fois que nécessaire — ce qui permet de **réutiliser** du code.

- Les **paramètres** transmettent des données existantes à la fonction.
- La **valeur de retour** permet de récupérer une nouvelle donnée créée par la fonction.
- Certaines fonctions "rendent un service" (transforment des données sans forcément en créer, ex : afficher un [tableau](Tableaux.md)) ; d'autres **retournent une valeur** (ex : calculer une somme).

En C :
- Langage structuré en blocs `{ … }`.
- Seule fonction **obligatoire** : `main()`.
- Pas de notion de "procédure" séparée : tout est fonction.
- Une fonction secondaire peut en appeler une autre, ou **s'appeler elle-même** (récursivité).
- **Pas d'imbrication** : la définition d'une fonction secondaire doit être placée avant ou après `main`, jamais à l'intérieur.

## Définition d'une fonction

```c
type nom_fonction (type1 arg1, ..., typeN argN)   // en-tête (paramètres formels)
{
    // [déclarations de variables locales]
    // liste d'instructions
    return c;   // valeur de retour ; c doit avoir le type de l'en-tête
}
```

- Fonction qui ne retourne rien → type de retour `void`.
- Fonction sans paramètre → remplacer la liste par `void` (ou la laisser vide).

```c
void direBonjour()
{
    printf("Bonjour ! \n");
}
// appel :
direBonjour();
```

```c
int ajouterDeuxNombres(int a, int b)
{
    return a + b;   // retourne la somme
}
// appel :
int somme = ajouterDeuxNombres(5, 7);
```

> [!important] Plusieurs `return`
> On peut avoir plusieurs `return` dans une fonction : le retour au programme appelant est provoqué par le **premier** `return` rencontré lors de l'exécution.

## Appel d'une fonction

`nom_fonction(para1, para2, ..., paraN)`

- Les arguments dans la **définition** = **paramètres formels**.
- Les valeurs passées lors de l'**appel** = **paramètres effectifs** (peuvent être des expressions).
- L'ordre et le type des paramètres effectifs doivent **concorder** avec l'en-tête.
- L'**ordre d'évaluation** des paramètres effectifs n'est pas garanti (dépend du compilateur).

## Prototype (déclaration partielle)

> [!important] Règle
> Au moment de l'appel, la fonction doit être **"connue" du compilateur** — via sa définition complète, ou via un **prototype** placé avant l'appel.

```c
type nom_fonction(type1, ..., typeN);   // prototype — le ';' final est obligatoire
```

```c
#include <stdio.h>

void afficheBonjour();          // déclaration (prototype)

int main() {
    afficheBonjour();           // appel
}

void afficheBonjour() {         // définition (code)
    printf("Bonjour !");
}
```

Les fichiers `.h` de la librairie standard (ex : `math.h`) contiennent des prototypes. Ex : `extern double pow(double, double);` permet au compilateur de convertir automatiquement des paramètres `int` en `double` lors de l'appel à `pow`. **Sans le bon `#include`, cette conversion n'a pas lieu** et l'appel peut produire un résultat faux.

## Portée des variables locales

> [!important] Passage par valeur
> Quand une fonction est appelée, ses **paramètres sont copiés** (passage par valeur). La fonction travaille sur ces **copies** et ne peut donc pas modifier les variables du programme appelant. Cette copie disparaît au retour de la fonction.

```c
void change(int v) {
    v *= 100;
    printf("var change() = %d\n", v);   // 500
}

int main() {
    int var = 5;
    change(var);
    printf("var main() = %d\n", var);   // reste 5 !
    return 0;
}
```

Pour qu'une fonction modifie réellement une variable du programme appelant, il faut lui passer l'**adresse** de cette variable (pointeurs), et non sa valeur.

Une variable déclarée dans une fonction est **locale** : elle n'existe que dans cette fonction, qui n'a **aucune connaissance** des variables d'une autre fonction. Les variables locales n'ont par ailleurs aucun lien avec une variable globale de même nom.

## Fonctions récursives

> [!important] Définition
> Une fonction **récursive** comporte, dans sa définition, **au moins un appel à elle-même**. Il faut toujours prévoir une **condition d'arrêt**, sous peine de récursion infinie.

- Souvent plus simple à comprendre que l'équivalent itératif (boucle).
- Mais **moins performante** à l'exécution : un plus grand nombre d'appels de fonction doit être effectué.

```c
long puissance_rec(int a, int n)      // récursive
{
    if (n == 0) return 1;             // stoppe la récursion
    return (a * puissance_rec(a, n-1));
}

long puissance_iter(int a, int n)     // équivalent itératif
{
    long res = 1;
    for (int i = 0; i < n; i++) res *= a;
    return res;
}
```

```c
long factorielle_rec(int n)
{
    if (n == 1) return 1;
    return n * factorielle_rec(n-1);
}
```

## Voir aussi

[Variables et Opérateurs](Variables%20et%20Op%C3%A9rateurs.md) · [Structures de Contrôle](Structures%20de%20Contr%C3%B4le.md) · [Tableaux](Tableaux.md) · [Algo/Les Fonctions.md](../../Algo/Cours/Les%20Fonctions.md)