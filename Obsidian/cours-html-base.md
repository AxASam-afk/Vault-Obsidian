# Cours HTML — Les fondamentaux

## 1. Structure de base d'une page HTML

```markdown
<!DOCTYPE html>
<html lang="fr">
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Mon site</title>
  </head>
  <body>
    <!-- Contenu ici -->
  </body>
</html>
```

- `<!DOCTYPE html>` — Déclare le type de document
- `<html lang="fr">` — Racine de la page (français)
- `<head>` — Métadonnées (charset, viewport, titre)
- `<body>` — Contenu visible de la page

## 2. Balises essentielles

### Titres

```markdown
# Titre principal (h1)
## Sous-titre (h2)
### Sous-sous-titre (h3)
```

### Paragraphes et texte

```markdown
Ceci est un paragraphe.

**Gras** (importance sémantique)
*Italique* (emphasis)
```

### Liens et images

```markdown
[Texte du lien](https://example.com "Titre optionnel")

![Description de l'image](image.jpg "Description alternative")
```

### Listes

```markdown
- Élément 1
- Élément 2

1. Premier
2. Second
```

### Tableaux

```markdown
| En-tête 1 | En-tête 2 |
|-----------|-----------|
| Cellule 1 | Cellule 2 |
```

### Formulaires

```markdown
```html
<form action="/inscription" method="post">
  <label for="nom">Nom :</label>
  <input type="text" id="nom" name="nom" required><br><br>
  <label for="email">Email :</label>
  <input type="email" id="email" name="email" required><br><br>
  <button type="submit">Envoyer</button>
</form>
```
```

## 3. Attributs courants

| Attribut | Usage |
|----------|-------|
| `class` | CSS (ex: `class="btn-primary"`) |
| `id` | Identifiant unique (ex: `id="header"`) |
| `style` | CSS inline (ex: `style="color: red;"`) |
| `src` | Source pour images/ressources |
| `alt` | Texte alternatif pour l'accessibilité |
| `href` | URL pour les liens |
| `target` | Où ouvrir le lien (`_blank`, `_self`) |

## 4. Rappels

⚠️ Les navigateurs affichage varient ; testez sur plusieurs.
Les attributs `width`/`height` sur `<img>` aident le layout.

Ce cours est un aperçu rapide — approfondissez sur [MDN Web Docs](https://developer.mozilla.org/) ou [w3schools](https://www.w3schools.com/).