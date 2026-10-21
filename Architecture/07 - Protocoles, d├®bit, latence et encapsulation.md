---
titre: Protocoles, performances et encapsulation
formateur: Elimane NDIAYE
niveau: débutant
tags: [cours, protocole, débit, latence, encapsulation]
---

# 07 — Protocoles, performances et encapsulation

Précédent : [06 - Types de communication (Unicast, Multicast, Broadcast)](06%20-%20Types%20de%20communication%20%28Unicast%2C%20Multicast%2C%20Broadcast%29.md) · Suivant : [[08 - Modèle OSI]]

## 1. Notion de protocole
> [!abstract] Définition
> Un **protocole** est un **ensemble de règles** qui définissent comment deux équipements communiquent : format des messages, ordre des échanges, gestion des erreurs.

**Analogie** : deux personnes doivent parler la même langue et respecter des règles (« bonjour », attendre son tour). Sans protocole commun, pas de communication.

| Exemple | Rôle |
|---|---|
| Ethernet | livrer des trames dans un LAN |
| IP | adresser et acheminer les paquets |
| TCP | transport fiable |
| HTTP | échanger des pages web |
| DNS | traduire des noms en adresses IP |

Une **suite de protocoles** (ex. TCP/IP) est un ensemble de protocoles qui coopèrent. Les protocoles sont normalisés par l'**IEEE**, l'**IETF** (RFC), l'**ISO**.

## 2. Bande passante, débit, latence, jitter

| Notion | Définition | Unité | Analogie (autoroute) |
|---|---|---|---|
| **Bande passante** | capacité **maximale théorique** d'un lien | bit/s | nombre de voies |
| **Débit** (throughput) | quantité **réellement** transférée | bit/s | voitures qui passent réellement |
| **Latence** | délai d'acheminement d'un paquet | ms | temps de trajet |
| **Jitter** | **variation** de la latence | ms | trajets irréguliers |
| **Perte de paquets** | paquets qui n'arrivent pas | % | voitures perdues |

- 1 octet = 8 bits. **Mbit/s ≠ Mo/s** : 100 Mbit/s ≈ 12,5 Mo/s.
- Temps de transfert = taille / débit. Un fichier de 1 Go (8 000 Mbit) sur 100 Mbit/s : ≈ **80 s** au mieux.
- La voix sur IP (VoIP) exige une **faible latence** (< 150 ms) et un **faible jitter**.
- Le **débit réel** est inférieur à la bande passante (surcharge des en-têtes, congestion, Wi-Fi partagé).

```bash
ping -c 4 8.8.8.8      # latence (RTT) et perte
iperf3 -c serveur      # mesure du débit
```

## 3. Encapsulation et désencapsulation

> [!abstract] Principe
> Pour envoyer des données, chaque couche **ajoute son en-tête** (encapsulation). À la réception, chaque couche **retire son en-tête** (désencapsulation).

```text
Émetteur                                          Récepteur
Application : DONNÉES                             DONNÉES  ▲
Transport   : [TCP  | DONNÉES]  = segment         ▲ retire en-tête TCP
Réseau      : [IP | TCP | DONNÉES] = paquet       ▲ retire en-tête IP
Liaison     : [ETH | IP | TCP | DONNÉES | FCS] = trame  ▲ retire en-tête Ethernet
Physique    : 0101101… (bits)  ───────────────────►
```

| Couche | Nom de l'unité (PDU) | Adressage |
|---|---|---|
| Application | données / message | — |
| Transport | **segment** (TCP) / **datagramme** (UDP) | **ports** |
| Réseau | **paquet** | **adresses IP** |
| Liaison | **trame** | **adresses MAC** |
| Physique | **bits** | — |

## 📌 À retenir
- Protocole = règles de communication.
- Bande passante (capacité) ≥ débit (réel) ; latence = délai ; jitter = variation du délai.
- Encapsulation : Données → segment → paquet → trame → bits.

## 🧪 Exercices
1. Combien de temps pour transférer 500 Mo sur un lien de 100 Mbit/s (débit idéal) ?
2. Quel PDU contient des adresses IP ? Des adresses MAC ?

<details><summary>Réponses</summary>

1. 500 Mo = 4 000 Mbit → 4 000/100 = **40 s**.
2. Le paquet ; la trame.
</details>

## 🔗 Liens
[[08 - Modèle OSI]] · [[09 - Modèle TCP-IP et communication complète]] · [00 - Index](00%20-%20Index.md)
