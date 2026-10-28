---
tags: [bootstrap, template, starter, boilerplate, html, copier-coller]
aliases: [Template, Template Bootstrap, Squelette HTML, Starter, Boilerplate, Page de départ, Base HTML, Hello World Bootstrap, Modèle de page]
---
# Template de départ (copier-coller)

**Mots-clés** : template, squelette, starter, boilerplate, html de base, démarrer un projet, page vide, hello world, modèle, base

Retour : [[00 - Index Bootstrap]] · Installation : [[01 - Installation]]

## Page HTML complète Bootstrap 5.3
```html
<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Ma page Bootstrap</title>
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet">
</head>
<body>

  <!-- Navbar : voir [[10 - Navbar]] -->
  <nav class="navbar navbar-expand-lg bg-body-tertiary">
    <div class="container">
      <a class="navbar-brand" href="#">MonSite</a>
      <button class="navbar-toggler" data-bs-toggle="collapse" data-bs-target="#menu">
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="menu">
        <ul class="navbar-nav ms-auto">
          <li class="nav-item"><a class="nav-link active" href="#">Accueil</a></li>
          <li class="nav-item"><a class="nav-link" href="#contact">Contact</a></li>
        </ul>
      </div>
    </div>
  </nav>

  <!-- Hero -->
  <header class="py-5 bg-primary text-white text-center">
    <div class="container">
      <h1 class="display-4 fw-bold">Bienvenue</h1>
      <p class="lead">Une page propre avec Bootstrap.</p>
      <a href="#cartes" class="btn btn-light btn-lg">Découvrir</a>
    </div>
  </header>

  <!-- Cartes -->
  <main class="container my-5" id="cartes">
    <div class="row row-cols-1 row-cols-md-3 g-4">
      <div class="col">
        <div class="card h-100 shadow-sm">
          <div class="card-body">
            <h5 class="card-title">Carte 1</h5>
            <p class="card-text">Contenu.</p>
          </div>
        </div>
      </div>
      <div class="col">
        <div class="card h-100 shadow-sm">
          <div class="card-body">
            <h5 class="card-title">Carte 2</h5>
            <p class="card-text">Contenu.</p>
          </div>
        </div>
      </div>
      <div class="col">
        <div class="card h-100 shadow-sm">
          <div class="card-body">
            <h5 class="card-title">Carte 3</h5>
            <p class="card-text">Contenu.</p>
          </div>
        </div>
      </div>
    </div>
  </main>

  <!-- Footer -->
  <footer class="bg-dark text-white text-center py-3" id="contact">
    <small>© 2026 MonSite</small>
  </footer>

  <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js"></script>
</body>
</html>
```
> Dans Obsidian, les `[[liens]]` à l'intérieur du bloc de code ne sont pas actifs. Ce sont juste des rappels.

## Page centrée (login)
```html
<body class="bg-body-tertiary">
  <div class="container d-flex justify-content-center align-items-center min-vh-100">
    <div class="card shadow p-4" style="width: 22rem;">
      <h3 class="text-center mb-3">Connexion</h3>
      <input class="form-control mb-3" type="email" placeholder="Email">
      <input class="form-control mb-3" type="password" placeholder="Mot de passe">
      <button class="btn btn-primary w-100">Se connecter</button>
    </div>
  </div>
</body>
```
Voir [[03 - Centrer]] et [[09 - Formulaires]].
