---
tags: [bootstrap, centrer, alignement, flex, recette, important]
aliases: [Centrer, Centrer une div, Mettre au milieu, Mettre au millieu, Au milieu, Au centre, Center, Centrer verticalement, Centrer horizontalement, Centrer texte, Centrer image, Centrer bouton, mx-auto, justify-content-center, align-items-center, text-center, vertical align, Aligner au centre, milieu, millieu, centre]
---
# 🎯 Centrer avec Bootstrap

**Mots-clés** : centrer, centré, centre, milieu, millieu, au milieu, au millieux, mettre au centre, center, middle, centrer horizontalement, centrer verticalement, centrer une div, centrer un texte, centrer une image, centrer un bouton, centrer dans la page, parfaitement centré, text-center, mx-auto, d-flex, justify-content-center, align-items-center, vh-100

Retour : [[00 - Index Bootstrap]] · Voir aussi : [[05 - Flexbox]] · [[17 - FAQ Comment faire]]

---
## ⚡ Tableau rapide
| Je veux centrer… | Solution |
|---|---|
| Du **texte** | `class="text-center"` |
| Un **bloc** (div avec largeur) horizontalement | `class="mx-auto"` |
| Une **image** | `class="d-block mx-auto"` |
| Des éléments **inline** / boutons | parent `class="text-center"` |
| **Horizontalement** avec flex | parent `class="d-flex justify-content-center"` |
| **Verticalement** avec flex | parent `class="d-flex align-items-center"` (+ hauteur !) |
| **Horizontal + vertical** (milieu exact) | parent `class="d-flex justify-content-center align-items-center"` |
| **Au milieu de toute la page** | `class="d-flex justify-content-center align-items-center vh-100"` |
| Une **colonne** de la grille | `row justify-content-center` |
| Avec la **grille CSS** | `d-grid` + `style="place-items:center"` (voir §11) |

---
## 1. Centrer du texte
```html
<p class="text-center">Texte centré</p>
```
Par breakpoint : `text-center`, `text-md-center`, `text-lg-start`.

## 2. Centrer un bloc horizontalement (`mx-auto`)
Le bloc doit avoir une **largeur définie**, sinon il prend 100 % et il n'y a rien à centrer.
```html
<div class="w-50 mx-auto">Bloc de 50 % centré</div>
<div class="mx-auto" style="width: 300px;">300px centré</div>
```

## 3. Centrer une image
```html
<img src="photo.jpg" class="d-block mx-auto" alt="">
```
Une `<img>` est inline : sans `d-block`, `mx-auto` ne marche pas. Alternative : le parent en `text-center`.

## 4. Centrer un bouton
```html
<div class="text-center">
  <button class="btn btn-primary">Cliquez</button>
</div>
```
ou :
```html
<button class="btn btn-primary d-block mx-auto">Cliquez</button>
```

## 5. Centrer horizontalement avec Flex
```html
<div class="d-flex justify-content-center">
  <div>Je suis centré</div>
</div>
```

## 6. Centrer verticalement ⚠️ Il faut une hauteur
Flex ne peut centrer verticalement que si le parent est plus haut que son contenu.
```html
<div class="d-flex align-items-center" style="height: 300px;">
  <div>Centré verticalement</div>
</div>
```
Hauteurs utiles : `h-100`, `vh-100`, `min-vh-100`, `h-50`.

## 7. 🏆 Centrer exactement au milieu (horizontal + vertical)
```html
<div class="d-flex justify-content-center align-items-center vh-100">
  <div>Au milieu de la page</div>
</div>
```
Variante pour un bloc dans un parent : `d-flex justify-content-center align-items-center` + `style="height:400px"`.

## 8. Centrer du texte dans une card / un bouton
```html
<div class="card text-center">...</div>
```

## 9. Centrer des colonnes dans la grille
```html
<div class="row justify-content-center">
  <div class="col-md-6">Colonne centrée</div>
</div>
```
Ou avec `offset` :
```html
<div class="col-6 offset-3">Centré (3 + 6 + 3 = 12)</div>
```

## 10. Centrer verticalement dans une row
```html
<div class="row align-items-center" style="min-height: 100vh;">
  <div class="col">Centré dans la hauteur</div>
</div>
```

## 11. Centrer avec la grille CSS (Bootstrap 5.3)
```html
<div class="d-grid" style="place-items: center; height: 300px;">
  <div>Centré</div>
</div>
```
`place-items` n'est pas une classe Bootstrap → style inline ou CSS perso.

## 12. Centrer dans une navbar
```html
<ul class="navbar-nav mx-auto">...</ul>
```
Voir [[10 - Navbar]].

## 13. Centrer un formulaire / une card de login (cas classique)
```html
<div class="container d-flex justify-content-center align-items-center min-vh-100">
  <div class="card p-4" style="width: 22rem;">
    <h3 class="text-center">Connexion</h3>
    <!-- formulaire -->
  </div>
</div>
```

## 14. Centrer le contenu d'un `<td>`, `<th>`
```html
<td class="text-center align-middle">Contenu</td>
```
`align-middle` = centrage vertical dans un tableau.

## 15. Centrer verticalement du texte à côté d'une image (media object)
```html
<div class="d-flex align-items-center">
  <img src="a.jpg" class="me-3" width="64" alt="">
  <p class="mb-0">Texte aligné au milieu de l'image</p>
</div>
```

---
## 🧩 Récap des classes
| Classe | Rôle |
|---|---|
| `text-center` | Centre texte et éléments inline |
| `mx-auto` | Marges gauche/droite auto → centre un bloc |
| `d-block` | Rend un élément inline en bloc |
| `d-flex` | Active flexbox |
| `justify-content-center` | Centre sur l'axe horizontal (par défaut) |
| `align-items-center` | Centre sur l'axe vertical (par défaut) |
| `vh-100` / `min-vh-100` | Hauteur = tout l'écran |
| `align-middle` | Centre verticalement (tableaux, inline) |
| `align-self-center` | Centre **un seul** enfant flex |
| `m-auto` | Centre dans les deux directions **dans un parent flex** |

## 💡 Astuce `m-auto` avec flex
```html
<div class="d-flex vh-100">
  <div class="m-auto">Centré partout</div>
</div>
```

## ❌ Pourquoi ça ne marche pas ?
- `mx-auto` sans largeur → bloc à 100 %, rien ne bouge
- `align-items-center` sans hauteur sur le parent → aucun effet visible
- `justify-content-center` sans `d-flex` → rien ne se passe
- `text-center` ne centre pas un bloc, seulement son contenu
- Flex direction `flex-column` : les axes sont **inversés** (voir [[05 - Flexbox]])
