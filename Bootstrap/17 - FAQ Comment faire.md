---
tags: [bootstrap, faq, cheatsheet, recettes, aide-memoire, important]
aliases: [FAQ Bootstrap, Comment faire, Cheat sheet Bootstrap, Aide-mémoire Bootstrap, Recettes Bootstrap, Astuces Bootstrap, Quelle classe, Je veux faire, Pense-bête]
---
# ❓ FAQ : « Comment faire X avec Bootstrap ? »

**Mots-clés** : comment, comment faire, je veux, quelle classe, astuce, recette, aide-mémoire, cheat sheet, rapide, pense-bête

Retour : [[00 - Index Bootstrap]]
> Utilise la recherche Obsidian (Ctrl/Cmd + Shift + F) et tape directement ce que tu veux faire : « centrer », « cacher mobile », « espace », « à droite »…

## 🎯 Alignement et position
| Je veux… | Classe / solution | Détails |
|---|---|---|
| Centrer du texte | `text-center` | [[03 - Centrer]] |
| Centrer une div / un bloc | `mx-auto` + largeur (`w-50`) | [[03 - Centrer]] |
| Mettre au milieu (horizontal + vertical) | `d-flex justify-content-center align-items-center` + hauteur | [[03 - Centrer]] |
| Mettre au milieu de la page | `d-flex justify-content-center align-items-center vh-100` | [[03 - Centrer]] |
| Centrer une image | `d-block mx-auto` | [[03 - Centrer]] |
| Centrer un bouton | parent `text-center` | [[03 - Centrer]] |
| Aligner à droite | `text-end` (texte) · `ms-auto` (bloc) · `justify-content-end` (flex) | [[05 - Flexbox]] |
| Aligner à gauche | `text-start` · `me-auto` | [[05 - Flexbox]] |
| Mettre côte à côte | `d-flex` ou `row` + `col` | [[02 - Grille et Layout]] |
| Mettre l'un au-dessus de l'autre | `flex-column` ou `vstack` | [[05 - Flexbox]] |
| Espacer à fond (gauche / droite) | `d-flex justify-content-between` | [[05 - Flexbox]] |
| Coller en bas de l'écran | `fixed-bottom` | [[15 - Position Taille Bordures Ombres]] |
| Coin haut-droite | `position-absolute top-0 end-0` | [[15 - Position Taille Bordures Ombres]] |
| Faire flotter à droite | `float-end` | [[15 - Position Taille Bordures Ombres]] |

## 📏 Espaces et tailles
| Je veux… | Classe |
|---|---|
| Marge en haut / bas | `mt-3` / `mb-3` |
| Marge à gauche / droite | `ms-3` / `me-3` |
| Marge horizontale / verticale | `mx-3` / `my-3` |
| Padding intérieur | `p-3`, `px-4`, `py-2` |
| Supprimer une marge | `m-0`, `mb-0` |
| Espace entre éléments flex | `gap-3` |
| Espace entre colonnes | `g-3` sur la `row` |
| Pleine largeur | `w-100` |
| Moitié de la largeur | `w-50` |
| Hauteur de tout l'écran | `vh-100` / `min-vh-100` |
| Image qui s'adapte | `img-fluid` |

Voir [[04 - Espacements]].

## 📱 Responsive
| Je veux… | Classe |
|---|---|
| Cacher sur mobile | `d-none d-md-block` |
| Montrer seulement sur mobile | `d-block d-md-none` |
| 1 colonne mobile, 2 tablette, 3 PC | `row row-cols-1 row-cols-md-2 row-cols-lg-3` |
| Empilé mobile, côte à côte PC | `d-flex flex-column flex-md-row` |
| Texte centré mobile, gauche PC | `text-center text-lg-start` |

Voir [[14 - Display et Responsive]] · [[02 - Grille et Layout]].

