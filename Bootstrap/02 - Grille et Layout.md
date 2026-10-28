---
tags: [bootstrap, grille, grid, layout, container, row, col, responsive]
aliases: [Grille Bootstrap, Grid Bootstrap, Colonnes, col-md, Container, Row, Breakpoints, Points de rupture, Mise en page, Layout, 12 colonnes]
---
# Grille et Layout

**Mots-clés** : grille, grid, colonnes, col, row, container, container-fluid, breakpoint, responsive, 12 colonnes, mettre côte à côte, deux colonnes, trois colonnes, gutter, g-3, offset, order

Retour : [[00 - Index Bootstrap]]

## Structure obligatoire
`container` → `row` → `col`
```html
<div class="container">
  <div class="row">
    <div class="col">Colonne 1</div>
    <div class="col">Colonne 2</div>
  </div>
</div>
```
- `.container` : largeur fixe centrée, qui change selon la taille d'écran
- `.container-fluid` : 100 % de la largeur
- `.container-md` : fluid jusqu'à `md`, puis largeur fixe
- `.row` : ligne (flex), contient les colonnes
- `.col` : colonne

## Les breakpoints
| Préfixe | À partir de | Appareil |
|---|---|---|
| *(aucun)* | 0 px | Petit mobile |
| `sm` | 576 px | Mobile paysage |
| `md` | 768 px | Tablette |
| `lg` | 992 px | Laptop |
| `xl` | 1200 px | Desktop |
| `xxl` | 1400 px | Grand écran |

## Largeurs de colonnes (total = 12)
```html
<div class="row">
  <div class="col-4">1/3</div>
  <div class="col-8">2/3</div>
</div>
```
- `col` : largeur égale automatique
- `col-6` : moitié
- `col-4` : tiers
- `col-3` : quart
- `col-auto` : largeur du contenu

## Responsive : empiler sur mobile, côte à côte sur grand écran
```html
<div class="row">
  <div class="col-12 col-md-6 col-lg-4">A</div>
  <div class="col-12 col-md-6 col-lg-4">B</div>
  <div class="col-12 col-md-12 col-lg-4">C</div>
</div>
```
Lecture : mobile = 1 par ligne, tablette = 2, laptop = 3.

## Espacement entre colonnes (gutters)
```html
<div class="row g-3">...</div>      <!-- horizontal + vertical -->
<div class="row gx-5">...</div>     <!-- horizontal seulement -->
<div class="row gy-2">...</div>     <!-- vertical seulement -->
```

## Cartes / colonnes de même nombre automatiquement
```html
<div class="row row-cols-1 row-cols-md-2 row-cols-lg-4 g-3">
  <div class="col">...</div>
  <div class="col">...</div>
  <div class="col">...</div>
  <div class="col">...</div>
</div>
```

## Décaler / réordonner
```html
<div class="col-md-4 offset-md-4">Centré avec offset</div>
<div class="col order-2">Deuxième</div>
<div class="col order-1">Premier</div>
<div class="col order-md-first">Premier à partir de md</div>
```

## Forcer un retour à la ligne
```html
<div class="w-100"></div>
```

## Alignement des colonnes dans la row
Voir [[03 - Centrer]] et [[05 - Flexbox]].
```html
<div class="row align-items-center justify-content-center">...</div>
```

## Erreurs classiques
- Mettre des `col` sans `row` → marges cassées
- `row` sans `container` → débordement horizontal
- Total des `col-*` > 12 → la colonne passe à la ligne
