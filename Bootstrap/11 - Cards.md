---
tags: [bootstrap, card, carte, composant]
aliases: [Cards, Card, Carte, Cartes, card-body, card-title, Carte image, Card horizontale, Liste de cartes]
---
# Cards

**Mots-clés** : card, carte, vignette, image en haut, titre, texte, bouton dans carte, carte horizontale, plusieurs cartes, grille de cartes, hauteur égale, header, footer

Retour : [[00 - Index Bootstrap]]

## Card simple
```html
<div class="card" style="width: 18rem;">
  <img src="photo.jpg" class="card-img-top" alt="...">
  <div class="card-body">
    <h5 class="card-title">Titre</h5>
    <p class="card-text">Du texte de présentation.</p>
    <a href="#" class="btn btn-primary">Voir</a>
  </div>
</div>
```

## Avec header / footer
```html
<div class="card">
  <div class="card-header">En-tête</div>
  <div class="card-body">Contenu</div>
  <div class="card-footer text-body-secondary">Pied</div>
</div>
```

## Plusieurs cartes alignées, de même hauteur
```html
<div class="row row-cols-1 row-cols-md-3 g-4">
  <div class="col">
    <div class="card h-100">
      <img src="a.jpg" class="card-img-top" alt="">
      <div class="card-body">
        <h5 class="card-title">Titre</h5>
        <p class="card-text">Texte</p>
      </div>
    </div>
  </div>
  <!-- répéter -->
</div>
```
`h-100` = toutes les cartes de la ligne ont la même hauteur.

## Carte horizontale
```html
<div class="card mb-3" style="max-width: 540px;">
  <div class="row g-0">
    <div class="col-md-4">
      <img src="a.jpg" class="img-fluid rounded-start" alt="">
    </div>
    <div class="col-md-8">
      <div class="card-body">
        <h5 class="card-title">Titre</h5>
        <p class="card-text">Texte</p>
      </div>
    </div>
  </div>
</div>
```

## Texte sur image (overlay)
```html
<div class="card text-bg-dark">
  <img src="a.jpg" class="card-img" alt="">
  <div class="card-img-overlay">
    <h5 class="card-title">Titre</h5>
  </div>
</div>
```

## Couleur / bordure
`class="card text-bg-primary"` · `class="card border-success"`

## Centrer le texte
`class="card text-center"` · Voir [[03 - Centrer]]

## Lien sur toute la carte
Ajouter `stretched-link` dans le lien :
```html
<a href="#" class="stretched-link">Voir</a>
```
(le parent doit être `position-relative`, c'est déjà le cas pour `.card`)
