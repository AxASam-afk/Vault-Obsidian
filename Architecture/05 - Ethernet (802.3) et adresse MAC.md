---
titre: Ethernet (802.3) et adresse MAC
formateur: Elimane NDIAYE
niveau: 1re année
tags: [cours, ethernet, mac, couche-2]
---

# 05 — Ethernet (802.3) et adresse MAC

Précédent : [04 - Topologies physiques et logiques](04%20-%20Topologies%20physiques%20et%20logiques.md) · Suivant : [06 - Types de communication (Unicast, Multicast, Broadcast)](06%20-%20Types%20de%20communication%20%28Unicast%2C%20Multicast%2C%20Broadcast%29.md)

## 1. Ethernet (IEEE 802.3)

> [!abstract] Définition
> **Ethernet** est la technologie de **réseau local filaire** la plus utilisée. Elle est normalisée par l'**IEEE** sous la référence **802.3**. Elle définit le **câblage/signal (couche 1)** et le **format des trames avec l'adressage MAC (couche 2)** du modèle OSI.

### Évolution des débits
| Norme | Débit | Support courant |
|---|---|---|
| 10BASE-T | 10 Mbit/s | paires torsadées |
| 100BASE-TX (Fast Ethernet) | 100 Mbit/s | Cat5 |
| 1000BASE-T (Gigabit) | 1 Gbit/s | Cat5e/Cat6 |
| 10GBASE-T | 10 Gbit/s | Cat6a |
| 10GBASE-SR/LR | 10 Gbit/s | fibre |

Longueur maximale d'un segment cuivre : **100 m**.

### Half-duplex / full-duplex
- **Half-duplex** : on émet **ou** on reçoit (hubs). Les collisions sont gérées par **CSMA/CD**.
- **Full-duplex** : émission **et** réception simultanées (switchs). **Plus de collisions.**

## 2. L'adresse MAC

> [!abstract] Définition
> Une **adresse MAC** (Media Access Control) est l'**identifiant de couche 2** d'une **interface réseau** (carte Ethernet, carte Wi-Fi). Elle sert à **livrer les trames à l'intérieur d'un même réseau local**.

- Longueur : **48 bits = 6 octets**, notée en **hexadécimal**.
- Formats : `00:1A:2B:3C:4D:5E` (Linux) ou `00-1A-2B-3C-4D-5E` (Windows).

```text
  00:1A:2B      :   3C:4D:5E
 └──────────┘     └──────────┘
   OUI (24 bits)   Identifiant de l'interface
   = constructeur  (attribué par le constructeur)
```

- **OUI** (Organizationally Unique Identifier) : identifie le **constructeur**.
- Une adresse MAC est **gravée** à la fabrication, mais peut être **modifiée** par logiciel ou **randomisée** (confidentialité sur smartphones).

### Bits particuliers du premier octet
| Bit | Valeur 0 | Valeur 1 |
|---|---|---|
| Bit de poids faible (I/G) | **Unicast** | **Multicast/Broadcast** |
| 2e bit de poids faible (U/L) | Universelle (constructeur) | Localement administrée |

Exemple : `01:00:5E:…` → premier octet `0000 0001` → bit I/G = 1 → **multicast**.

### Adresse MAC vs adresse IP
| | MAC | IP |
|---|---|---|
| Couche | 2 (liaison) | 3 (réseau) |
| Portée | **réseau local** | **de bout en bout**, entre réseaux |
| Longueur | 48 bits | 32 bits (IPv4), 128 (IPv6) |
| Attribuée | par le constructeur | par l'administrateur ou [DHCP](17%20-%20DHCP.md) |

> [!tip] Analogie
> L'adresse **IP** est comme l'**adresse postale complète** (ville, rue). L'adresse **MAC** est comme le **nom sur la sonnette** : elle ne sert que dans l'immeuble (le LAN).

### Afficher son adresse MAC
```bash
ipconfig /all        # Windows  → "Adresse physique"
ip link              # Linux    → "link/ether"
arp -a               # voir les MAC des voisins déjà contactés
```

## 3. La trame Ethernet (aperçu)

```text
┌──────────┬─────┬──────────┬──────────┬──────┬───────────┬─────┐
│ Préambule│ SFD │ MAC dest │ MAC src  │ Type │  Données  │ FCS │
│  7 octets│ 1 o │  6 o     │  6 o     │ 2 o  │ 46–1500 o │ 4 o │
└──────────┴─────┴──────────┴──────────┴──────┴───────────┴─────┘
```
- **Type** : protocole transporté (`0x0800` = IPv4, `0x86DD` = IPv6, `0x0806` = ARP).
- **FCS** : contrôle d'erreurs (CRC).
- Taille de trame : **64 à 1518 octets** (hors préambule/SFD). La **MTU** standard est de **1500 octets**.

## 📌 À retenir
- Ethernet = **IEEE 802.3**, couches 1 et 2.
- MAC = 48 bits, 6 octets hexadécimaux, OUI + identifiant.
- La MAC fonctionne **localement** ; l'IP fonctionne **entre réseaux**.
- Le switch utilise les MAC pour commuter (voir [Switch et table MAC](10%20-%20Switch%2C%20table%20MAC%20et%20ARP.md)).

## 🧪 Exercices
1. Combien d'adresses MAC différentes existent en théorie ? (2⁴⁸ = 281 474 976 710 656)
2. `FF:FF:FF:FF:FF:FF` est-elle une adresse unicast ?
3. Retrouve l'adresse MAC de ta machine et cherche son constructeur via l'OUI.

<details><summary>Réponses</summary>

2. Non, c'est l'adresse de **broadcast**.
</details>

## 🔗 Liens
[06 - Types de communication (Unicast, Multicast, Broadcast)](06%20-%20Types%20de%20communication%20%28Unicast%2C%20Multicast%2C%20Broadcast%29.md) · [04 - Topologies physiques et logiques](04%20-%20Topologies%20physiques%20et%20logiques.md) · [ARP](10%20-%20Switch%2C%20table%20MAC%20et%20ARP.md) · [00 - Index](00%20-%20Index.md)
