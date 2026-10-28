---
tags: [bootstrap, texte, typographie, titres, police, utilitaires]
aliases: [Texte, Typographie, Titres, Police, Font, Taille du texte, Gras, Italique, Aligner le texte, text-center, fw-bold, fs-1, display-1, lead, Souligner, Majuscules, Texte tronqué]
---
# Typographie et texte

**Mots-clés** : texte, titre, h1, police, font, taille, gras, bold, italique, souligné, majuscule, minuscule, aligner texte, centrer texte, justifier, couper texte, tronquer, ellipsis, lead, display, citation, blockquote, interligne

Retour : [[00 - Index Bootstrap]]

## Titres
```html
<h1>Titre 1</h1> ... <h6>Titre 6</h6>
<p class="h3">Un paragraphe stylé comme un h3</p>
<h1 class="display-1">Très gros titre</h1>   <!-- display-1 à display-6 -->
<p class="lead">Paragraphe d'introduction plus grand</p>
```

## Taille
`fs-1` (grand) → `fs-6` (petit)
```html
<p class="fs-2">Texte taille 2</p>
<small class="text-body-secondary">Petit texte discret</small>
```

## Alignement
| Classe | |
|---|---|
| `text-start` | Gauche |
| `text-center` | Centre |
| `text-end` | Droite |
| `text-md-center` | Centre à partir de md |

## Poids et style
| Classe | |
|---|---|
| `fw-bold` | Gras |
| `fw-semibold` | Semi-gras |
| `fw-normal` | Normal |
| `fw-light` | Léger |
| `fst-italic` | Italique |
| `fst-normal` | Pas d'italique |

## Transformation
`text-lowercase` · `text-uppercase` · `text-capitalize`

## Décoration
`text-decoration-none` (enlever le soulignement d'un lien) · `text-decoration-underline` · `text-decoration-line-through`

## Interligne
`lh-1` · `lh-sm` · `lh-base` · `lh-lg`

## Couper / tronquer
```html
<p class="text-truncate" style="max-width:200px">Un très long texte qui sera coupé avec ...</p>
<p class="text-break">Casse les mots très longs (URL)</p>
<p class="text-nowrap">Pas de retour à la ligne</p>
```

## Police monospace
```html
<code>code inline</code>
<p class="font-monospace">Police monospace</p>
```

## Citation
```html
<blockquote class="blockquote">
  <p>Citation.</p>
  <footer class="blockquote-footer">Auteur</footer>
</blockquote>
```

## Couleur du texte
Voir [[07 - Couleurs et fonds]].
