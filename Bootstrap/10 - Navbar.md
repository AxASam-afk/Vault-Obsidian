---
tags: [bootstrap, navbar, menu, navigation, composant]
aliases: [Navbar, Barre de navigation, Menu, Menu burger, Burger, navbar-expand-lg, Navigation, Header, En-tête, Menu responsive, Navbar fixe, Sticky]
---
# Navbar (barre de navigation)

**Mots-clés** : navbar, menu, navigation, burger, hamburger, header, en-tête, logo, liens, fixe, sticky, collapse, responsive, dropdown menu, centrer le menu, menu à droite, couleur navbar

Retour : [[00 - Index Bootstrap]] · JS requis : [[01 - Installation]]

## Navbar complète et responsive
```html
<nav class="navbar navbar-expand-lg bg-body-tertiary">
  <div class="container">
    <a class="navbar-brand" href="#">MonSite</a>

    <button class="navbar-toggler" type="button"
            data-bs-toggle="collapse" data-bs-target="#menu"
            aria-controls="menu" aria-expanded="false" aria-label="Menu">
      <span class="navbar-toggler-icon"></span>
    </button>

    <div class="collapse navbar-collapse" id="menu">
      <ul class="navbar-nav ms-auto">
        <li class="nav-item"><a class="nav-link active" href="#">Accueil</a></li>
        <li class="nav-item"><a class="nav-link" href="#">À propos</a></li>
        <li class="nav-item dropdown">
          <a class="nav-link dropdown-toggle" href="#" data-bs-toggle="dropdown">Services</a>
          <ul class="dropdown-menu">
            <li><a class="dropdown-item" href="#">Web</a></li>
            <li><a class="dropdown-item" href="#">Mobile</a></li>
          </ul>
        </li>
      </ul>
    </div>
  </div>
</nav>
```
Le `id="menu"` du bouton et du collapse doivent correspondre.

## Breakpoint du burger
`navbar-expand-sm | md | lg | xl` : le menu est déplié **à partir** de cette taille, burger en dessous.

## Position des liens
| Je veux… | Classe sur `<ul class="navbar-nav ...">` |
|---|---|
| Liens à droite | `ms-auto` |
| Liens à gauche (défaut) | `me-auto` |
| Liens centrés | `mx-auto` |

## Couleurs
```html
<nav class="navbar navbar-expand-lg bg-dark" data-bs-theme="dark">...</nav>
<nav class="navbar navbar-expand-lg bg-primary" data-bs-theme="dark">...</nav>
```
⚠️ Bootstrap 5.3 : on utilise `data-bs-theme="dark"` (les anciennes `navbar-dark` / `navbar-light` sont dépréciées).

## Navbar fixe
```html
<nav class="navbar fixed-top">...</nav>
<nav class="navbar fixed-bottom">...</nav>
<nav class="navbar sticky-top">...</nav>   <!-- reste en haut au scroll -->
```
Avec `fixed-top`, ajouter du padding au `body` (`pt-5`) sinon le contenu passe dessous.

## Logo image
```html
<a class="navbar-brand" href="#">
  <img src="logo.svg" alt="" width="30" height="24" class="d-inline-block align-text-top">
  MonSite
</a>
```

## Formulaire de recherche
```html
<form class="d-flex" role="search">
  <input class="form-control me-2" type="search" placeholder="Rechercher">
  <button class="btn btn-outline-success" type="submit">OK</button>
</form>
```

## Problèmes fréquents
- Le burger ne s'ouvre pas → JS bundle manquant ([[01 - Installation]])
- `data-bs-target` ne correspond pas à l'id
- Menu non centré → `mx-auto` sur la `ul`, voir [[03 - Centrer]]
