---
tags: [bootstrap, flex, flexbox, alignement, utilitaires]
aliases: [Flexbox, Flex, d-flex, Aligner, Alignement, justify-content, align-items, flex-column, flex-wrap, flex-row, Côte à côte, Espace entre, space-between, Aligner à droite]
---
# Flexbox avec Bootstrap

**Mots-clés** : flex, flexbox, d-flex, aligner, alignement, côte à côte, en ligne, en colonne, space-between, justify-content, align-items, flex-wrap, flex-grow, flex-fill, aligner à droite, aligner à gauche, espacer, ordre

Retour : [[00 - Index Bootstrap]] · Voir aussi : [[03 - Centrer]]

## Activer
```html
<div class="d-flex">...</div>          <!-- block-level -->
<div class="d-inline-flex">...</div>   <!-- inline -->
```

## Direction
| Classe | Effet |
|---|---|
| `flex-row` | Horizontal (défaut) |
| `flex-row-reverse` | Horizontal inversé |
| `flex-column` | Vertical |
| `flex-column-reverse` | Vertical inversé |

## justify-content : axe principal
| Classe | Effet |
|---|---|
| `justify-content-start` | Début |
| `justify-content-center` | Centre |
| `justify-content-end` | Fin |
| `justify-content-between` | Espace **entre** (extrémités collées) |
| `justify-content-around` | Espace autour |
| `justify-content-evenly` | Espace égal partout |

## align-items : axe secondaire
| Classe | Effet |
|---|---|
| `align-items-start` | Haut |
| `align-items-center` | Milieu |
| `align-items-end` | Bas |
| `align-items-baseline` | Ligne de base du texte |
| `align-items-stretch` | Étiré (défaut) |

`align-self-*` : pareil mais pour **un seul** enfant.

## ⚠️ Axes inversés avec flex-column
Avec `flex-column`, `justify-content` agit **verticalement** et `align-items` **horizontalement**.
```html
<div class="d-flex flex-column justify-content-center align-items-center vh-100">
  <!-- centré partout, empilé verticalement -->
</div>
```

## Wrap
```html
<div class="d-flex flex-wrap">...</div>      <!-- passe à la ligne -->
<div class="d-flex flex-nowrap">...</div>
```

## Taille des enfants
```html
<div class="d-flex">
  <div class="flex-fill">Prend l'espace dispo</div>
  <div class="flex-grow-1">Idem (grandit)</div>
  <div class="flex-shrink-0">Ne rétrécit pas</div>
</div>
```

## Cas fréquents
**Logo à gauche, menu à droite**
```html
<div class="d-flex justify-content-between align-items-center">
  <div>Logo</div>
  <div>Menu</div>
</div>
```
**Un élément à gauche, le reste à droite**
```html
<div class="d-flex">
  <div>Gauche</div>
  <div class="ms-auto">Droite</div>
</div>
```
**Ordre**
```html
<div class="d-flex">
  <div class="order-2">2</div>
  <div class="order-1">1</div>
</div>
```
**Responsive** : `flex-column flex-md-row` → empilé sur mobile, ligne sur tablette+.

Voir aussi : [[04 - Espacements]] (gap)
