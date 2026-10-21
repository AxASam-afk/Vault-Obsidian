---
tags: [algorithmique, C, ESGI, printf, scanf]
---

# Entrées Sorties

Fait suite à : [Types et Conversions](Types%20et%20Conversions.md) · Suite : [Pointeurs](Pointeurs.md)

## `printf` : afficher

`printf("format", valeurs...)` — le **format** contient du texte et des **spécificateurs** `%...`.

| Format             | Type attendu          | Exemple              |
| ------------------ | --------------------- | -------------------- |
| `%d` / `%i`        | `int`                 | `42`                 |
| `%u`               | `unsigned int`        | `42`                 |
| `%ld` / `%lld`     | `long` / `long long`  | `123456789012`       |
| `%f`               | `float` / `double`    | `3.141593`           |
| `%.2f`             | réel, 2 décimales     | `3.14`               |
| `%e`               | notation scientifique | `3.14e+00`           |
| `%c`               | `char`                | `A`                  |
| `%s`               | chaîne                | `Bonjour`            |
| `%x` / `%X` / `%o` | hexa / octal          | `ff` / `377`         |
| `%p`               | pointeur (adresse)    | `0x7ffd...`          |
| `%zu`              | `size_t`              | résultat de `sizeof` |
| `%%`               | le caractère `%`      | `%`                  |

### Largeur, alignement, remplissage
```c
printf("[%5d]\n",  42);     // [   42]   largeur 5
printf("[%-5d]\n", 42);     // [42   ]   aligné à gauche
printf("[%05d]\n", 42);     // [00042]   rempli de zéros
printf("[%8.3f]\n", 3.14159);   // [   3.142]
printf("[%10s]\n", "abc");  // [       abc]
```
> [!important] Le format doit correspondre au type
> `printf("%d", 3.5)` ou `printf("%f", 3)` donnent un résultat **indéfini**. `-Wall` signale ces erreurs.
> En `printf`, un `float` est promu en `double` : `%f` convient aux deux. En **`scanf`**, `%f` lit un `float`, **`%lf`** lit un `double`.

## `scanf` : saisir

`scanf("format", &variable)` — il faut l'**adresse** de la variable (`&`), sauf pour un tableau de caractères.

```c
int age;
float taille;
char initiale;
char prenom[20];

scanf("%d", &age);
scanf("%f", &taille);
scanf(" %c", &initiale);      // l'espace avant %c ignore les retours à la ligne en attente
scanf("%19s", prenom);        // PAS de & pour un tableau ; 19 = limite anti-débordement
```

> [!important] Corrections par rapport aux notes de base
> - `scanf("%19s", &prenom)` est incorrect : `prenom` est déjà une adresse. On écrit `scanf("%19s", prenom)`.
> - `%s` s'arrête au **premier espace** : « Jean Paul » ne se lit pas en entier.

### `scanf` renvoie le nombre de valeurs lues
```c
int n;
if (scanf("%d", &n) != 1)
{
    printf("Saisie invalide.\n");
    return 1;
}
```
Toujours **vérifier** le retour quand l'utilisateur peut taper n'importe quoi.

### Le problème du tampon (buffer)
Quand l'utilisateur tape `42⏎`, `scanf("%d")` consomme `42` mais **laisse le `\n`** dans le tampon : un `scanf("%c")` ou un `fgets` suivant lira ce `\n`.
```c
void viderBuffer(void)
{
    int c;
    while ((c = getchar()) != '\n' && c != EOF) { }
}
```
> [!important] Ne jamais écrire `fflush(stdin)` : comportement indéfini.

## Lecture robuste : `fgets` + `sscanf`

Lire **une ligne entière** puis l'analyser évite la plupart des pièges :
```c
char ligne[100];
int age;

printf("Votre age : ");
if (fgets(ligne, sizeof ligne, stdin) != NULL && sscanf(ligne, "%d", &age) == 1)
    printf("Age = %d\n", age);
else
    printf("Saisie invalide\n");
```
`fgets(destination, taille, stdin)` lit au plus `taille-1` caractères, **conserve le `\n`**, et termine par `\0`. Pour retirer le `\n` : `ligne[strcspn(ligne, "\n")] = '\0';` (`<string.h>`).

> [!important] `gets` est interdit
> `gets()` ne limite pas la taille lue : c'est une faille de sécurité (débordement). Elle a été **supprimée du langage** (C11). Utiliser `fgets`.

## Autres fonctions

| Fonction | Rôle |
|---|---|
| `putchar(c)` / `getchar()` | écrire / lire **un** caractère |
| `puts("texte")` | affiche la chaîne + `\n` |
| `fprintf(stderr, "erreur\n")` | écrit sur la **sortie d'erreur** (séparée de `stdout`) |
| `sprintf` / `snprintf(buf, taille, fmt, ...)` | « affiche » dans une chaîne ; préférer `snprintf` |
| `sscanf(chaine, fmt, ...)` | `scanf` depuis une chaîne |

Flux standards : `stdin` (clavier), `stdout` (écran), `stderr` (erreurs). Redirections shell : `./prog < entree.txt > sortie.txt`.

## Exemple complet

```c
#include <stdio.h>
#include <string.h>

int main(void)
{
    char nom[50];
    int age;
    char ligne[64];

    printf("Votre nom complet : ");
    if (fgets(nom, sizeof nom, stdin) == NULL) return 1;
    nom[strcspn(nom, "\n")] = '\0';

    printf("Votre age : ");
    if (fgets(ligne, sizeof ligne, stdin) == NULL || sscanf(ligne, "%d", &age) != 1)
    {
        fprintf(stderr, "Age invalide\n");
        return 1;
    }

    printf("Bonjour %s, vous avez %d ans (%.1f%% de 100).\n", nom, age, (double)age);
    return 0;
}
```

## Exercices

1. Lire deux entiers et afficher leur moyenne avec 2 décimales.
2. Afficher une table de multiplication de 7 alignée (`%2d x 7 = %3d`).
3. Lire un mot et un entier en vérifiant le retour de `scanf`.

<details><summary>Corrigés</summary>

```c
// 1
int a, b;
if (scanf("%d %d", &a, &b) == 2) printf("%.2f\n", (a + b) / 2.0);

// 2
for (int i = 1; i <= 10; i++) printf("%2d x 7 = %3d\n", i, i * 7);
```
</details>

## Voir aussi

[Variables et Opérateurs](Variables%20et%20Op%C3%A9rateurs.md) · [Chaînes de Caractères](Cha%C3%AEnes%20de%20Caract%C3%A8res.md) · [Fichiers](../Exo/Fichiers.md)
