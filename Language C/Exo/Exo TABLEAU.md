

## title: Cours - Les tableaux en langage C tags: [C, algorithmique, tableaux, cours, exercices, L1]

# Cours : les tableaux en langage C

> [!abstract] Niveau Cours niveau **première année**. Le code reste simple : une instruction par ligne, des noms de variables parlants, et seulement les notions vues en début d'année (`for`, `while`, `if`, `scanf`, `printf`).

> [!abstract] Sommaire
> 
> - [Rappels](#Rappels)
> - [Exercice 1](#Exercice%201)
> - [Exercice 2](#Exercice%202)
> - [Exercice 3](#Exercice%203)
> - [Exercice 4](#Exercice%204)
> - [Exercice 5](#Exercice%205)
> - [Exercice 6](#Exercice%206)
> - [Exercice 7](#Exercice%207)
> - [Exercice 8](#Exercice%208)
> - [Exercice 9](#Exercice%209)
> - [Exercice 10](#Exercice%2010)
> - [Exercice 11](#Exercice%2011)
> - [Exercice 12](#Exercice%2012)
> - [Exercice 13](#Exercice%2013)
> - [Exercice 14](#Exercice%2014)
> - [Exercice 15](#Exercice%2015)

---

## Rappels

- Un **tableau** est une suite de cases du même type, rangées les unes à côté des autres.
- `int T[50];` crée 50 cases numérotées de **0 à 49**. La première case est `T[0]`, la dernière est `T[49]`.
- Pour parcourir un tableau de `N` cases, on utilise une boucle `for` :

```c
for (i = 0; i < N; i++)
{
    // on travaille avec T[i]
}
```

- Un **tableau à 2 dimensions** `int M[L][C];` a `L` lignes et `C` colonnes. La case de la ligne `i` et de la colonne `j` est `M[i][j]`. On le parcourt avec **deux boucles `for` imbriquées** : la première pour les lignes, la seconde pour les colonnes.
- Lecture d'un entier : `scanf("%d", &T[i]);` (n'oubliez pas le `&`).
- Quand l'énoncé dit « dimension maximale 50 », on déclare le tableau avec 50 cases, mais on n'utilise que les `N` premières.

> [!warning] Attention Ne jamais utiliser une case qui n'existe pas (par exemple `T[50]` dans un tableau de 50 cases). Le programme peut planter ou donner des résultats faux.

---

## Exercice 1

> [!question] Énoncé Écrire un programme qui lit la dimension N d'un tableau unidimensionnel T du type int (dimension maximale 50 composantes), remplit le tableau par des valeurs entrées au clavier et affiche le tableau. Calculer et afficher ensuite la somme des éléments du tableau. Même problème pour un tableau bidimensionnel.

### Tableau à 1 dimension

```c
#include <stdio.h>

int main()
{
    int T[50];
    int N, i;
    int somme = 0;

    // 1. Lecture de N (on recommence tant que N n'est pas valide)
    do
    {
        printf("Dimension N (max 50) : ");
        scanf("%d", &N);
    } while (N < 1 || N > 50);

    // 2. Remplissage du tableau
    for (i = 0; i < N; i++)
    {
        printf("T[%d] = ", i);
        scanf("%d", &T[i]);
    }

    // 3. Affichage du tableau
    printf("Le tableau est : ");
    for (i = 0; i < N; i++)
    {
        printf("%d ", T[i]);
    }

    // 4. Calcul de la somme
    for (i = 0; i < N; i++)
    {
        somme = somme + T[i];
    }
    printf("\nLa somme est : %d\n", somme);

    return 0;
}
```

**Explications**

1. La boucle `do ... while` redemande `N` tant qu'il n'est pas entre 1 et 50. Sans cette vérification, un `N` trop grand ferait déborder le tableau.
2. À chaque tour de boucle, on lit une valeur et on la range dans la case `i`.
3. On repasse sur toutes les cases pour les afficher.
4. `somme` est un **accumulateur** : on la met à **0 avant** la boucle, puis on lui ajoute chaque case.

### Tableau à 2 dimensions

```c
#include <stdio.h>

int main()
{
    int T[50][50];
    int L, C, i, j;
    int somme = 0;

    printf("Nombre de lignes : ");
    scanf("%d", &L);
    printf("Nombre de colonnes : ");
    scanf("%d", &C);

    // Remplissage
    for (i = 0; i < L; i++)
    {
        for (j = 0; j < C; j++)
        {
            printf("T[%d][%d] = ", i, j);
            scanf("%d", &T[i][j]);
        }
    }

    // Affichage et calcul de la somme
    for (i = 0; i < L; i++)
    {
        for (j = 0; j < C; j++)
        {
            printf("%d\t", T[i][j]);
            somme = somme + T[i][j];
        }
        printf("\n");
    }

    printf("La somme est : %d\n", somme);
    return 0;
}
```

> [!tip] À retenir Le `printf("\n");` est placé **après** la boucle des colonnes, mais **dans** la boucle des lignes. Ainsi chaque ligne du tableau s'affiche sur une ligne de l'écran. `\t` est une tabulation qui aligne les colonnes.

---

## Exercice 2

> [!question] Énoncé Écrire un programme qui détermine le plus grand élément d'un tableau unidimensionnel de 10 valeurs. Compléter ensuite le programme pour afficher également le plus petit. Même problème pour un tableau bidimensionnel.

### Tableau à 1 dimension

```c
#include <stdio.h>

int main()
{
    int T[10];
    int i;
    int max, min;

    for (i = 0; i < 10; i++)
    {
        printf("T[%d] = ", i);
        scanf("%d", &T[i]);
    }

    // On part du premier élément
    max = T[0];
    min = T[0];

    // On compare avec tous les autres
    for (i = 1; i < 10; i++)
    {
        if (T[i] > max)
        {
            max = T[i];
        }
        if (T[i] < min)
        {
            min = T[i];
        }
    }

    printf("Le plus grand : %d\n", max);
    printf("Le plus petit : %d\n", min);
    return 0;
}
```

**Explications**

- On initialise `max` et `min` avec `T[0]`, **pas avec 0**. Sinon le résultat serait faux si toutes les valeurs sont négatives (le max ne pourrait jamais être inférieur à 0).
- La boucle commence à `i = 1` car la case 0 est déjà dans `max` et `min`.
- Dès qu'on trouve une valeur plus grande que `max`, on la mémorise. Même principe pour `min`.

### Tableau à 2 dimensions

```c
#include <stdio.h>

int main()
{
    int T[50][50];
    int L, C, i, j;
    int max, min;

    printf("Nombre de lignes : ");
    scanf("%d", &L);
    printf("Nombre de colonnes : ");
    scanf("%d", &C);

    for (i = 0; i < L; i++)
    {
        for (j = 0; j < C; j++)
        {
            printf("T[%d][%d] = ", i, j);
            scanf("%d", &T[i][j]);
        }
    }

    max = T[0][0];
    min = T[0][0];

    for (i = 0; i < L; i++)
    {
        for (j = 0; j < C; j++)
        {
            if (T[i][j] > max)
            {
                max = T[i][j];
            }
            if (T[i][j] < min)
            {
                min = T[i][j];
            }
        }
    }

    printf("Le plus grand : %d\n", max);
    printf("Le plus petit : %d\n", min);
    return 0;
}
```

C'est exactement la même idée, avec deux boucles imbriquées pour parcourir toutes les cases.

---

## Exercice 3

> [!question] Énoncé Dans une matrice de 10 lignes et 3 colonnes, mémoriser les tables de multiplication de 5, 6, et 7.

```c
#include <stdio.h>

int main()
{
    int M[10][3];
    int i, j;

    // Remplissage
    for (i = 0; i < 10; i++)
    {
        M[i][0] = 5 * (i + 1);   // table de 5
        M[i][1] = 6 * (i + 1);   // table de 6
        M[i][2] = 7 * (i + 1);   // table de 7
    }

    // Affichage
    for (i = 0; i < 10; i++)
    {
        for (j = 0; j < 3; j++)
        {
            printf("%4d", M[i][j]);
        }
        printf("\n");
    }
    return 0;
}
```

**Explications**

- Colonne 0 : table de 5. Colonne 1 : table de 6. Colonne 2 : table de 7.
- La ligne `i` contient le résultat pour le multiplicateur `i + 1` (car `i` commence à 0, alors que les tables commencent à ×1).
- `%4d` affiche chaque nombre sur 4 caractères, ce qui aligne bien les colonnes.

---

## Exercice 4

> [!question] Énoncé Nombre de caractères dans une chaîne : Demander à l'utilisateur de saisir son nom. Compter les caractères et afficher leur nombre. Rappel : un tableau de caractère (chaine de caractères), se termine par le caractère spécial : `'\0'`.

```c
#include <stdio.h>

int main()
{
    char nom[50];
    int nb = 0;

    printf("Entrez votre nom : ");
    scanf("%s", nom);          // pas de & devant nom

    while (nom[nb] != '\0')
    {
        nb++;
    }

    printf("Votre nom contient %d caractères.\n", nb);
    return 0;
}
```

**Explications**

- Une chaîne de caractères est un tableau de `char`. Le langage C ajoute automatiquement `'\0'` après le dernier caractère pour marquer la fin.
- Exemple : `"Ali"` est rangé dans `'A'`, `'l'`, `'i'`, `'\0'`.
- On avance case par case **tant qu'on n'a pas trouvé `'\0'`**. Le compteur `nb` donne alors le nombre de caractères.
- Pour une chaîne, on écrit `scanf("%s", nom);` **sans le `&`**.

> [!note] Remarque `scanf("%s", ...)` s'arrête au premier espace : « Jean Dupont » ne donnerait que « Jean ».

---

## Exercice 5

> [!question] Énoncé Saisir un mot et l'afficher dans l'ordre inversé.

```c
#include <stdio.h>

int main()
{
    char mot[50];
    int longueur = 0;
    int i;

    printf("Entrez un mot : ");
    scanf("%s", mot);

    // 1. Calcul de la longueur
    while (mot[longueur] != '\0')
    {
        longueur++;
    }

    // 2. Affichage à l'envers
    for (i = longueur - 1; i >= 0; i--)
    {
        printf("%c", mot[i]);
    }
    printf("\n");

    return 0;
}
```

**Explications**

1. On calcule d'abord la longueur, comme dans l'exercice 4.
2. On part de la **dernière lettre** (`longueur - 1`, car on commence à compter à 0) et on **recule** jusqu'à la case 0.

- Exemple avec `"chat"` : longueur = 4, on affiche `mot[3]`, `mot[2]`, `mot[1]`, `mot[0]`, ce qui donne `tahc`.
- `%c` sert à afficher **un seul caractère**.

---

## Exercice 6

> [!question] Énoncé Écrire un programme qui transfère un tableau M à deux dimensions L et C (dimensions maximales : 10 lignes et 10 colonnes) dans un tableau V unidimensionnel de L*C éléments.

```c
#include <stdio.h>

int main()
{
    int M[10][10];
    int V[100];
    int L, C, i, j;
    int k = 0;       // indice dans le tableau V

    printf("Nombre de lignes (max 10) : ");
    scanf("%d", &L);
    printf("Nombre de colonnes (max 10) : ");
    scanf("%d", &C);

    // Remplissage de M
    for (i = 0; i < L; i++)
    {
        for (j = 0; j < C; j++)
        {
            printf("M[%d][%d] = ", i, j);
            scanf("%d", &M[i][j]);
        }
    }

    // Transfert vers V
    for (i = 0; i < L; i++)
    {
        for (j = 0; j < C; j++)
        {
            V[k] = M[i][j];
            k++;
        }
    }

    // Affichage de V
    printf("Le tableau V : ");
    for (k = 0; k < L * C; k++)
    {
        printf("%d ", V[k]);
    }
    printf("\n");

    return 0;
}
```

**Explications**

- `V` contient `L * C` éléments (par exemple 3 lignes × 4 colonnes = 12 éléments).
- On parcourt `M` ligne par ligne. Pour chaque case, on la copie dans `V[k]`, puis on passe à la case suivante de `V` avec `k++`.
- Exemple : `M = {{1,2},{3,4}}` donne `V = {1,2,3,4}`.

---

## Exercice 7

> [!question] Énoncé Addition de deux matrices : Écrire un programme qui réalise l'addition de deux matrices A et B de mêmes dimensions N et M. Le résultat de l'addition sera mémorisé dans une troisième matrice C qui sera ensuite affichée.

```c
#include <stdio.h>

int main()
{
    int A[10][10], B[10][10], C[10][10];
    int N, M, i, j;

    printf("Nombre de lignes N : ");
    scanf("%d", &N);
    printf("Nombre de colonnes M : ");
    scanf("%d", &M);

    // Saisie de A
    printf("--- Matrice A ---\n");
    for (i = 0; i < N; i++)
    {
        for (j = 0; j < M; j++)
        {
            printf("A[%d][%d] = ", i, j);
            scanf("%d", &A[i][j]);
        }
    }

    // Saisie de B
    printf("--- Matrice B ---\n");
    for (i = 0; i < N; i++)
    {
        for (j = 0; j < M; j++)
        {
            printf("B[%d][%d] = ", i, j);
            scanf("%d", &B[i][j]);
        }
    }

    // Addition
    for (i = 0; i < N; i++)
    {
        for (j = 0; j < M; j++)
        {
            C[i][j] = A[i][j] + B[i][j];
        }
    }

    // Affichage de C
    printf("--- Matrice C = A + B ---\n");
    for (i = 0; i < N; i++)
    {
        for (j = 0; j < M; j++)
        {
            printf("%d\t", C[i][j]);
        }
        printf("\n");
    }

    return 0;
}
```

**Explications**

- Additionner deux matrices, c'est additionner les cases qui sont **à la même position** : `C[i][j] = A[i][j] + B[i][j]`.
- C'est pour cela que A et B doivent avoir les mêmes dimensions.

---

## Exercice 8

> [!question] Énoncé Écrire un programme qui permet de trier par ordre croissant les éléments d'un tableau unidimensionnel de nombres entiers.

On utilise le **tri par sélection** : on cherche le plus petit élément, on le met en première position, puis on recommence avec le reste du tableau.

```c
#include <stdio.h>

int main()
{
    int T[50];
    int N, i, j;
    int indice_min, temp;

    printf("Nombre d'éléments N : ");
    scanf("%d", &N);

    for (i = 0; i < N; i++)
    {
        printf("T[%d] = ", i);
        scanf("%d", &T[i]);
    }

    // Tri par sélection
    for (i = 0; i < N - 1; i++)
    {
        // Recherche de l'indice du plus petit élément à partir de i
        indice_min = i;
        for (j = i + 1; j < N; j++)
        {
            if (T[j] < T[indice_min])
            {
                indice_min = j;
            }
        }

        // Échange de T[i] et T[indice_min]
        temp = T[i];
        T[i] = T[indice_min];
        T[indice_min] = temp;
    }

    printf("Tableau trié : ");
    for (i = 0; i < N; i++)
    {
        printf("%d ", T[i]);
    }
    printf("\n");

    return 0;
}
```

**Explications**

- Au tour `i`, on cherche le plus petit élément parmi les cases `i` à `N-1`. On retient son **indice** dans `indice_min`.
- On **échange** ensuite ce minimum avec la case `i`. L'échange nécessite une variable `temp` : sans elle, une des deux valeurs serait écrasée.
- Après le tour `i`, les cases 0 à `i` sont définitivement bien placées.

**Exemple** avec `{5, 3, 8, 1}`

|Tour|Tableau après l'échange|
|---|---|
|i = 0|`{1, 3, 8, 5}`|
|i = 1|`{1, 3, 8, 5}`|
|i = 2|`{1, 3, 5, 8}`|

---

## Exercice 9

> [!question] Énoncé Déclarer un tableau d'entiers de 100 éléments et l'initialiser avec les nombres 0 à 99 (utiliser une boucle !). Afficher le tableau en séparant les valeurs par des virgules (limiter à 10 valeurs par lignes). Résultat attendu :
> 
> ```
> 0, 1, 2, 3, 4, 5, 6, 7, 8, 9
> 10, 11, 12, 13, 14, 15, 16, 17, 18, 19
> ...
> 90, 91, 92, 93, 94, 95, 96, 97, 98, 99
> ```

```c
#include <stdio.h>

int main()
{
    int T[100];
    int i;

    // Initialisation : la case i contient la valeur i
    for (i = 0; i < 100; i++)
    {
        T[i] = i;
    }

    // Affichage
    for (i = 0; i < 100; i++)
    {
        printf("%d", T[i]);

        if ((i + 1) % 10 == 0)
        {
            printf("\n");       // 10 valeurs affichées : on change de ligne
        }
        else
        {
            printf(", ");       // sinon on met une virgule
        }
    }

    return 0;
}
```

**Explications**

- L'opérateur `%` (modulo) donne le **reste de la division**. Par exemple `13 % 10 = 3` et `20 % 10 = 0`.
- `(i + 1) % 10 == 0` est vrai quand on vient d'afficher la 10e, 20e, 30e... valeur (donc pour `i` = 9, 19, 29...). On passe alors à la ligne.
- Dans les autres cas, on affiche une virgule et un espace.

---

## Exercice 10

> [!question] Énoncé Remplir un tableau avec des nombres entiers générés aléatoirement, puis afficher le contenu des cases de celui-ci.

```c
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

int main()
{
    int T[20];
    int i;

    srand(time(NULL));

    for (i = 0; i < 20; i++)
    {
        T[i] = rand() % 100;
    }

    for (i = 0; i < 20; i++)
    {
        printf("T[%d] = %d\n", i, T[i]);
    }

    return 0;
}
```

**Explications**

- `rand()` (bibliothèque `stdlib.h`) donne un grand nombre entier au hasard.
- `rand() % 100` donne le reste de la division par 100, donc un nombre entre **0 et 99**.
- `srand(time(NULL));` change le point de départ du hasard en fonction de l'heure (`time.h`). Sans cette ligne, on obtiendrait **les mêmes nombres à chaque exécution**. On ne l'écrit qu'**une seule fois**, avant les `rand()`.

---

## Exercice 11

> [!question] Énoncé Écrire un algorithme pour saisir 7 notes d'une classe dans un tableau. Modifier le programme pour en plus calculer la moyenne des notes.

```c
#include <stdio.h>

int main()
{
    float notes[7];
    float somme = 0;
    float moyenne;
    int i;

    // Saisie des notes
    for (i = 0; i < 7; i++)
    {
        printf("Note %d : ", i + 1);
        scanf("%f", &notes[i]);
    }

    // Calcul de la somme
    for (i = 0; i < 7; i++)
    {
        somme = somme + notes[i];
    }

    // Calcul et affichage de la moyenne
    moyenne = somme / 7;
    printf("La moyenne de la classe est : %.2f\n", moyenne);

    return 0;
}
```

**Explications**

- Les notes peuvent avoir des virgules, donc on utilise le type `float` et le format `%f`.
- La **moyenne = somme ÷ nombre de notes**.
- `%.2f` affiche le résultat avec 2 chiffres après la virgule.
- Le programme de base (saisie seule) s'arrête après la première boucle. Les deux dernières étapes sont la modification demandée.

---

## Exercice 12

> [!question] Énoncé
> 
> - Écrire un programme qui lit la dimension N d'un tableau T du type int (dimension maximale : 50 composantes), remplit le tableau par des valeurs entrées au clavier et affiche le tableau.
> - Copiez ensuite toutes les composantes strictement positives dans un deuxième tableau TPOS et toutes les valeurs strictement négatives dans un troisième tableau TNEG. Afficher les tableaux TPOS et TNEG.
> - Effacer ensuite toutes les occurrences de la valeur 0 dans le tableau et afficher le tableau résultant.

```c
#include <stdio.h>

int main()
{
    int T[50], TPOS[50], TNEG[50];
    int N, i, j;
    int nb_pos = 0;     // nombre d'éléments dans TPOS
    int nb_neg = 0;     // nombre d'éléments dans TNEG

    // --- Partie 1 : saisie et affichage ---
    do
    {
        printf("Dimension N (max 50) : ");
        scanf("%d", &N);
    } while (N < 1 || N > 50);

    for (i = 0; i < N; i++)
    {
        printf("T[%d] = ", i);
        scanf("%d", &T[i]);
    }

    printf("Tableau T : ");
    for (i = 0; i < N; i++)
    {
        printf("%d ", T[i]);
    }

    // --- Partie 2 : TPOS et TNEG ---
    for (i = 0; i < N; i++)
    {
        if (T[i] > 0)
        {
            TPOS[nb_pos] = T[i];
            nb_pos++;
        }
        if (T[i] < 0)
        {
            TNEG[nb_neg] = T[i];
            nb_neg++;
        }
    }

    printf("\nTableau TPOS : ");
    for (i = 0; i < nb_pos; i++)
    {
        printf("%d ", TPOS[i]);
    }

    printf("\nTableau TNEG : ");
    for (i = 0; i < nb_neg; i++)
    {
        printf("%d ", TNEG[i]);
    }

    // --- Partie 3 : suppression des zéros ---
    j = 0;      // prochaine case où écrire
    for (i = 0; i < N; i++)
    {
        if (T[i] != 0)
        {
            T[j] = T[i];
            j++;
        }
    }
    N = j;      // nouvelle taille du tableau

    printf("\nTableau T sans les zéros : ");
    for (i = 0; i < N; i++)
    {
        printf("%d ", T[i]);
    }
    printf("\n");

    return 0;
}
```

**Explications**

- **Partie 2** : `nb_pos` compte combien de valeurs positives on a déjà rangées. Elle sert à la fois d'**indice de la prochaine case libre** de `TPOS` et de **taille** de ce tableau. Même chose pour `nb_neg`. Les zéros ne sont copiés dans aucun des deux tableaux.
- **Partie 3** : pour supprimer les zéros, on « tasse » les valeurs vers la gauche. `i` lit toutes les cases, `j` indique où écrire. On recopie `T[i]` seulement s'il est différent de 0. À la fin, `j` est le nouveau nombre d'éléments.

**Exemple** : `T = {3, 0, -2, 0, 5}`

- `TPOS = {3, 5}`
- `TNEG = {-2}`
- `T` sans les zéros : `{3, -2, 5}` (N devient 3)

---

## Exercice 13

> [!question] Énoncé Écrire un programme qui lit la dimension N d'un tableau T du type int (dimension maximale : 50 composantes), remplit le tableau par des valeurs entrées au clavier et affiche le tableau. Ranger ensuite les éléments du tableau T dans l'ordre inverse sans utiliser de tableau d'aide. Afficher le tableau résultant. Idée : Échanger les éléments du tableau à l'aide de deux indices qui parcourent le tableau en commençant respectivement au début et à la fin du tableau et qui se rencontrent en son milieu.

```c
#include <stdio.h>

int main()
{
    int T[50];
    int N, i, j, temp;

    do
    {
        printf("Dimension N (max 50) : ");
        scanf("%d", &N);
    } while (N < 1 || N > 50);

    for (i = 0; i < N; i++)
    {
        printf("T[%d] = ", i);
        scanf("%d", &T[i]);
    }

    printf("Tableau initial : ");
    for (i = 0; i < N; i++)
    {
        printf("%d ", T[i]);
    }

    // Inversion
    i = 0;          // indice du début
    j = N - 1;      // indice de la fin
    while (i < j)
    {
        temp = T[i];
        T[i] = T[j];
        T[j] = temp;

        i++;        // le début avance
        j--;        // la fin recule
    }

    printf("\nTableau inversé : ");
    for (i = 0; i < N; i++)
    {
        printf("%d ", T[i]);
    }
    printf("\n");

    return 0;
}
```

**Explications**

- `i` commence au **début** du tableau, `j` à la **fin**.
- À chaque tour, on **échange** `T[i]` et `T[j]`, puis `i` avance et `j` recule.
- On s'arrête quand `i >= j` : les deux indices se sont rencontrés au milieu.

**Exemple** avec `{1, 2, 3, 4, 5}`

|Étape|i|j|Tableau|
|---|---|---|---|
|départ|0|4|`{1, 2, 3, 4, 5}`|
|après échange 1|1|3|`{5, 2, 3, 4, 1}`|
|après échange 2|2|2|`{5, 4, 3, 2, 1}`|

Quand `i = j = 2`, la boucle s'arrête : l'élément du milieu reste à sa place.

> [!warning] Erreur fréquente Si on parcourait tout le tableau avec `i` de 0 à `N-1` en échangeant avec `T[N-1-i]`, chaque paire serait échangée **deux fois** et le tableau reviendrait à son état initial.

---

## Exercice 14

> [!question] Énoncé Remplir un tableau à l'aide de valeurs générées aléatoirement, trier ces valeurs avant de les afficher.

Cet exercice **combine l'exercice 10** (valeurs aléatoires) **et l'exercice 8** (tri par sélection).

```c
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

int main()
{
    int T[20];
    int N = 20;
    int i, j;
    int indice_min, temp;

    srand(time(NULL));

    // Remplissage aléatoire
    for (i = 0; i < N; i++)
    {
        T[i] = rand() % 100;
    }

    // Tri par sélection (comme à l'exercice 8)
    for (i = 0; i < N - 1; i++)
    {
        indice_min = i;
        for (j = i + 1; j < N; j++)
        {
            if (T[j] < T[indice_min])
            {
                indice_min = j;
            }
        }
        temp = T[i];
        T[i] = T[indice_min];
        T[indice_min] = temp;
    }

    // Affichage
    printf("Valeurs triées : ");
    for (i = 0; i < N; i++)
    {
        printf("%d ", T[i]);
    }
    printf("\n");

    return 0;
}
```

**Explications** : l'ordre des étapes est important. On remplit d'abord, **puis** on trie, **puis** on affiche.

---

## Exercice 15

> [!question] Énoncé Écrire un programme qui lit les dimensions L et C d'un tableau T à deux dimensions du type int (dimensions maximales : 50 lignes et 50 colonnes). Remplir le tableau par des valeurs entrées au clavier et afficher le tableau ainsi que la somme de chaque ligne et de chaque colonne en n'utilisant qu'une variable d'aide pour la somme.

```c
#include <stdio.h>

int main()
{
    int T[50][50];
    int L, C, i, j;
    int somme;      // la seule variable d'aide pour les sommes

    printf("Nombre de lignes (max 50) : ");
    scanf("%d", &L);
    printf("Nombre de colonnes (max 50) : ");
    scanf("%d", &C);

    // Saisie
    for (i = 0; i < L; i++)
    {
        for (j = 0; j < C; j++)
        {
            printf("T[%d][%d] = ", i, j);
            scanf("%d", &T[i][j]);
        }
    }

    // Affichage du tableau
    printf("\nLe tableau :\n");
    for (i = 0; i < L; i++)
    {
        for (j = 0; j < C; j++)
        {
            printf("%d\t", T[i][j]);
        }
        printf("\n");
    }

    // Somme de chaque ligne
    for (i = 0; i < L; i++)
    {
        somme = 0;
        for (j = 0; j < C; j++)
        {
            somme = somme + T[i][j];
        }
        printf("Somme de la ligne %d : %d\n", i, somme);
    }

    // Somme de chaque colonne
    for (j = 0; j < C; j++)
    {
        somme = 0;
        for (i = 0; i < L; i++)
        {
            somme = somme + T[i][j];
        }
        printf("Somme de la colonne %d : %d\n", j, somme);
    }

    return 0;
}
```

**Explications**

- **Somme d'une ligne** : on fixe la ligne `i` (boucle extérieure) et on fait varier la colonne `j` (boucle intérieure).
- **Somme d'une colonne** : on **inverse** les rôles. La boucle extérieure fixe la colonne `j`, et la boucle intérieure fait varier la ligne `i`.
- On remet `somme = 0` **avant chaque ligne et chaque colonne**. Si on oublie, les sommes s'additionnent les unes aux autres et les résultats sont faux.
- La **même variable** `somme` est réutilisée partout, ce qui respecte la contrainte de l'énoncé.

**Exemple** avec le tableau 2 lignes × 3 colonnes

```
1 2 3
4 5 6
```

Sommes des lignes : 6 et 15. Sommes des colonnes : 5, 7 et 9.

---

## Récapitulatif

|Idée à retenir|Exercices|
|---|---|
|Parcourir un tableau avec `for (i = 0; i < N; i++)`|tous|
|Accumulateur : `somme = 0` avant la boucle, puis `somme = somme + T[i]`|1, 11, 15|
|Initialiser `max` et `min` avec la **première case**|2|
|Deux boucles imbriquées pour les tableaux 2D|1, 2, 3, 6, 7, 15|
|Une chaîne se termine par `'\0'`|4, 5|
|Échanger deux cases avec une variable `temp`|8, 13, 14|
|Compteur séparé pour remplir un autre tableau|6, 12|
|`rand()`, `srand()` et `%` pour les nombres aléatoires|10, 14|