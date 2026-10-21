---
titre: Subnetting, CIDR et VLSM
formateur: Elimane NDIAYE
niveau: 2e année
tags: [cours, subnetting, cidr, vlsm, calcul]
---

# 13 — Subnetting, CIDR et VLSM

Précédent : [12 - Adressage IPv4](12%20-%20Adressage%20IPv4.md) · Suivant : [14 - IPv6](14%20-%20IPv6.md)

## 1. Pourquoi découper un réseau ?
Réduire les domaines de broadcast, organiser (par service/site), sécuriser, économiser les adresses.

## 2. CIDR et préfixes
**CIDR** (Classless Inter-Domain Routing) note le masque par un **préfixe** `/n` = nombre de bits à 1.
`192.168.1.0/24` = masque `255.255.255.0`.

## 3. Outils de calcul

### Puissances de 2
| n | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
|---|---|---|---|---|---|---|---|---|---|---|
| 2ⁿ | 2 | 4 | 8 | 16 | 32 | 64 | 128 | 256 | 512 | 1024 |

### Valeurs d'octet de masque
`128, 192, 224, 240, 248, 252, 254, 255` (1, 2, 3, 4, 5, 6, 7, 8 bits à 1)

### Tableau des masques (dernier octet)
| Préfixe | Masque | Bits hôtes | Taille du bloc | Hôtes utilisables |
|---|---|:-:|:-:|:-:|
| /24 | 255.255.255.0 | 8 | 256 | 254 |
| /25 | 255.255.255.128 | 7 | 128 | 126 |
| /26 | 255.255.255.192 | 6 | 64 | 62 |
| /27 | 255.255.255.224 | 5 | 32 | 30 |
| /28 | 255.255.255.240 | 4 | 16 | 14 |
| /29 | 255.255.255.248 | 3 | 8 | 6 |
| /30 | 255.255.255.252 | 2 | 4 | 2 |
| /31 | 255.255.255.254 | 1 | 2 | 2 (liaisons point à point, RFC 3021) |
| /32 | 255.255.255.255 | 0 | 1 | 1 hôte (route hôte) |

