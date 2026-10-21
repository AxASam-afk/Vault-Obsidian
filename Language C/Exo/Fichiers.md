---
tags: [algorithmique, C, ESGI, fichiers, fopen]
---

# Fichiers

Fait suite à : [Mémoire et Allocation Dynamique](../Cours/M%C3%A9moire%20et%20Allocation%20Dynamique.md) · [Entrées Sorties](../Cours/Entr%C3%A9es%20Sorties.md) · Suite : [Préprocesseur et Projets multi-fichiers](../Cours/Pr%C3%A9processeur%20et%20Projets%20multi-fichiers.md)

> [!important] Principe
> Un fichier se manipule via un **flux** de type `FILE *` : on **ouvre** (`fopen`), on **lit/écrit**, puis on **ferme** (`fclose`). Les fonctions ressemblent à celles de la console : `printf` → `fprintf`, `scanf` → `fscanf`.

## Ouvrir et fermer

```c
FILE *f = fopen("donnees.txt", "r");
if (f == NULL)
{
    perror("fopen");                    // affiche la cause (ex. "No such file or directory")
    return 1;
}
// ... utilisation ...
fclose(f);
```
**Toujours** tester `NULL` et **toujours** fermer (les données en tampon sont écrites à la fermeture).

| Mode | Effet | Si le fichier n'existe pas |
|---|---|---|
| `"r"` | lecture | **erreur** (`NULL`) |
| `"w"` | écriture, **efface** le contenu existant | créé |
| `"a"` | écriture à la **fin** (ajout) | créé |
| `"r+"` | lecture + écriture | erreur |
| `"w+"`, `"a+"` | lecture + écriture | créé |
| ajouter `b` (`"rb"`, `"wb"`) | mode **binaire** | — |

## Fichiers texte

### Écrire
```c
FILE *f = fopen("notes.txt", "w");
if (f == NULL) return 1;
fprintf(f, "%s;%d\n", "Awa", 15);
fputs("Moussa;12\n", f);
fclose(f);
```

### Lire ligne par ligne (méthode recommandée)
```c
char ligne[128];
while (fgets(ligne, sizeof ligne, f) != NULL)
{
    ligne[strcspn(ligne, "\n")] = '\0';
    printf("[%s]\n", ligne);
}
```
`fgets` renvoie `NULL` à la fin du fichier ou en cas d'erreur.

> [!warning] N'écris pas `while (!feof(f))`
> `feof` ne devient vrai **qu'après** une lecture ratée : on traite alors une dernière fois des données invalides. Teste plutôt le **retour** de la fonction de lecture.

### Lire des valeurs formatées
```c
char nom[30]; int note;
while (fscanf(f, "%29[^;];%d\n", nom, &note) == 2)
    printf("%s -> %d\n", nom, note);
```

### Caractère par caractère
```c
int c;                                  // int (et non char) pour pouvoir stocker EOF
while ((c = fgetc(f)) != EOF)
    putchar(c);
```

## Fichiers binaires

On écrit la représentation **brute** en mémoire : compact et rapide, mais non lisible et non portable d'une machine à l'autre (taille des types, ordre des octets, padding).
```c
typedef struct { char nom[30]; int age; } Personne;

Personne p = {"Awa", 19};
FILE *f = fopen("p.bin", "wb");
fwrite(&p, sizeof p, 1, f);             // (adresse, taille d'un élément, nombre d'éléments, flux)
fclose(f);

Personne q;
f = fopen("p.bin", "rb");
if (fread(&q, sizeof q, 1, f) == 1) printf("%s %d\n", q.nom, q.age);
fclose(f);
```
`fwrite`/`fread` renvoient le **nombre d'éléments** réellement traités.

## Se déplacer dans un fichier

| Fonction | Rôle |
|---|---|
| `ftell(f)` | position courante (octets) |
| `fseek(f, décalage, origine)` | se placer ; origine = `SEEK_SET` (début), `SEEK_CUR`, `SEEK_END` |
| `rewind(f)` | retour au début |
```c
fseek(f, 0, SEEK_END);
long taille = ftell(f);                 // taille du fichier en octets
rewind(f);
```

## Exemple complet : copier un fichier

```c
#include <stdio.h>

int main(int argc, char *argv[])
{
    if (argc != 3)
    {
        fprintf(stderr, "Usage : %s source destination\n", argv[0]);
        return 1;
    }

    FILE *src = fopen(argv[1], "rb");
    if (src == NULL) { perror("source"); return 1; }

    FILE *dst = fopen(argv[2], "wb");
    if (dst == NULL) { perror("destination"); fclose(src); return 1; }

    char tampon[4096];
    size_t n;
    while ((n = fread(tampon, 1, sizeof tampon, src)) > 0)
        fwrite(tampon, 1, n, dst);

    fclose(src);
    fclose(dst);
    return 0;
}
```

## Exemple : compter les lignes

```c
#include <stdio.h>

int main(void)
{
    FILE *f = fopen("notes.txt", "r");
    if (f == NULL) { perror("notes.txt"); return 1; }

    int lignes = 0, c;
    while ((c = fgetc(f)) != EOF)
        if (c == '\n') lignes++;

    fclose(f);
    printf("%d ligne(s)\n", lignes);
    return 0;
}
```

## Autres fonctions utiles
`remove("f.txt")` (supprime), `rename("a", "b")`, `fflush(f)` (force l'écriture), `ferror(f)` (erreur ?), `errno` / `strerror(errno)` (`<errno.h>`).

## Exercices

1. Écris un programme qui sauvegarde un tableau d'entiers dans un fichier texte (un par ligne) puis le relit.
2. Affiche la plus longue ligne d'un fichier.
3. Ajoute (mode `"a"`) une ligne horodatée dans un fichier de log.

<details><summary>Corrigé de 1</summary>

```c
#include <stdio.h>

int main(void)
{
    int t[5] = {4, 8, 15, 16, 23};

    FILE *f = fopen("tab.txt", "w");
    if (f == NULL) return 1;
    for (int i = 0; i < 5; i++) fprintf(f, "%d\n", t[i]);
    fclose(f);

    int lu[5], n = 0;
    f = fopen("tab.txt", "r");
    if (f == NULL) return 1;
    while (n < 5 && fscanf(f, "%d", &lu[n]) == 1) n++;
    fclose(f);

    for (int i = 0; i < n; i++) printf("%d ", lu[i]);
    printf("\n");
    return 0;
}
```
</details>

