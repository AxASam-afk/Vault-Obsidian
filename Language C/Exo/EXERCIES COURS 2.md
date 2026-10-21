exo 1: Demander à l’utilisateur un nombre entier positif. Afficher tous les nombres pairs entre 0 et le nombre saisi.
```c
#include <stdio.h>

int main() {
    int n;
    int b = 0;

    printf("Entrez un nombre entier : ");
    scanf("%d", &n);

    printf("Les nombres positifs pairs entre 0 et %d sont :\n", n);

    while (b <= n) {
        printf("%d ", b);
        b += 2;
    }

    return 0;
}
```

Exo2: Demander le prix unitaire HT et le nombre d’exemplaires. Calculer et afficher le prix total HT, la TVA et le prix total (TTC) à payer.

```C
#include <stdio.h>

int main() {
    float PrixHT;
    int nbrexe;
    float PrixTVA;

    printf("Prix HT : ");
    scanf("%f", &PrixHT);
    printf("Nombre d'exemplaire : ");
    scanf("%d", &nbrexe);
    PrixHT = PrixHT*nbrexe;
    PrixTVA = PrixHT*1.2;
    printf("Prix TTC est de = %2f", PrixTVA);
    return 0;
}
```
Exo3: Demander à l’utilisateur de saisir un nombre réel. Afficher sa valeur absolue.
```c
#include <stdio.h>

int main() {
    double x;
    printf("Saisissez un nombre : ");
    scanf("%lf", &x);

    if (x < 0)
        x = -x;

    printf("Valeur absolue : %g\n", x);
    return 0;
}
```
Exo4: 4. Demander à l’utilisateur de saisir un nombre et afficher sa table de multiplication.
```c
#include <stdio.h>

int main() {
    int x, result;
    int multy = 1;
    printf("Saisissez un nombre : ");
    scanf("%d", &x);
    while(multy<=9){
        multy = multy+1;
        result = x*multy;
        printf("%d x %d = %d \n",x,multy,result);
    }
    return 0;
}
```
Exo5: Écrire un programme qui affiche à l'écran les tables de multiplication de 1 à 10
```c
#include <stdio.h>

int main() {
    int multy = 1, valeurb = 1, result;

    while (valeurb <= 10) {
        result = multy * valeurb;
        printf("%d x %d = %d\n", valeurb, multy, result);

        multy++;

        if (multy > 10) {
            multy = 1;
            valeurb++;
        }
    }

    return 0;
}
```
Exo6; Ecrire un programme quid détermine tous les diviseurs d’un nombre entier saisi, plus grands que 1 
```c
#include <stdio.h>

int main() {
    int n, i;
    printf("Entrez un nombre entier : ");
    scanf("%d", &n);

    printf("Les diviseurs de %d supérieurs à 1 sont :\n", n);
    for (i = 2; i <= n; i++) {
        if (n % i == 0) {
            printf("%d\n", i);
        }
    }
    return 0;
}
```
Exo7:La suite de Fibonacci est une suite u, définie par u0 = u1 = 1 et un = un-1 + un-2 pour n ≥ 2 - Affichez le premiers ‘n’ éléments de la suite Fibonacci. - Affichez les valeurs de la suite qui ne dépassent pas une valeur limite ‘x’*
```c
#include <stdio.h>

int main(void) {
    int n;
    int a=0,b=1,c;
    printf("Nombre de termes : ");
    scanf("%d", &n);
    for (int i = 0; i < n; i++) {
        c=a+b;
        a=b;
        b=c;
        printf("%d, ",a);
        
    }
        
    return 0;
}
```
Exo8: Écrire un programme qui simule une horloge numérique. Ce programme doit afficher toutes les heures et toutes les minutes d'une journée complète, du 00:00 (minuit) jusqu'à 23:59 (fin de la journée). Aide : Pour afficher un entier avec au moins deux chiffres (si l'entier a moins de 2 chiffres, il sera complété par des zéros à gauche), il faut utiliser %02d dans le printf.
```c
#include <stdio.h>

int main() {
    int h, m;

    for (h = 0; h < 24; h++) {
        for (m = 0; m < 60; m++) {
            printf("%02d:%02d\n", h, m);
        }
    }
    return 0;
}
```
Exo9:Jeu de devinette de chiffre : Le joueur doit essayer de deviner un nombre généré aléatoirement entre 1 et 100. Le programme fournit des indications à l'utilisateur (trop grand ou trop petit) jusqu'à ce que le nombre soit deviné. Le programme compte le nombre d'essais nécessaires pour trouver le bon nombre et limite le nombre d’essai à 10. Si le nombre cherché n'a pas été deviné, la machine affiche « Perdu ».
```c
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

int main() {
    srand(time(NULL));
    int rdmNum = rand() % 101, GNum, Try = 10;

    while (Try != 0) {
        printf("Devine le nombre (0-100) : ");
        scanf("%d", &GNum);

        if (GNum == rdmNum) {
            printf("Bravoo !! Le nombre était : %d\n", rdmNum);
            break;
        } else if (GNum < rdmNum) {
            Try--;
            printf("Trop petit ! Il te reste %d essai(s)\n", Try);
        } else {
            Try--;
            printf("Trop grand ! Il te reste %d essai(s)\n", Try);
        }
    }

    if (Try == 0) {
        printf("Perdu ! Le nombre était : %d\n", rdmNum);
    }

    return 0;
}
```
Exo10:Ecrire un programme qui affiche les formes suivantes. Le nombre de lignes est entré au clavier. Ex : Nombre de lignes : 10
![[Pasted image 20261001103528.png]]

A).
```c
#include <stdio.h>

int main() {
    int x;
    char c = '*';

    printf("Entrez le nombre de lignes : ");
    scanf("%d", &x);

    for (int i = 1; i <= x; i++) {
        for (int j = 1; j <= i; j++) {
            printf("%c", c);
        }
        printf("\n");
    }

    return 0;
}
```

B)
```c
#include <stdio.h>

int main() {
    int x;
    char c = '*';

    printf("Entrez le nombre de lignes : ");
    scanf("%d", &x);

    for (int i = x; i >= 1; i--) {
        for (int j = 1; j <= i; j++) {
            printf("%c", c);
        }
        printf("\n");
    }

    return 0;
}
```
C)
```c
#include <stdio.h>

int main() {
    int x;
    char c = '*';

    printf("Entrez le nombre de lignes : ");
    scanf("%d", &x);

    for (int i = 1; i <= x; i++) {

        for (int j = 1; j <= i - 1; j++) {
            printf(" ");
        }

        for (int j = 1; j <= x - i + 1; j++) {
            printf("%c", c);
        }

        printf("\n");
    }

    return 0;
}
```
D)
```c
#include <stdio.h>

int main() {
    int x;
    char c = '*';

    printf("Entrez le nombre de lignes : ");
    scanf("%d", &x);

    for (int i = 1; i <= x; i++) {

        for (int j = 1; j <= x - i; j++) {
            printf(" ");
        }

        for (int j = 1; j <= i; j++) {
            printf("%c", c);
        }

        printf("\n");
    }

    return 0;
}
```
fv