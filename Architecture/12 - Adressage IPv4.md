---
titre: Adressage IPv4
formateur: Elimane NDIAYE
niveau: 1re année
tags: [cours, ipv4, adressage]
---

# 12 — Adressage IPv4

Précédent : [11 - VLAN, trunk, STP et EtherChannel](11%20-%20VLAN%2C%20trunk%2C%20STP%20et%20EtherChannel.md) · Suivant : [13 - Subnetting, CIDR et VLSM](13%20-%20Subnetting%2C%20CIDR%20et%20VLSM.md)

## 1. Adresse IPv4
Identifiant **logique** d'une interface, sur **32 bits**, écrit en **4 octets décimaux** séparés par des points (**notation décimale pointée**).

```text
192      .168      .1        .10
11000000 .10101000 .00000001 .00001010
```
Chaque octet : de 0 à 255. Environ **4,3 milliards** d'adresses (2³²) → pénurie → NAT, IPv6.

## 2. Structure : partie réseau + partie hôte
Le **masque de sous-réseau** indique quelle partie de l'adresse identifie le **réseau** (bits à 1) et laquelle identifie l'**hôte** (bits à 0).

```text
IP      192.168.1.10   11000000.10101000.00000001.00001010
Masque  255.255.255.0  11111111.11111111.11111111.00000000
                       └──────── réseau ────────┘└─ hôte ─┘
Réseau  192.168.1.0    (IP AND masque)
```

## 3. Adresses particulières d'un réseau
| Adresse | Rôle | Exemple (192.168.1.0/24) |
|---|---|---|
| **Adresse réseau** | tous les bits hôte à **0** ; identifie le réseau | 192.168.1.**0** |
| **Adresse de broadcast** | tous les bits hôte à **1** | 192.168.1.**255** |
| **Hôtes utilisables** | entre les deux | .1 à .254 (254 hôtes) |
| **Passerelle par défaut** | routeur pour sortir du réseau (souvent 1ʳᵉ ou dernière adresse) | 192.168.1.1 |

Nombre d'hôtes = **2ⁿ − 2** (n = bits hôte).

## 4. Passerelle par défaut
Quand la destination est **hors du réseau** (test : `IP_dest AND masque ≠ réseau local`), le PC envoie à la **passerelle**. Sans passerelle (ou mauvaise) : accès local possible, **pas d'accès extérieur**.

## 5. Classes historiques
| Classe | 1ᵉʳ octet | Masque par défaut | Usage |
|---|---|---|---|
| A | 1–126 | /8 (255.0.0.0) | très grands réseaux |
| B | 128–191 | /16 (255.255.0.0) | moyens |
| C | 192–223 | /24 (255.255.255.0) | petits |
| D | 224–239 | — | multicast |
| E | 240–255 | — | réservé |

Les classes sont **obsolètes** (remplacées par **CIDR**) mais restent du vocabulaire.

## 6. Adresses privées, publiques, spéciales
| Type | Plage | Remarque |
|---|---|---|
| Privées (RFC 1918) | `10.0.0.0/8`, `172.16.0.0/12` (172.16–172.31), `192.168.0.0/16` | non routées sur Internet, réutilisables |
| Publiques | le reste | uniques, attribuées par les RIR/FAI |
| **Loopback** | `127.0.0.0/8` (`127.0.0.1` = localhost) | la machine elle-même |
| **APIPA / link-local** | `169.254.0.0/16` | auto-attribuée quand **DHCP indisponible** |
| CGNAT | `100.64.0.0/10` | NAT opérateur |
| Multicast | `224.0.0.0/4` | voir [06 - Types de communication (Unicast, Multicast, Broadcast)](06%20-%20Types%20de%20communication%20%28Unicast%2C%20Multicast%2C%20Broadcast%29.md) |
| Broadcast limité | `255.255.255.255` | |

> [!warning] Une IP en 169.254.x.x signifie souvent : « DHCP injoignable ».

## 7. Commandes
```bash
ipconfig /all      # Windows
ip a               # Linux
ping 127.0.0.1     # test de la pile TCP/IP locale
```

## 📌 À retenir
- IPv4 : 32 bits, 4 octets. Réseau = IP AND masque.
- Hôtes utilisables : 2ⁿ − 2 (réseau et broadcast exclus).
- Privé : 10/8, 172.16/12, 192.168/16 ; loopback 127/8 ; APIPA 169.254/16.

## 🧪 Exercices
1. `172.20.5.9` est-elle privée ? `172.32.0.1` ?
2. Adresse réseau de `192.168.10.77/24` ?
3. Pourquoi `169.254.12.4` sur un PC est-il un symptôme ?
<details><summary>Réponses</summary>1. Oui (172.16–172.31) ; non. 2. 192.168.10.0. 3. Pas de réponse DHCP → APIPA.</details>

## 🔗 Liens
[13 - Subnetting, CIDR et VLSM](13%20-%20Subnetting%2C%20CIDR%20et%20VLSM.md) · [17 - DHCP](17%20-%20DHCP.md) · [20 - NAT et PAT](20%20-%20NAT%20et%20PAT.md) · [14 - IPv6](14%20-%20IPv6.md) · [00 - Index](00%20-%20Index.md)
