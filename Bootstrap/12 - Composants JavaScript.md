---
tags: [bootstrap, javascript, modal, collapse, dropdown, carousel, toast, tooltip, popover, offcanvas, accordion, composant]
aliases: [Modal, Modale, Fenêtre popup, Popup, Collapse, Accordéon, Accordion, Dropdown, Menu déroulant, Carousel, Carrousel, Toast, Tooltip, Info-bulle, Popover, Offcanvas, Sidebar, Onglets, Tabs, Alert, Alerte, Spinner, Badge, Progress, Barre de progression]
---
# Composants (avec JavaScript)

**Mots-clés** : modal, popup, fenêtre, collapse, déplier, replier, accordéon, dropdown, menu déroulant, carousel, slider, toast, notification, tooltip, info-bulle, popover, offcanvas, sidebar, tabs, onglets, alert, alerte, badge, progress, spinner, data-bs-toggle, data-bs-target

Retour : [[00 - Index Bootstrap]]

> Tous ces composants demandent `bootstrap.bundle.min.js`. Voir [[01 - Installation]].
> Principe : on utilise des attributs `data-bs-toggle` + `data-bs-target`, **pas besoin d'écrire du JS**.

## Modal (popup)
```html
<button class="btn btn-primary" data-bs-toggle="modal" data-bs-target="#maModal">Ouvrir</button>

<div class="modal fade" id="maModal" tabindex="-1">
  <div class="modal-dialog modal-dialog-centered">
    <div class="modal-content">
      <div class="modal-header">
        <h5 class="modal-title">Titre</h5>
        <button class="btn-close" data-bs-dismiss="modal"></button>
      </div>
      <div class="modal-body">Contenu</div>
      <div class="modal-footer">
        <button class="btn btn-secondary" data-bs-dismiss="modal">Fermer</button>
        <button class="btn btn-primary">Valider</button>
      </div>
    </div>
  </div>
</div>
```
- Centré verticalement : `modal-dialog-centered`
- Scroll interne : `modal-dialog-scrollable`
- Tailles : `modal-sm`, `modal-lg`, `modal-xl`, `modal-fullscreen`

Via JS :
```js
const m = new bootstrap.Modal('#maModal');
m.show(); m.hide();
```

## Collapse (déplier / replier)
```html
<button class="btn btn-primary" data-bs-toggle="collapse" data-bs-target="#zone">Afficher</button>
<div class="collapse" id="zone">
  <div class="card card-body">Contenu caché</div>
</div>
```

## Accordéon
```html
<div class="accordion" id="acc">
  <div class="accordion-item">
    <h2 class="accordion-header">
      <button class="accordion-button" data-bs-toggle="collapse" data-bs-target="#a1">Question 1</button>
    </h2>
    <div id="a1" class="accordion-collapse collapse show" data-bs-parent="#acc">
      <div class="accordion-body">Réponse 1</div>
    </div>
  </div>
</div>
```
`data-bs-parent` = un seul ouvert à la fois. Sans lui → plusieurs possibles. `accordion-flush` = sans bordures.

## Dropdown (menu déroulant)
```html
<div class="dropdown">
  <button class="btn btn-secondary dropdown-toggle" data-bs-toggle="dropdown">Menu</button>
  <ul class="dropdown-menu">
    <li><a class="dropdown-item" href="#">Action</a></li>
    <li><hr class="dropdown-divider"></li>
    <li><a class="dropdown-item" href="#">Autre</a></li>
  </ul>
</div>
```
Direction : `dropup`, `dropend`, `dropstart`. Aligné à droite : `dropdown-menu-end`.

## Carousel
```html
<div id="car" class="carousel slide" data-bs-ride="carousel">
  <div class="carousel-inner">
    <div class="carousel-item active"><img src="1.jpg" class="d-block w-100" alt=""></div>
    <div class="carousel-item"><img src="2.jpg" class="d-block w-100" alt=""></div>
  </div>
  <button class="carousel-control-prev" data-bs-target="#car" data-bs-slide="prev">
    <span class="carousel-control-prev-icon"></span>
  </button>
  <button class="carousel-control-next" data-bs-target="#car" data-bs-slide="next">
    <span class="carousel-control-next-icon"></span>
  </button>
</div>
```
Au moins un `.carousel-item` doit avoir `active`.

