---
tags: [bootstrap, espacement, margin, padding, gap, utilitaires]
aliases: [Marges, Margin, Padding, Espacement, Espace, Gap, mt-3, p-4, Marge intérieure, Marge extérieure, Espacer, mb-3, me-2, ms-auto, Écarter, Rapprocher]
---
# Espacements (margin, padding, gap)

**Mots-clés** : marge, margin, padding, espace, espacer, écarter, rapprocher, décaler, mt, mb, ms, me, mx, my, gap, gouttière, auto, négatif

Retour : [[00 - Index Bootstrap]]

## Syntaxe
`{propriété}{côté}-{breakpoint}-{taille}`

| Propriété | |
|---|---|
| `m` | margin (extérieur) |
| `p` | padding (intérieur) |

| Côté | |
|---|---|
| `t` | top (haut) |
| `b` | bottom (bas) |
| `s` | start (gauche en LTR) |
| `e` | end (droite en LTR) |
| `x` | gauche + droite |
| `y` | haut + bas |
| *(rien)* | les 4 côtés |

| Taille | Valeur |
|---|---|
| `0` | 0 |
| `1` | 0.25rem (4px) |
| `2` | 0.5rem (8px) |
| `3` | 1rem (16px) |
| `4` | 1.5rem (24px) |
| `5` | 3rem (48px) |
| `auto` | auto (margin seulement) |

## Exemples
```html
<div class="mt-3">marge haut 1rem</div>
<div class="mb-4">marge bas 1.5rem</div>
<div class="p-3">padding 1rem partout</div>
<div class="px-4 py-2">padding horizontal 1.5rem, vertical 0.5rem</div>
<div class="ms-auto">pousse à droite</div>
<div class="me-auto">pousse à gauche</div>
<div class="mx-auto" style="width:200px">centré</div>
```

## Responsive
```html
<div class="p-2 p-md-4 p-lg-5">Le padding grandit avec l'écran</div>
```

## Marges négatives
```html
<div class="mt-n3">marge négative (activer via Sass $enable-negative-margins)</div>
```
Désactivé par défaut.

## Gap (entre enfants flex / grid)
```html
<div class="d-flex gap-3">
  <div>A</div><div>B</div><div>C</div>
</div>
```
`gap-0` à `gap-5`, `row-gap-*`, `column-gap-*`.

## Mettre un espace entre éléments empilés
```html
<div class="vstack gap-3">
  <div>Ligne 1</div>
  <div>Ligne 2</div>
</div>
<div class="hstack gap-3">
  <div>Gauche</div><div>Droite</div>
</div>
```

## Cas fréquents
| Je veux… | Classe |
|---|---|
| Supprimer la marge d'un paragraphe | `mb-0` |
| Espace sous un titre | `mb-3` |
| Pousser un élément à droite | `ms-auto` |
| Espace entre boutons | `me-2` ou `gap-2` |
| Supprimer tout espace | `m-0 p-0` |

Voir aussi : [[05 - Flexbox]] · [[03 - Centrer]]
