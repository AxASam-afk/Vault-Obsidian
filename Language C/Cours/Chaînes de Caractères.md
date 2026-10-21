---
tags: [algorithmique, C, ESGI, chaines, string]
---

# Chaînes de Caractères

Fait suite à : [Pointeurs et Tableaux](Pointeurs%20et%20Tableaux.md) · Suite : [Structures et Types Composés](Structures%20et%20Types%20Compos%C3%A9s.md)

> [!important] Définition
> En C, une **chaîne** est un **tableau de `char` terminé par le caractère nul `'\0'`**. Il n'existe pas de type « string ». Une chaîne de *n* caractères occupe donc **n + 1 octets**.

```c
char mot[8] = "Bonjour";            // 7 lettres + '\0' = 8 octets
char vide[1] = "";                  // contient seulement '\0'
char tab[] = {'a', 'b', 'c', '\0'}; // équivalent à "abc"
```
```text
indice :  0    1    2    3    4    5    6    7
          B    o    n    j    o    u    r   '\0'
```

## Tableau ou littéral ?

```c
char s1[] = "abc";      // copie modifiable dans un tableau
char *s2  = "abc";      // pointe vers un littéral EN LECTURE SEULE
s1[0] = 'X';            // OK
s2[0] = 'X';            // ❌ comportement indéfini (plantage)
```
Pour un littéral, préférer `const char *s2 = "abc";` (le compilateur interdit alors la modification).

## Parcourir une chaîne

On s'arrête au `'\0'` :
```c
int longueur(const char *s)             // réécriture de strlen
{
    int n = 0;
    while (s[n] != '\0') n++;
    return n;
}

void afficherMajuscules(const char *s)
{
    for (int i = 0; s[i] != '\0'; i++)
        putchar(toupper((unsigned char)s[i]));   // <ctype.h>
    putchar('\n');
}
```

## La bibliothèque `<string.h>`

| Fonction | Rôle | Remarque |
|---|---|---|
| `strlen(s)` | longueur (sans `'\0'`) | `size_t`, parcours en O(n) |
| `strcpy(dst, src)` | copie `src` dans `dst` | ⚠️ ne vérifie pas la taille |
| `strncpy(dst, src, n)` | copie au plus `n` | ⚠️ **n'ajoute pas** `'\0'` si `src` ≥ `n` |
| `strcat(dst, src)` | concatène | ⚠️ `dst` doit être assez grand |
| `strcmp(a, b)` | compare | renvoie **0** si égales, `<0` ou `>0` sinon |
| `strncmp(a, b, n)` | compare les `n` premiers | |
| `strchr(s, c)` | 1ère occurrence d'un caractère | `NULL` si absent |
| `strstr(s, sub)` | 1ère occurrence d'une sous-chaîne | `NULL` si absent |
| `strcspn(s, rejet)` | longueur avant un caractère de `rejet` | utile pour retirer `\n` |
| `memcpy`, `memset`, `memmove` | copie / remplit des octets | [Tableaux](Tableaux.md) |

> [!important] Comparer des chaînes : `strcmp`, jamais `==`
> `if (a == b)` compare des **adresses**, pas le contenu. Écrire `if (strcmp(a, b) == 0)`.
> De même, on ne copie pas avec `=` : `strcpy`/`snprintf`.

```c
#include <stdio.h>
#include <string.h>

int main(void)
{
    char prenom[20] = "Awa";
    char nom[20]    = "Diop";
    char complet[50];

    snprintf(complet, sizeof complet, "%s %s", prenom, nom);   // concaténation sûre
    printf("%s (%zu caracteres)\n", complet, strlen(complet)); // Awa Diop (8 caracteres)

    if (strcmp(prenom, "Awa") == 0) printf("C'est Awa\n");
    if (strstr(complet, "Dio") != NULL) printf("contient Dio\n");
    return 0;
}
```

### Copie sécurisée
```c
snprintf(dest, sizeof dest, "%s", source);      // tronque proprement, ajoute toujours '\0'
```
`sizeof dest` ne fonctionne que si `dest` est un **vrai tableau** (pas un paramètre pointeur).

## Lire une chaîne en sécurité

```c
char nom[30];
if (fgets(nom, sizeof nom, stdin) != NULL)
    nom[strcspn(nom, "\n")] = '\0';              // retire le '\n' conservé par fgets
```
(voir [Entrées Sorties](Entr%C3%A9es%20Sorties.md)). `scanf("%29s", nom)` lit un mot sans espace.

## Conversions chaîne ↔ nombre

```c
int n = atoi("123");                    // simple, mais ne détecte pas les erreurs

char *fin;
long v = strtol("42abc", &fin, 10);     // v = 42, fin pointe sur "abc"
if (fin == "42abc") { /* rien converti */ }

char buf[32];
snprintf(buf, sizeof buf, "%d", 2025);  // nombre → chaîne
```
`strtod` pour les réels. `strtol` permet de **détecter** la saisie invalide (`*fin != '\0'`).

## Tableaux de chaînes

```c
const char *jours[] = {"lundi", "mardi", "mercredi"};
for (int i = 0; i < 3; i++) puts(jours[i]);

char noms[3][20] = {"Awa", "Moussa", "Fatou"};      // 3 chaînes de 19 caractères max
```

## Arguments de la ligne de commande

```c
#include <stdio.h>

int main(int argc, char *argv[])
{
    for (int i = 0; i < argc; i++)
        printf("argv[%d] = %s\n", i, argv[i]);      // argv[0] = nom du programme
    return 0;
}
```
`./prog bonjour 42` → `argc = 3`.

## Exemple : palindrome

```c
#include <stdio.h>
#include <string.h>

int estPalindrome(const char *s)
{
    int i = 0, j = (int)strlen(s) - 1;
    while (i < j)
    {
        if (s[i] != s[j]) return 0;
        i++;
        j--;
    }
    return 1;
}

int main(void)
{
    printf("%d\n", estPalindrome("radar"));     // 1
    printf("%d\n", estPalindrome("bonjour"));   // 0
    return 0;
}
```

## Erreurs classiques

| Erreur | Cause |
|---|---|
| Tableau trop petit (oubli du `'\0'`) | `char m[7] = "Bonjour";` → pas de place pour `'\0'` |
| `if (a == b)` entre chaînes | compare des adresses |
| `strcpy` vers un tampon trop petit | **débordement** (faille de sécurité) |
| Modifier un littéral | `char *s = "abc"; s[0]='x';` |
| `gets()` | supprimée du langage |

## Exercices

1. Compte les voyelles d'une chaîne.
2. Inverse une chaîne **en place**.
3. Écris `int compterMots(const char *s)`.

<details><summary>Corrigés</summary>

```c
int voyelles(const char *s)
{
    int c = 0;
    for (; *s; s++)
        if (strchr("aeiouyAEIOUY", *s)) c++;
    return c;
}

void inverserChaine(char *s)
{
    int i = 0, j = (int)strlen(s) - 1;
    while (i < j) { char t = s[i]; s[i++] = s[j]; s[j--] = t; }
}

int compterMots(const char *s)
{
    int mots = 0, dansMot = 0;
    for (; *s; s++)
    {
        if (*s != ' ' && !dansMot) { mots++; dansMot = 1; }
        else if (*s == ' ')         dansMot = 0;
    }
    return mots;
}
```
</details>

## Voir aussi

[Pointeurs et Tableaux](Pointeurs%20et%20Tableaux.md) · [Entrées Sorties](Entr%C3%A9es%20Sorties.md) · [Fichiers](../Exo/Fichiers.md) · [Exercices Cours 3-6](Exercices%20Cours%203-6.md)
