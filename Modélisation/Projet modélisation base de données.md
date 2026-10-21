---
titre: Modélisation de Base de Données
cours: Bases de Données
niveau: 1re et 2e année (informatique)
tags: [bases de données, modélisation, SQL, Merise]
---

# Modélisation de Base de Données

> [!info] À propos du cours
> Ce cours couvre les principes de la modélisation conceptuelle et logique des bases de données, en utilisant la méthode Merise et le langage SQL pour l'implémentation.

## 🧩 Partie 1 : Modélisation Conceptuelle (Merise)

### 1.1 Le Modèle Conceptuel des Données (MCD)

Le **MCD** représente les données de manière abstraite, indépendamment de tout système de gestion de base de données particulier.

#### Entités
Une **entité** est un objet du monde réel que l'on veut représenter dans la base de données.

**Exemples d'entités :**
- Client
- Produit
- Commande
- Employé

#### Associations
Une **association** représente une relation entre deux ou plusieurs entités.

**Cardinalités :**
- 0,1 : zéro ou une occurrence
- 1,1 : exactement une occurrence
- 0,n : zéro, une ou plusieurs occurrences
- 1,n : une ou plusieurs occurrences

> [!example] Exemple d'association
> Une **Client** peut passer **0,n** **Commande**  
> Une **Commande** est passée par **1,1** **Client**

### 1.2 Le Modèle Conceptuel des Traitements (MCT)

Le **MCT** représente les traitements à appliquer sur les données.

#### Flux de données
Un **flux** représente le mouvement de données entre entités et traitements.

#### Treillis de décision
Un **treillis de décision** représente les différents traitements possibles selon des conditions.

## Partie 2 : Modélisation Logique

### 2.1 Passage du MCD au Modèle Logique des Données (MLD)

Le **MLD** adapte le MCD à un type spécifique de SGBD (relationnel, hiérarchique, etc.).

#### Règles de passage Merise vers Relationnel :
1. **Entité** → Table
2. **Propriété simple** → Attribut
3. **Clé primaire** → Identifiant de l'entité
4. **Association 1,1** → Fusion des tables ou clé étrangère
5. **Association 1,n** → Clé étrangère dans la table du côté "n"
6. **Association 0,n ou 1,n** → Même règle que 1,n
7. **Association n,m** → Table d'association

### 2.2 Normalisation

La **normalisation** vise à réduire les redondances et améliorer l'intégrité des données.

#### Formes normales :
- **1NF** : Chaque cellule contient une valeur atomique
- **2NF** : 1NF + pas de dépendance partielle par rapport à la clé
- **3NF** : 2NF + pas de dépendance transitive
- **BCNF** : Version renforcée de la 3NF

> [!warning] Dénormalisation
> Parfois, on dénomalise intentionnellement pour améliorer les performances de lecture, au prix de redondances contrôlées.

## Partie 3 : Implémentation SQL

### 3. Création des tables

```sql
-- Table Client
CREATE TABLE Client (
    client_id INT PRIMARY KEY AUTO_INCREMENT,
    nom VARCHAR(50) NOT NULL,
    prenom VARCHAR(50) NOT NULL,
    email VARCHAR(100) UNIQUE,
    telephone VARCHAR(20),
    date_inscription DATE NOT NULL
);

-- Table Produit
CREATE TABLE Produit (
    produit_id INT PRIMARY KEY AUTO_INCREMENT,
    reference VARCHAR(20) UNIQUE NOT NULL,
    designation VARCHAR(100) NOT NULL,
    prix_unitaire DECIMAL(10,2) NOT NULL CHECK (prix_unitaire >= 0),
    stock INT NOT NULL DEFAULT 0,
    categorie VARCHAR(50)
);

-- Table Commande
CREATE TABLE Commande (
    commande_id INT PRIMARY KEY AUTO_INCREMENT,
    client_id INT NOT NULL,
    date_commande DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
    statut ENUM('en cours', 'expédiée', 'livrée', 'annulée') DEFAULT 'en cours',
    total DECIMAL(10,2) NOT NULL DEFAULT 0,
    FOREIGN KEY (client_id) REFERENCES Client(client_id)
);

-- Table LigneCommande (association n:m entre Commande et Produit)
CREATE TABLE LigneCommande (
    ligne_id INT PRIMARY KEY AUTO_INCREMENT,
    commande_id INT NOT NULL,
    produit_id INT NOT NULL,
    quantite INT NOT NULL CHECK (quantite > 0),
    prix_unitaire DECIMAL(10,2) NOT NULL,
    sous_total DECIMAL(10,2) GENERATED ALWAYS AS (quantite * prix_unitaire) STORED,
    FOREIGN KEY (commande_id) REFERENCES Commande(commande_id),
    FOREIGN KEY (produit_id) REFERENCES Produit(produit_id)
);
```



---

**Commande ↔ Client** (`Commande_client_id`)

- Côté Commande : **1,1** → une commande appartient à **exactement un** client.
- Côté Client : **0,N** → un client peut avoir **zéro, une ou plusieurs** commandes (un nouveau client n’a pas encore commandé).

**Paiement ↔ Commande** (`Paiement_commande_id`)

- Côté Paiement : **1,1** → un paiement concerne **exactement une** commande.
- Côté Commande : **0,N** → une commande peut avoir **zéro, un ou plusieurs** paiements (pas encore payée, ou payée en plusieurs fois).

**LigneCommande ↔ Commande** (`LigneCommande_commande_id`)

- Côté LigneCommande : **1,1** → une ligne appartient à **exactement une** commande.
- Côté Commande : **0,N** → une commande contient **plusieurs** lignes.

**LigneCommande ↔ Produit** (`LigneCommande_produit_id`)

- Côté LigneCommande : **1,1** → une ligne concerne **exactement un** produit.
- Côté Produit : **0,N** → un produit peut apparaître dans **plusieurs** lignes (ou dans aucune s’il n’a jamais été commandé).
## Liens utiles

- [[Modélisation Merise - Cours Complet]]
- [SQL - Langage de Requêtage](../Language%20C/SQL/SQL%20-%20Langage%20de%20Requ%C3%AAtage.md)
- [[Normalisation des Bases de Données]]
- [[Transactions et Concurrence]]

## À retenir

1. **Merise** sépare clairement les données (MCD/MLD) des traitements (MCT/M LT)
2. La **normalisation** réduit les redondances mais peut nécessitant des jointures coûteuses
3. En SQL, toujours définir les **contraintes d'intégrité** (clés primaires/étrangères, CHECK, UNIQUE)
4. Penser à l'**indexation** des colonnes fréquemment utilisées dans les WHERE/JOIN
5. Documenter le **modèle de données** est aussi important que l'implémentation

> [!NOTE] Prochain cours
> Nous verrons les **vues**, les **procédures stockées** et les **déclencheurs** pour encapsuler la logique métier dans la base de données.