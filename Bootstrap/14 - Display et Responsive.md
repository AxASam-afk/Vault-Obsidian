---
tags: [bootstrap, display, responsive, cacher, afficher, visibilite, utilitaires]
aliases: [Display, d-none, d-block, Cacher, Masquer, Afficher, Responsive, Cacher sur mobile, Cacher sur desktop, Visible, Invisible, visually-hidden, Mobile, Tablette, Écran]
---
# Display et visibilité responsive

**Mots-clés** : display, cacher, masquer, hide, afficher, show, visible, invisible, mobile, desktop, tablette, seulement sur mobile, seulement sur desktop, d-none, d-block, d-md-none, responsive, breakpoint, print, impression, screen reader

Retour : [[00 - Index Bootstrap]] · Breakpoints : [[02 - Grille et Layout]]

## Valeurs de display
`d-none` · `d-block` · `d-inline` · `d-inline-block` · `d-flex` · `d-inline-flex` · `d-grid` · `d-table`

## Principe
`d-{breakpoint}-{valeur}` = s'applique **à partir** du breakpoint.

## 📱 Cacher / afficher selon l'écran
| Je veux… | Classes |
|---|---|
| Visible **seulement sur mobile** (< md) | `d-block d-md-none` |
| Visible **seulement à partir de md** | `d-none d-md-block` |
| Visible **seulement sur tablette** (md → lg) | `d-none d-md-block d-lg-none` |
| Visible **seulement à partir de lg** | `d-none d-lg-block` |
| Caché **partout** | `d-none` |
| Caché **seulement sur lg+** | `d-lg-none` |
| Caché seulement sur mobile (< sm) | `d-none d-sm-block` |

Si l'élément est flex : remplacer `d-block` par `d-flex` (`d-none d-md-flex`).

## Exemple
```html
<div class="d-block d-md-none">Je n'apparais que sur mobile</div>
<div class="d-none d-md-block">Je n'apparais que sur tablette et plus</div>
```

## Visibility vs display
- `d-none` : supprime l'élément de la mise en page
- `invisible` : cache mais **garde la place**
- `visible` : l'inverse

## Accessibilité : caché visuellement mais lu par les lecteurs d'écran
```html
<span class="visually-hidden">Texte pour lecteur d'écran</span>
```

## Impression
`d-print-none` · `d-print-block`

## Changer une mise en page selon la taille
```html
<div class="d-flex flex-column flex-md-row gap-3">...</div>
<p class="text-center text-lg-start">...</p>
<div class="p-2 p-lg-5">...</div>
```
Voir [[05 - Flexbox]] · [[04 - Espacements]] · [[06 - Typographie]]
