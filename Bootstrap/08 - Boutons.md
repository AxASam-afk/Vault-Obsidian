---
tags: [bootstrap, bouton, button, btn, composant]
aliases: [Boutons, Bouton, Button, btn, btn-primary, Bouton outline, Bouton large, Bouton désactivé, Groupe de boutons, Bouton arrondi, Bouton plein largeur, Bouton icône]
---
# Boutons

**Mots-clés** : bouton, button, btn, cliquer, lien bouton, outline, taille bouton, large, petit, désactivé, disabled, pleine largeur, groupe, spinner, loading, arrondi, bouton rond

Retour : [[00 - Index Bootstrap]]

## Base
```html
<button type="button" class="btn btn-primary">Primary</button>
<button type="button" class="btn btn-secondary">Secondary</button>
<button type="button" class="btn btn-success">Success</button>
<button type="button" class="btn btn-danger">Danger</button>
<button type="button" class="btn btn-warning">Warning</button>
<button type="button" class="btn btn-info">Info</button>
<button type="button" class="btn btn-light">Light</button>
<button type="button" class="btn btn-dark">Dark</button>
<button type="button" class="btn btn-link">Link</button>
```

## Outline (contour seulement)
```html
<button class="btn btn-outline-primary">Outline</button>
```

## Tailles
```html
<button class="btn btn-primary btn-lg">Grand</button>
<button class="btn btn-primary btn-sm">Petit</button>
```

## Bouton pleine largeur
```html
<button class="btn btn-primary w-100">100 %</button>
<!-- ou -->
<div class="d-grid"><button class="btn btn-primary">Pleine largeur</button></div>
<div class="d-grid gap-2 d-md-block">...</div>  <!-- pleine largeur mobile seulement -->
```

## Bouton centré
Voir [[03 - Centrer]] : parent `text-center` ou `d-block mx-auto`.

## Lien en forme de bouton
```html
<a class="btn btn-primary" href="#" role="button">Lien</a>
```

## Désactivé
```html
<button class="btn btn-primary" disabled>Désactivé</button>
<a class="btn btn-primary disabled" aria-disabled="true">Lien désactivé</a>
```

## Actif (visuellement appuyé)
`class="btn btn-primary active"`

## Bouton rond / avec icône
```html
<button class="btn btn-primary rounded-circle"><i class="bi bi-plus"></i></button>
<button class="btn btn-primary rounded-pill">Pilule</button>
```
Icônes : [[01 - Installation]] + Bootstrap Icons.

## Spinner (chargement)
```html
<button class="btn btn-primary" disabled>
  <span class="spinner-border spinner-border-sm" aria-hidden="true"></span>
  Chargement...
</button>
```

## Groupe de boutons
```html
<div class="btn-group" role="group">
  <button class="btn btn-outline-primary">Gauche</button>
  <button class="btn btn-outline-primary">Milieu</button>
  <button class="btn btn-outline-primary">Droite</button>
</div>
```
Vertical : `btn-group-vertical`.

## Fermer (croix)
```html
<button type="button" class="btn-close" aria-label="Fermer"></button>
```

## Espace entre boutons
`me-2` sur chaque bouton, ou parent `d-flex gap-2`. Voir [[04 - Espacements]].
