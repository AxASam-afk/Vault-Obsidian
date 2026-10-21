# Rapport de modélisation - Application TravelNow

## 1. Modèle Conceptuel de Données (MCD)

### Entités

#### Utilisateur

| Attribut | Type | Description |
|----------|------|-------------|
| idUtilisateur | INT (PK) | Identifiant unique |
| email | VARCHAR(255) | Adresse e-mail |
| motDePasse | VARCHAR(255) | Mot de passe hashé |
| nom | VARCHAR(100) | Nom de famille |
| prenom | VARCHAR(100) | Prénom |
| dateNaissance | DATE | Date de naissance |

#### Service

| Attribut | Type | Description |
|----------|------|-------------|
| idService | INT (PK) | Identifiant unique |
| type | ENUM('vol','hébergement','activite') | Type de service |
| libellé | VARCHAR(255) | Nom ou libellé du service |
| destination | VARCHAR(100) | Destination (ex: Paris, Rome) |
| prixBase | DECIMAL(10,2) | Prix de base du service |

#### Réservation

| Attribut | Type | Description |
|----------|------|-------------|
| idReservation | INT (PK) | Identifiant unique |
| idUtilisateur | INT (FK) | Référence à l'utilisateur |
| dateReservation | DATE | Date de la réservation |
| statut | ENUM('en option','confirmée','annulée') | Statut de la réservation |
| montantTotal | DECIMAL(10,2) | Montant total de la réservation |
| devise | CHAR(3) | Devise (ex: EUR, USD) |

#### LigneReservation

| Attribut | Type | Description |
|----------|------|-------------|
| idLigne | INT (PK) | Identifiant unique |
| idReservation | INT (FK) | Référence à la réservation |
| idService | INT (FK) | Référence au service |
| quantité | INT | Quantité de ce service (nombre d'unités) |
| prixUnitaire | DECIMAL(10,2) | Prix unitaire au moment de la réservation |

#### Paiement

| Attribut | Type | Description |
|----------|------|-------------|
| idPaiement | INT (PK) | Identifiant unique |
| idReservation | INT (FK) | Référence à la réservation |
| montant | DECIMAL(10,2) | Montant du paiement |
| datePaiement | DATE | Date du paiement |
| modePaiement | VARCHAR(50) | Mode de paiement (carte, virement, etc.)

### Relations et cardinalités

- Un Utilisateur peut effectuer 0,N Réservations (0..N)
- Une Réservation appartient à un seul Utilisateur (1)
- Une Réservation comprend 1,N Lignes de réservation (1..N)
- Une Ligne de réservation fait référence à exactement un Service (1)
- Un Service peut apparaître dans 0,N Lignes de réservation (0..N)
- Une Réservation peut avoir 0,N Paiements (0..N)
- Un Paiement est lié à exactement une Réservation (1)

### Justification des choix

- La séparation entre Service et LigneReservation permet de garder l'historique des prix (prixUnitaire) même si le prix de base du service change.
- Le statut de la réservation suit le cycle de vie typique (option, confirmée, annulée).
- Le montant total de la réservation peut être calculé à partir des lignes, mais stocké pour performance.
- Les paiements multiples permettent acomptes et soldes.

## 2. Requêtes SQL pour l'agence

### Définir de nouveaux services
```sql
INSERT INTO Service (type, libellé, destination, prixBase)
VALUES ('vol', 'Paris - New York', 'New York', 450.00);
```

### Modifier les services existants
```sql
UPDATE Service
SET prixBase = 480.00
WHERE idService = 12;
```

### Visualiser les réservations par utilisateur pour une période donnée
```sql
SELECT r.idReservation, r.dateReservation, r.statut, r.montantTotal, r.devise,
       u.nom, u.prenom
FROM Reservation r
JOIN Utilisateur u ON r.idUtilisateur = u.idUtilisateur
WHERE u.idUtilisateur = :idUtilisateur
  AND r.dateReservation BETWEEN :dateDebut AND :dateFin
ORDER BY r.dateReservation DESC;
```

## 3. Requêtes SQL pour l'utilisateur

### Créer un compte sur la plateforme
```sql
INSERT INTO Utilisateur (email, motDePasse, nom, prenom, dateNaissance)
VALUES ('john.doe@example.com', 'motdepasse sécurisé', 'Doe', 'John', '1990-05-15');
```

### Créer une réservation (avec ses lignes de réservation)
```sql
-- Début de transaction
START TRANSACTION;

-- 1. Créer la réservation
INSERT INTO Reservation (idUtilisateur, dateReservation, statut, montantTotal, devise)
VALUES (:idUtilisateur, CURDATE(), 'en option', 0, 'EUR');

SET @idReservation = LAST_INSERT_ID();

-- 2. Insérer chaque ligne de réservation et accumuler le total
INSERT INTO LigneReservation (idReservation, idService, quantité, prixUnitaire)
VALUES (@idReservation, :idService1, 1, (SELECT prixBase FROM Service WHERE idService = :idService1)),
       (@idReservation, :idService2, 2, (SELECT prixBase FROM Service WHERE idService = :idService2));

-- 3. Mettre à jour le montant total de la réservation (somme des lignes)
UPDATE Reservation
SET montantTotal = (
    SELECT SUM(lr.quantité * lr.prixUnitaire)
    FROM LigneReservation lr
    WHERE lr.idReservation = @idReservation
)
WHERE idReservation = @idReservation;

COMMIT;
```

### Annuler une réservation
```sql
UPDATE Reservation
SET statut = 'annulée'
WHERE idReservation = :idReservation AND idUtilisateur = :idUtilisateur;
```

### Effectuer un paiement
```sql
INSERT INTO Paiement (idReservation, montant, datePaiement, modePaiement)
VALUES (:idReservation, :montant, CURDATE(), 'carte');
```


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