## 🎨 Style
| Je veux… | Classe |
|---|---|
| Texte en gras | `fw-bold` |
| Texte en italique | `fst-italic` |
| Texte en majuscules | `text-uppercase` |
| Plus gros texte | `fs-1` à `fs-6`, `display-1` |
| Texte en rouge | `text-danger` |
| Fond bleu | `bg-primary` |
| Fond + texte lisibles | `text-bg-primary` |
| Fond transparent à moitié | `bg-opacity-50` |
| Enlever le soulignement d'un lien | `text-decoration-none` |
| Coins arrondis | `rounded` |
| Cercle | `rounded-circle` |
| Bordure | `border` |
| Ombre | `shadow` |
| Tronquer un long texte | `text-truncate` |

Voir [[06 - Typographie]] · [[07 - Couleurs et fonds]] · [[15 - Position Taille Bordures Ombres]].

## 🧱 Composants
| Je veux… | Solution | Détails |
|---|---|---|
| Menu avec burger | `navbar navbar-expand-lg` | [[10 - Navbar]] |
| Menu à droite | `ms-auto` sur la `ul` | [[10 - Navbar]] |
| Une popup | `modal` | [[12 - Composants JavaScript]] |
| Déplier / replier | `collapse` | [[12 - Composants JavaScript]] |
| FAQ dépliable | `accordion` | [[12 - Composants JavaScript]] |
| Menu déroulant | `dropdown` | [[12 - Composants JavaScript]] |
| Slider d'images | `carousel` | [[12 - Composants JavaScript]] |
| Notification | `toast` | [[12 - Composants JavaScript]] |
| Info-bulle | `tooltip` (à activer en JS) | [[12 - Composants JavaScript]] |
| Panneau latéral | `offcanvas` | [[12 - Composants JavaScript]] |
| Onglets | `nav-tabs` + `tab-content` | [[12 - Composants JavaScript]] |
| Message d'alerte | `alert alert-danger` | [[12 - Composants JavaScript]] |
| Pastille / étiquette | `badge` | [[12 - Composants JavaScript]] |
| Carte | `card` | [[11 - Cards]] |
| Bouton | `btn btn-primary` | [[08 - Boutons]] |
| Formulaire | `form-control` | [[09 - Formulaires]] |
| Tableau rayé | `table table-striped` | [[13 - Images Tableaux Listes]] |
| Vidéo YouTube responsive | `ratio ratio-16x9` | [[13 - Images Tableaux Listes]] |
| Mode sombre | `data-bs-theme="dark"` | [[16 - Personnalisation et Dark mode]] |

## 🆘 « Ça ne marche pas »
| Symptôme | Piste |
|---|---|
| Aucun style | Lien CSS cassé → [[01 - Installation]] |
| Burger / modal / dropdown muets | JS bundle absent → [[01 - Installation]] |
| Pas responsive sur téléphone | Balise `viewport` absente → [[01 - Installation]] |
| `mx-auto` ne centre pas | Pas de largeur, ou élément inline → [[03 - Centrer]] |
| `align-items-center` sans effet | Parent sans hauteur, ou sans `d-flex` → [[03 - Centrer]] |
| Colonnes qui débordent | `row` hors d'un `container`, ou total > 12 → [[02 - Grille et Layout]] |
| Mon CSS n'est pas pris en compte | Charger ton CSS **après** Bootstrap → [[16 - Personnalisation et Dark mode]] |
| Classes de Bootstrap 4 qui ne marchent plus | Voir ci-dessous |

## 🔄 Bootstrap 4 → 5 (changements de noms)
| Bootstrap 4 | Bootstrap 5 |
|---|---|
| `ml-*` / `mr-*` | `ms-*` / `me-*` |
| `pl-*` / `pr-*` | `ps-*` / `pe-*` |
| `float-left` / `float-right` | `float-start` / `float-end` |
| `text-left` / `text-right` | `text-start` / `text-end` |
| `font-weight-bold` | `fw-bold` |
| `font-italic` | `fst-italic` |
| `data-toggle` / `data-target` | `data-bs-toggle` / `data-bs-target` |
| `badge-primary` | `text-bg-primary` |
| `form-group` | `mb-3` |
| `no-gutters` | `g-0` |
| `jumbotron` | supprimé → `p-5 bg-body-tertiary rounded-3` |
| jQuery requis | plus besoin |
