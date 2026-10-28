# Cours CSS — Les fondamentaux

## 1. Sélection d'éléments

```css
/* Sélection par type */
p { color: #333; }

/* Sélection par classe */
.btn { background: blue; color: white; }

/* Sélection par ID */
#header { padding: 1rem; }
```

## 2. Box Model

```css
.element {
  width: 300px;
  padding: 1rem;     /* Espace interne */
  border: 2px solid #ccc; /* Bordure */
  margin: 2rem;      /* Espace externe */
}
```

- `width`/`height` — Dimensions du contenu
- `padding` — Espace à l'intérieur de la bordure
- `border` — La bordure elle-même
- `margin` — Espace à l'extérieur de l'élément

## 3. Disposition

```css
/* Flexbox */
.container {
  display: flex;
  justify-content: center;   /* Alignment horizontal */
  align-items: center;       /* Alignment vertical */
}

/* Grid */
.grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 1rem;
}
```

## 4. Typographie

```css
body {
  font-family: system-ui, sans-serif;
  line-height: 1.6;
  color: #2c3e50;
}

h1, h2, h3 {
  color: #2c3e50;
}
```

## 5. Sélecteurs avancés

```css
/* Descendant */
.parent .enfant { color: red; }

/ enfant direct */
.parent > .enfant { font-weight: bold; }

/ État
input:focus { border-color: blue; }

/ Pseudo-éléments
button::before { content: "→"; }
```