## Tabs (onglets)
```html
<ul class="nav nav-tabs" role="tablist">
  <li class="nav-item"><button class="nav-link active" data-bs-toggle="tab" data-bs-target="#t1">Un</button></li>
  <li class="nav-item"><button class="nav-link" data-bs-toggle="tab" data-bs-target="#t2">Deux</button></li>
</ul>
<div class="tab-content">
  <div class="tab-pane fade show active" id="t1">Contenu 1</div>
  <div class="tab-pane fade" id="t2">Contenu 2</div>
</div>
```
Variante pilules : `nav-pills`.

## Offcanvas (panneau latéral)
```html
<button class="btn btn-primary" data-bs-toggle="offcanvas" data-bs-target="#side">Ouvrir</button>
<div class="offcanvas offcanvas-start" id="side" tabindex="-1">
  <div class="offcanvas-header">
    <h5 class="offcanvas-title">Menu</h5>
    <button class="btn-close" data-bs-dismiss="offcanvas"></button>
  </div>
  <div class="offcanvas-body">Contenu</div>
</div>
```
Côtés : `offcanvas-start | end | top | bottom`.

## Alert
```html
<div class="alert alert-success alert-dismissible fade show" role="alert">
  Bravo !
  <button class="btn-close" data-bs-dismiss="alert"></button>
</div>
```
Couleurs : `alert-primary | success | danger | warning | info`.

## Toast (notification)
```html
<div class="toast-container position-fixed bottom-0 end-0 p-3">
  <div id="monToast" class="toast" role="alert">
    <div class="toast-header">
      <strong class="me-auto">Info</strong>
      <button class="btn-close" data-bs-dismiss="toast"></button>
    </div>
    <div class="toast-body">Message envoyé.</div>
  </div>
</div>
<script>
  bootstrap.Toast.getOrCreateInstance('#monToast').show();
</script>
```

## Tooltip / Popover : ⚠️ à activer en JS
```html
<button class="btn btn-secondary" data-bs-toggle="tooltip" title="Info-bulle">Survole-moi</button>
<script>
  document.querySelectorAll('[data-bs-toggle="tooltip"]').forEach(el => new bootstrap.Tooltip(el));
</script>
```
Contrairement au modal, tooltip et popover ne s'activent **pas** tout seuls.

## Badge, progress, spinner
```html
<span class="badge text-bg-primary">Nouveau</span>
<span class="badge rounded-pill text-bg-danger">3</span>

<div class="progress"><div class="progress-bar" style="width: 60%">60 %</div></div>
<div class="progress-bar progress-bar-striped progress-bar-animated" style="width: 40%"></div>

<div class="spinner-border text-primary" role="status"></div>
<div class="spinner-grow text-danger"></div>
```

## Pagination, breadcrumb, list-group
```html
<nav><ul class="pagination">
  <li class="page-item"><a class="page-link" href="#">1</a></li>
  <li class="page-item active"><a class="page-link" href="#">2</a></li>
</ul></nav>

<nav><ol class="breadcrumb">
  <li class="breadcrumb-item"><a href="#">Accueil</a></li>
  <li class="breadcrumb-item active">Page</li>
</ol></nav>

<ul class="list-group">
  <li class="list-group-item active">Actif</li>
  <li class="list-group-item">Item</li>
</ul>
```

## Pourquoi ça ne marche pas ?
| Problème | Cause |
|---|---|
| Rien ne s'ouvre | Script bundle manquant |
| Mauvaise cible | `data-bs-target` ≠ `id` (oublier le `#`) |
| Tooltip invisible | Pas initialisé en JS |
| Carousel vide | Aucun `.carousel-item.active` |
| Modal derrière le fond gris | Modal placée dans un parent avec `position`/`z-index`. La mettre juste avant `</body>` |
