---
tags: [bootstrap, installation, cdn, npm, debut]
aliases: [Installer Bootstrap, CDN Bootstrap, Importer Bootstrap, npm bootstrap, Démarrer Bootstrap, Setup Bootstrap, lien bootstrap, link bootstrap]
---
# Installation de Bootstrap

**Mots-clés** : installer, installation, CDN, link, script, npm, importer, ajouter bootstrap, démarrer, setup, cdn jsdelivr

Retour : [[00 - Index Bootstrap]]

## Méthode 1 : CDN (la plus simple)
À mettre dans ton fichier HTML.

```html
<!-- Dans le <head> : CSS -->
<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet">

<!-- Juste avant </body> : JS (nécessaire pour modal, dropdown, navbar burger...) -->
<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js"></script>
```

> `bootstrap.bundle.min.js` inclut **Popper** (nécessaire pour dropdown, tooltip, popover). Utilise toujours le *bundle*.

## ⚠️ Ne pas oublier la balise viewport
Sans elle, le responsive ne marche pas sur mobile :
```html
<meta name="viewport" content="width=device-width, initial-scale=1">
```

## Méthode 2 : npm
```bash
npm install bootstrap@5.3.3
```
```js
import 'bootstrap/dist/css/bootstrap.min.css';
import 'bootstrap';
```
Avec Sass (pour personnaliser) → voir [[16 - Personnalisation et Dark mode]].

## Méthode 3 : fichiers locaux
Télécharger sur getbootstrap.com → copier `css/` et `js/` dans ton projet → `<link href="css/bootstrap.min.css">`.

## Vérifier que ça marche
```html
<button class="btn btn-primary">Ça marche</button>
```
Si le bouton est bleu et arrondi → OK.

## Problèmes fréquents
| Problème | Cause |
|---|---|
| Rien ne change | Lien CSS cassé ou mal placé |
| Navbar burger / modal ne s'ouvre pas | Script JS bundle manquant |
| Mise en page cassée sur mobile | Balise viewport manquante |
| Dropdown mal placé | Utilisé `bootstrap.min.js` sans Popper → prendre le *bundle* |

Voir aussi : [[18 - Template de départ]]
