# Cours PHP — Les fondamentaux

## 1. Syntaxe de base

```php
<?php
// Commentaire sur une ligne

/* Commentaire sur plusieurs lignes */

// Affichage de sortie
echo "Bonjour le monde!";
print "Hello PHP";

// Variables
$nom = "Sam";
$prix = 99.50;

// Types courants
// string, int, float, bool, array, object, null

// Opérateurs
$total = $prix * 1.20; // TVA incluse
```

## 2. Variables et types

```php
$texte = "C'est une chaîne";    // string
$nombre = 42;                   // int
$décimal = 3.14;                // float
$actif = true;                  // bool

// Tableaux
$couleurs = ["rouge", "vert", "bleu"];
$associatif = ["a" => 1, "b" => 2];

// Objets
$utilisateur = new stdClass();
$utilisateur->nom = "Marie";
$utilisateur->email = "marie@example.com";
```

## 3. Conditions et boucles

```php
// Condition
if ($prix > 100) {
  echo "Cher";
} elseif ($prix > 50) {
  echo "Modéré";
} else {
  echo "Pas cher";
}

// Boucle foreach
foreach ($couleurs as $couleur) {
  echo "- $couleur<br>";
}

// Boucle while
$i = 0;
while ($i < 5) {
  echo "$i<br>";
  $i++;
}
```

## 4. Fonctions

```php
// Définition
function additionner($a, $b) {
  return $a + $b;
}

// Appel
$resultat = additionner(5, 3); // 5

// Fonction avec type
function saluer(string $nom): string {
  return "Bonjour " . $nom;
}
```

## 5. Formulaires (traitement)

```php
if ($_SERVER["REQUEST_METHOD"] === "POST") {
  $nom = htmlspecialchars($_POST["nom"]);
  $email = filter_var($_POST["email"], FILTER_VALIDATE_EMAIL);
  
  if ($nom && $email) {
    echo "Merci $nom, votre email $email est validé.";
  }
}
```

## 6. Inclusion de fichiers

```php
<?php include "header.php"; ?>
<?php require "config.php"; ?>
```