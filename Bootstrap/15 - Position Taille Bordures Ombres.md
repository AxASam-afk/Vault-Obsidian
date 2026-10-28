---
tags: [bootstrap, position, taille, largeur, hauteur, bordure, ombre, arrondi, utilitaires]
aliases: [Position, Taille, Largeur, Hauteur, w-100, h-100, Bordure, Border, Ombre, Shadow, Arrondi, Rounded, z-index, Fixed, Sticky, Absolute, Relative, Overflow, Float, Coin arrondi]
---
# Position, taille, bordures, ombres

**Mots-clés** : position, absolute, relative, fixed, sticky, top, bottom, start, end, translate-middle, largeur, hauteur, w-100, h-100, vh-100, bordure, border, ombre, shadow, arrondi, rounded, overflow, float, z-index, clearfix

Retour : [[00 - Index Bootstrap]]

## Taille
| Classe | |
|---|---|
| `w-25 / w-50 / w-75 / w-100 / w-auto` | Largeur en % du parent |
| `h-25 / h-50 / h-75 / h-100 / h-auto` | Hauteur en % du parent |
| `mw-100` · `mh-100` | max-width / max-height 100 % |
| `vw-100` · `vh-100` | Largeur / hauteur de l'écran |
| `min-vw-100` · `min-vh-100` | Minimum écran |

Taille précise : `style="width: 250px"`.

## Position
```html
<div class="position-relative">
  <div class="position-absolute top-0 end-0">Coin haut droite</div>
</div>
```
Valeurs : `position-static | relative | absolute | fixed | sticky`
Placement : `top-0 | top-50 | top-100`, `bottom-0`, `start-0`, `end-0`

Raccourcis : `fixed-top`, `fixed-bottom`, `sticky-top`, `sticky-bottom`.

## ⭐ Centrer en absolu
```html
<div class="position-relative" style="height:300px">
  <div class="position-absolute top-50 start-50 translate-middle">Au centre exact</div>
</div>
```
Autre technique de centrage : [[03 - Centrer]].

## Badge de notification sur une icône
```html
<button class="btn btn-primary position-relative">
  Messages
  <span class="position-absolute top-0 start-100 translate-middle badge rounded-pill bg-danger">3</span>
</button>
```

## Bordures
```html
<div class="border">Toutes</div>
<div class="border-top">Haut</div>
<div class="border border-primary border-3">Bleue épaisse</div>
<div class="border-0">Aucune</div>
```
Côtés : `border-top | bottom | start | end`. Épaisseur : `border-1` à `border-5`.

## Arrondis
`rounded` · `rounded-1` à `rounded-5` · `rounded-circle` · `rounded-pill` · `rounded-0`
Un seul côté : `rounded-top`, `rounded-bottom`, `rounded-start`, `rounded-end`.

## Ombres
`shadow-none` · `shadow-sm` · `shadow` · `shadow-lg`

## Overflow
`overflow-auto` · `overflow-hidden` · `overflow-visible` · `overflow-scroll`

## Float
`float-start` · `float-end` · `float-none`, nettoyer avec `<div class="clearfix">`.

## z-index
`z-n1`, `z-0`, `z-1`, `z-2`, `z-3`.

## Lien étiré sur tout le parent
`stretched-link`, voir [[11 - Cards]].
