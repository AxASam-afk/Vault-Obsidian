---
tags: [algorithmique, C, ESGI, index]
---

# 🧭 Cours de Langage C — Index

Cours complet de **C** : du premier `printf` aux pointeurs, à la mémoire dynamique, aux fichiers et aux structures de données. Les notes de base (ESGI) sont conservées et **complétées**.

[Fonctions](Cours/Fonctions.md)
[Structures de Contrôle](Cours/Structures%20de%20Contr%C3%B4le.md)
[Tableaux](Cours/Tableaux.md)
[Variables et Opérateurs](Cours/Variables%20et%20Op%C3%A9rateurs.md)
[1ESGI/Language C/Exo/Corrigé Exercices Cours 1-2](Exo/Corrig%C3%A9%20Exercices%20Cours%201-2.md)

## 🟢 Niveau 1 — Bases
1. [Variables et Opérateurs](Cours/Variables%20et%20Op%C3%A9rateurs.md)
2. [Chaîne de compilation](Cours/Cha%C3%AEne%20de%20compilation.md)
3. [Types et Conversions](Cours/Types%20et%20Conversions.md)
4. [Entrées Sorties](Cours/Entr%C3%A9es%20Sorties.md)
5. [Structures de Contrôle](Cours/Structures%20de%20Contr%C3%B4le.md)
6. [Tableaux](Cours/Tableaux.md)
7. [Fonctions](Cours/Fonctions.md)
8. 📝 [1ESGI/Language C/Exo/Corrigé Exercices Cours 1-2](Exo/Corrig%C3%A9%20Exercices%20Cours%201-2.md)

## 🟡 Niveau 2 — Mémoire et données
9. [Pointeurs](Cours/Pointeurs.md)
10. [Pointeurs et Tableaux](Cours/Pointeurs%20et%20Tableaux.md)
11. [Chaînes de Caractères](Cours/Cha%C3%AEnes%20de%20Caract%C3%A8res.md)
12. [Structures et Types Composés](Cours/Structures%20et%20Types%20Compos%C3%A9s.md)
13. [Mémoire et Allocation Dynamique](Cours/M%C3%A9moire%20et%20Allocation%20Dynamique.md)
14. [Fichiers](Exo/Fichiers.md)
15. [Bits et Masques](Cours/Bits%20et%20Masques.md)

## 🔴 Niveau 3 — Programmation avancée
16. [Préprocesseur et Projets multi-fichiers](Cours/Pr%C3%A9processeur%20et%20Projets%20multi-fichiers.md)
17. [Structures de Données](Cours/Structures%20de%20Donn%C3%A9es.md)
18. [Tri et Recherche](Cours/Tri%20et%20Recherche.md)
19. [Récursivité](Cours/R%C3%A9cursivit%C3%A9.md)
20. [Débogage et Bonnes Pratiques](Cours/D%C3%A9bogage%20et%20Bonnes%20Pratiques.md)

## 🎯 Pratique
- 📝 [Exercices Cours 3-6](Cours/Exercices%20Cours%203-6.md)
- 🚀 [Projet Carnet de Contacts](Cours/Projet%20Carnet%20de%20Contacts.md)
- 📌 [Aide-mémoire C](Aide-m%C3%A9moire%20C.md)
- 🔧 [Corrections des notes de base](Cours/Corrections%20des%20notes%20de%20base.md)

## 🗺️ Carte du cours
```text
Variables ─► Types ─► E/S ─► Contrôle ─► Tableaux ─► Fonctions
                                             │            │
                                             ▼            ▼
                                       Pointeurs ─► Pointeurs/Tableaux ─► Chaînes
                                             │
                                             ▼
                       Structures ─► Mémoire dynamique ─► Fichiers
                                             │
                                             ▼
                     Modules/Makefile ─► Structures de données ─► Tri/Recherche ─► Récursivité
                                             │
                                             ▼
                                   Débogage ─► Projet final
```

## Lien avec le cours d'infrastructure
Le C sert à comprendre le fonctionnement bas niveau (octets, adresses, bits) vu en réseau : [Bits et Masques](Cours/Bits%20et%20Masques.md) ↔ adresses IP/masques de sous-réseau, `ssh`/`curl`/`tcpdump` écrits en C, sockets (suite possible du cours).