## 4. Méthode (à suivre à chaque fois)
1. Écrire le **préfixe** et repérer l'octet « intéressant » (celui où le masque n'est ni 0 ni 255).
2. **Bits hôtes** h = 32 − préfixe.
3. **Taille du bloc** = 2^h (dans l'octet intéressant : 256 − valeur du masque).
4. **Nombre d'hôtes** = 2^h − 2.
5. **Nombre de sous-réseaux** créés = 2^s (s = bits empruntés à la partie hôte).
6. Trouver le **multiple du bloc** immédiatement ≤ à l'IP → **adresse réseau**.
7. **Broadcast** = réseau suivant − 1.
8. **1ʳᵉ utilisable** = réseau + 1 ; **dernière** = broadcast − 1.

## 5. Exemple 1 — Découper 192.168.1.0/24 en 4 sous-réseaux
- Besoin : 4 sous-réseaux → 2² = 4 → **s = 2 bits** empruntés → nouveau préfixe **/26**.
- Bits hôtes : 32 − 26 = 6 → bloc = 2⁶ = **64** ; hôtes = 64 − 2 = **62**.

| Sous-réseau | Réseau | 1ʳᵉ utilisable | Dernière utilisable | Broadcast |
|:-:|---|---|---|---|
| 1 | 192.168.1.0/26 | 192.168.1.1 | 192.168.1.62 | 192.168.1.63 |
| 2 | 192.168.1.64/26 | .65 | .126 | .127 |
| 3 | 192.168.1.128/26 | .129 | .190 | .191 |
| 4 | 192.168.1.192/26 | .193 | .254 | .255 |

## 6. Exemple 2 — Retrouver le réseau d'une IP : 172.16.5.130/27
- /27 → masque 255.255.255.**224** ; bloc = 256 − 224 = **32** ; hôtes = 30.
- Multiples de 32 : 0, 32, 64, 96, **128**, 160 → 130 est dans le bloc de **128**.
- **Réseau** : 172.16.5.128 · **Broadcast** : 172.16.5.159 · **Plage** : .129 à .158.

## 7. Exemple 3 — Combien d'hôtes pour un besoin donné ?
Besoin : **50 postes** → 2^h − 2 ≥ 50 → h = 6 (62) → **/26** (h=5 donne seulement 30).

## 8. VLSM (Variable Length Subnet Mask)
Utiliser des **masques de tailles différentes** selon les besoins réels, pour ne pas gaspiller.

**Règle** : on alloue **du plus grand besoin au plus petit**, en plaçant chaque bloc **à la suite** du précédent.

**Cas** : réseau `192.168.10.0/24`. Besoins : **A = 100 hôtes**, **B = 50**, **C = 25**, **D = liaison point à point (2)**.

| Besoin | Hôtes requis | Bits hôtes | Préfixe | Bloc | Réseau | Plage utilisable | Broadcast |
|---|:-:|:-:|:-:|:-:|---|---|---|
| A | 100 | 7 (126) | /25 | 128 | 192.168.10.0/25 | .1 – .126 | .127 |
| B | 50 | 6 (62) | /26 | 64 | 192.168.10.128/26 | .129 – .190 | .191 |
| C | 25 | 5 (30) | /27 | 32 | 192.168.10.192/27 | .193 – .222 | .223 |
| D | 2 | 2 (2) | /30 | 4 | 192.168.10.224/30 | .225 – .226 | .227 |

Reste libre : `192.168.10.228` à `.255`. Sans VLSM (4 × /26), on aurait gaspillé des adresses, et A (100 hôtes) n'aurait même pas tenu.

## 9. Sur plusieurs octets : 10.10.0.0/20
- /20 → 3ᵉ octet : masque 255.255.**240**.0 ; bloc = 256 − 240 = **16** (dans le 3ᵉ octet).
- Hôtes : 32 − 20 = 12 bits → 2¹² − 2 = **4094**.
- Réseau 10.10.0.0 ; broadcast 10.10.**15**.255 ; plage 10.10.0.1 – 10.10.15.254.

## 10. Agrégation (supernetting)
Regrouper plusieurs réseaux contigus en **une seule route** : `192.168.0.0/24`, `.1.0/24`, `.2.0/24`, `.3.0/24` → **192.168.0.0/22** (4 blocs alignés).

## 📌 À retenir
- Hôtes = 2^h − 2 ; bloc = 2^h ; réseau = multiple du bloc ; broadcast = réseau suivant − 1.
- VLSM : trier les besoins du **plus grand au plus petit**.
- /30 pour les liens point à point (2 hôtes).

## 🧪 Exercices progressifs
1. Pour `192.168.1.100/24` : réseau, broadcast, nombre d'hôtes.
2. Pour `10.0.0.77/28` : réseau, broadcast, plage.
3. Découper `192.168.5.0/24` en **8** sous-réseaux : préfixe, bloc, hôtes, liste des réseaux.
4. Quel préfixe pour **500 hôtes** ?
5. VLSM sur `172.16.0.0/24` pour 60, 28, 12 et 2 hôtes.

<details><summary>Corrigés</summary>

1. Réseau 192.168.1.0, broadcast .255, 254 hôtes.
2. /28 → bloc 16 ; 77 → 64 ; réseau 10.0.0.64, broadcast 10.0.0.79, plage .65–.78.
3. 2³ = 8 → /27 ; bloc 32 ; 30 hôtes ; réseaux .0, .32, .64, .96, .128, .160, .192, .224.
4. 2⁹ − 2 = 510 ≥ 500 → **/23**.
5. 60→/26 : 172.16.0.0/26 ; 28→/27 : 172.16.0.64/27 ; 12→/28 : 172.16.0.96/28 ; 2→/30 : 172.16.0.112/30.
</details>

## 🔗 Liens
[12 - Adressage IPv4](12%20-%20Adressage%20IPv4.md) · [19 - Routage](19%20-%20Routage.md) · [11 - VLAN, trunk, STP et EtherChannel](11%20-%20VLAN%2C%20trunk%2C%20STP%20et%20EtherChannel.md) · [00 - Index](00%20-%20Index.md)
