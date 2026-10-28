---
tags: [bootstrap, sass, scss, variables, personnalisation, dark-mode, theme]
aliases: [Personnaliser, Personnalisation, Customiser, Sass, SCSS, Variables CSS, Dark mode, Mode sombre, Thème, Theme, data-bs-theme, Changer les couleurs, Changer la couleur primary, Override, Surcharger]
---
# Personnalisation et Dark mode

**Mots-clés** : personnaliser, customiser, changer couleur bootstrap, modifier primary, sass, scss, variables css, --bs-primary, dark mode, mode sombre, thème, theme, surcharger, override, police perso, google fonts, !important

Retour : [[00 - Index Bootstrap]]

## 🌙 Dark mode (Bootstrap 5.3+)
```html
<html lang="fr" data-bs-theme="dark">
```
Ou sur un seul élément :
```html
<div data-bs-theme="dark">Cette zone est en sombre</div>
```
Bascule en JS :
```js
const html = document.documentElement;
html.setAttribute('data-bs-theme',
  html.getAttribute('data-bs-theme') === 'dark' ? 'light' : 'dark');
```
Suivre le système :
```js
const dark = matchMedia('(prefers-color-scheme: dark)').matches;
document.documentElement.setAttribute('data-bs-theme', dark ? 'dark' : 'light');
```
Pour que ça marche bien, utiliser `bg-body`, `text-body`, `text-body-secondary` plutôt que `bg-white` / `text-dark`. Voir [[07 - Couleurs et fonds]].

## Méthode simple : surcharger avec du CSS
Placer **après** le lien Bootstrap.
```css
:root {
  --bs-primary: #6f42c1;
  --bs-primary-rgb: 111, 66, 193;
  --bs-body-font-family: 'Poppins', sans-serif;
  --bs-border-radius: 0.75rem;
}
.btn-primary {
  --bs-btn-bg: #6f42c1;
  --bs-btn-border-color: #6f42c1;
  --bs-btn-hover-bg: #5a32a3;
  --bs-btn-hover-border-color: #5a32a3;
}
```
Les composants Bootstrap 5.3 exposent leurs variables locales (`--bs-btn-*`, `--bs-card-*`, `--bs-navbar-*`…).

## Police Google Fonts
```html
<link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;600&display=swap" rel="stylesheet">
```
```css
body { font-family: 'Poppins', sans-serif; }
```

## Méthode avancée : Sass
```bash
npm install bootstrap sass
```
`custom.scss` :
```scss
// 1. Surcharger les variables AVANT l'import
$primary: #6f42c1;
$font-family-sans-serif: 'Poppins', sans-serif;
$enable-shadows: true;
$enable-negative-margins: true;

// 2. Importer Bootstrap
@import "bootstrap/scss/bootstrap";
```
Compiler :
```bash
sass custom.scss custom.css
```

## Alléger : importer seulement ce qu'il faut
```scss
@import "bootstrap/scss/functions";
@import "bootstrap/scss/variables";
@import "bootstrap/scss/variables-dark";
@import "bootstrap/scss/maps";
@import "bootstrap/scss/mixins";
@import "bootstrap/scss/root";
@import "bootstrap/scss/reboot";
@import "bootstrap/scss/buttons";
@import "bootstrap/scss/grid";
```

## Ordre de chargement des CSS
1. `bootstrap.min.css`
2. `ton-style.css` (après, pour gagner en priorité)

Éviter `!important` autant que possible.
