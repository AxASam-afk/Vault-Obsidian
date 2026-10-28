---
tags: [bootstrap, formulaire, form, input, select, checkbox, composant]
aliases: [Formulaires, Formulaire, Form, Input, Champ de texte, Select, Checkbox, Radio, Switch, form-control, form-floating, Validation, input-group, Label flottant, Champ]
---
# Formulaires

**Mots-clés** : formulaire, form, input, champ, texte, email, mot de passe, select, liste déroulante, checkbox, case à cocher, radio, switch, textarea, label, validation, erreur, input-group, floating label, inline, horizontal

Retour : [[00 - Index Bootstrap]]

## Champ de base
```html
<form>
  <div class="mb-3">
    <label for="email" class="form-label">Email</label>
    <input type="email" class="form-control" id="email" placeholder="nom@exemple.com">
  </div>
  <div class="mb-3">
    <label for="msg" class="form-label">Message</label>
    <textarea class="form-control" id="msg" rows="3"></textarea>
  </div>
  <button type="submit" class="btn btn-primary">Envoyer</button>
</form>
```
Le `mb-3` autour de chaque champ = espacement vertical standard.

## Tailles
`form-control-lg` · `form-control-sm`

## Select
```html
<select class="form-select" aria-label="Choix">
  <option selected>Choisir...</option>
  <option value="1">Un</option>
  <option value="2">Deux</option>
</select>
```

## Checkbox / Radio / Switch
```html
<div class="form-check">
  <input class="form-check-input" type="checkbox" id="c1">
  <label class="form-check-label" for="c1">J'accepte</label>
</div>

<div class="form-check">
  <input class="form-check-input" type="radio" name="r" id="r1" checked>
  <label class="form-check-label" for="r1">Option A</label>
</div>

<div class="form-check form-switch">
  <input class="form-check-input" type="checkbox" role="switch" id="s1">
  <label class="form-check-label" for="s1">Activer</label>
</div>
```
En ligne : `form-check form-check-inline`.

## Label flottant
```html
<div class="form-floating mb-3">
  <input type="email" class="form-control" id="fe" placeholder="nom@exemple.com">
  <label for="fe">Adresse email</label>
</div>
```
⚠️ Le `placeholder` est obligatoire pour que ça marche.

## Input group (icône / texte collé)
```html
<div class="input-group mb-3">
  <span class="input-group-text">@</span>
  <input type="text" class="form-control" placeholder="Pseudo">
</div>
<div class="input-group">
  <input type="text" class="form-control" placeholder="Recherche">
  <button class="btn btn-outline-secondary">Go</button>
</div>
```

## Formulaire sur une ligne (grille)
```html
<form class="row g-3">
  <div class="col-md-6">
    <label class="form-label">Prénom</label>
    <input type="text" class="form-control">
  </div>
  <div class="col-md-6">
    <label class="form-label">Nom</label>
    <input type="text" class="form-control">
  </div>
</form>
```

## Désactivé / lecture seule
`<input class="form-control" disabled>` · `readonly`

## Validation
```html
<form class="needs-validation" novalidate>
  <input class="form-control" required>
  <div class="invalid-feedback">Champ obligatoire.</div>
  <div class="valid-feedback">Parfait !</div>
</form>
```
Classes manuelles : `is-valid` · `is-invalid`.

JS pour activer la validation au submit :
```js
document.querySelectorAll('.needs-validation').forEach(form => {
  form.addEventListener('submit', e => {
    if (!form.checkValidity()) { e.preventDefault(); e.stopPropagation(); }
    form.classList.add('was-validated');
  });
});
```

## Fichier, couleur, range
```html
<input class="form-control" type="file">
<input type="color" class="form-control form-control-color">
<input type="range" class="form-range">
```

## Centrer un formulaire
Voir [[03 - Centrer]] (cas n°13).
