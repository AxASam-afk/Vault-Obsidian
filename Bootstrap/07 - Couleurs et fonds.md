---
tags: [bootstrap, couleur, couleurs, fond, background, text-bg, utilitaires]
aliases: [Couleurs, Couleur, Background, Fond, bg-primary, text-primary, Couleur du texte, Couleur de fond, Opacité, Bootstrap colors, Thème couleurs, Dégradé, Gradient]
---
# Couleurs et fonds

**Mots-clés** : couleur, color, fond, background, bg, text, primary, secondary, success, danger, warning, info, light, dark, opacité, transparent, dégradé, gradient, rouge, vert, bleu

Retour : [[00 - Index Bootstrap]]

## Les couleurs sémantiques
| Nom | Couleur par défaut | Usage |
|---|---|---|
| `primary` | Bleu | Action principale |
| `secondary` | Gris | Action secondaire |
| `success` | Vert | Succès |
| `danger` | Rouge | Erreur / danger |
| `warning` | Jaune | Avertissement |
| `info` | Cyan | Information |
| `light` | Gris clair | |
| `dark` | Noir | |

## Couleur du texte
```html
<p class="text-primary">Bleu</p>
<p class="text-danger">Rouge</p>
<p class="text-success">Vert</p>
<p class="text-body-secondary">Gris discret</p>
<p class="text-white">Blanc</p>
```

## Couleur de fond
```html
<div class="bg-primary">Fond bleu</div>
<div class="bg-light">Fond clair</div>
<div class="bg-dark text-white">Fond sombre + texte blanc</div>
```

## ⭐ text-bg-* : fond + texte avec bon contraste automatique
```html
<div class="text-bg-primary p-3">Contraste auto</div>
<div class="text-bg-warning p-3">Texte noir sur jaune</div>
```

## Variantes subtiles (5.3)
```html
<div class="bg-primary-subtle text-primary-emphasis p-3">Fond pâle + texte foncé</div>
<div class="bg-danger-subtle border border-danger-subtle p-3">Alerte douce</div>
```

## Opacité
```html
<div class="bg-primary bg-opacity-25">Fond bleu 25 %</div>
<p class="text-primary text-opacity-50">Texte 50 %</p>
<div class="opacity-50">Tout l'élément à 50 %</div>
```
Valeurs : `25`, `50`, `75`, `100`.

## Transparent / dégradé
```html
<div class="bg-transparent">...</div>
<div class="bg-primary bg-gradient">Léger dégradé</div>
```

## Fond de la page
`bg-body`, `bg-body-secondary`, `bg-body-tertiary` : s'adaptent au [[16 - Personnalisation et Dark mode|dark mode]].

## Lien coloré
```html
<a href="#" class="link-primary">Lien bleu</a>
<a href="#" class="link-danger link-underline-opacity-0">Sans soulignement</a>
```

## Une couleur personnalisée ?
→ CSS perso ou variables, voir [[16 - Personnalisation et Dark mode]].
