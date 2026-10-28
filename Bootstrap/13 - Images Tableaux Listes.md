---
tags: [bootstrap, image, img, tableau, table, liste, composant]
aliases: [Images, Image, Image responsive, img-fluid, Tableau, Tables, table-striped, Liste, Rounded, Image ronde, Miniature, Vidéo responsive, Ratio, Object-fit]
---
# Images, tableaux, listes

**Mots-clés** : image, img, responsive, img-fluid, arrondie, ronde, cercle, miniature, thumbnail, tableau, table, rayé, striped, hover, bordure, liste, ul, sans puces, vidéo, iframe, ratio, 16x9, youtube

Retour : [[00 - Index Bootstrap]]

## Images
```html
<img src="a.jpg" class="img-fluid" alt="">                  <!-- s'adapte à la largeur du parent -->
<img src="a.jpg" class="img-thumbnail" alt="">              <!-- cadre -->
<img src="a.jpg" class="rounded" alt="">                    <!-- coins arrondis -->
<img src="a.jpg" class="rounded-circle" width="100" height="100" alt="">  <!-- cercle (image carrée !) -->
<img src="a.jpg" class="rounded-pill" alt="">
```
Centrer une image → [[03 - Centrer]]. Alignement : `float-start`, `float-end`.

## Image qui remplit une zone sans se déformer
```html
<img src="a.jpg" class="w-100 object-fit-cover" style="height:200px" alt="">
```
`object-fit-cover | contain | fill | none`.

## Ratio (vidéo, iframe, image)
```html
<div class="ratio ratio-16x9">
  <iframe src="https://www.youtube.com/embed/ID" allowfullscreen></iframe>
</div>
```
Ratios : `ratio-1x1`, `ratio-4x3`, `ratio-16x9`, `ratio-21x9`.

## Tableaux
```html
<table class="table table-striped table-hover">
  <thead>
    <tr><th>#</th><th>Nom</th><th>Âge</th></tr>
  </thead>
  <tbody>
    <tr><td>1</td><td>Léa</td><td>25</td></tr>
    <tr><td>2</td><td>Tom</td><td>31</td></tr>
  </tbody>
</table>
```
| Classe | Effet |
|---|---|
| `table-striped` | Lignes alternées |
| `table-hover` | Survol |
| `table-bordered` | Bordures partout |
| `table-borderless` | Aucune |
| `table-sm` | Compact |
| `table-dark` | Fond sombre |
| `table-primary` (sur `tr`) | Ligne colorée |
| `align-middle` | Centre verticalement |

### Tableau scrollable sur mobile
```html
<div class="table-responsive">
  <table class="table">...</table>
</div>
```
Variantes : `table-responsive-md` (scroll jusqu'à md).

## Listes
```html
<ul class="list-unstyled">...</ul>          <!-- sans puces, sans marge -->
<ul class="list-inline">
  <li class="list-inline-item">A</li>
  <li class="list-inline-item">B</li>
</ul>
```
Pour des listes stylées, voir `list-group` dans [[12 - Composants JavaScript]].
