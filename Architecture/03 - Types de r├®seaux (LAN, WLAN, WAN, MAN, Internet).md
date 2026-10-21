---
titre: Types de réseaux
formateur: Elimane NDIAYE
niveau: débutant
tags: [cours, réseau, lan, wan, wlan]
---

# 03 — Types de réseaux

Précédent : [[02 - Introduction aux réseaux]] · Suivant : [04 - Topologies physiques et logiques](04%20-%20Topologies%20physiques%20et%20logiques.md)

## 1. Classification par étendue

| Type | Nom complet | Étendue | Exemple |
|---|---|---|---|
| **PAN** | Personal Area Network | quelques mètres | téléphone ↔ écouteurs Bluetooth |
| **LAN** | Local Area Network | une pièce, un bâtiment, un site | réseau d'une salle de TP |
| **WLAN** | Wireless LAN | comme un LAN, en **sans fil** | Wi-Fi d'un bureau |
| **MAN** | Metropolitan Area Network | une ville | réseau d'une université sur plusieurs campus |
| **WAN** | Wide Area Network | pays, monde | liaison entre deux sites d'entreprise |

## 2. LAN — réseau local
Un **LAN** relie des équipements **proches géographiquement**, dans un même lieu, sous la **même administration**.
- Technologie dominante : **Ethernet** ([05 - Ethernet (802.3) et adresse MAC](05%20-%20Ethernet%20%28802.3%29%20et%20adresse%20MAC.md)).
- Débits élevés (1 à 10 Gbit/s, voire plus), latence faible.
- Équipements : switchs, point d'accès, serveurs locaux.

> [!warning] Précision
> Dans le langage courant, « LAN » désigne souvent le réseau **filaire**, mais un LAN peut aussi inclure du Wi-Fi. Techniquement, le LAN se définit par sa **portée locale**, pas par son support.

**Lien avec IP** : un LAN correspond en général à **un réseau IP**, par exemple `192.168.1.0/24`, où les machines communiquent directement sans routeur.

## 3. WLAN — LAN sans fil
Un **WLAN** est un LAN où les appareils se connectent par **ondes radio** (norme **IEEE 802.11**, Wi-Fi) via un **point d'accès**.
- Avantages : mobilité, pas de câblage.
- Limites : interférences, débit partagé, sécurité à soigner (WPA2/WPA3).

## 4. WAN — réseau étendu
Un **WAN** interconnecte des **réseaux distants** (villes, pays), généralement via un **opérateur**.

**Exemple** : une entreprise avec un siège à Paris et une agence à Lyon. Les employés des deux sites utilisent **les mêmes applications** (ERP, messagerie) comme s'ils étaient sur le même réseau.

```text
 LAN Paris                                  LAN Lyon
┌──────────────┐     WAN (opérateur)     ┌──────────────┐
│ PC ─ Switch  ├──► Routeur ═════ Routeur ◄──┤ Switch ─ PC │
│ Serveur ERP  │    (fibre, VPN, MPLS)   │              │
└──────────────┘                         └──────────────┘
```

Supports courants : fibre opérateur, lignes louées, **VPN** sur Internet.
**Internet** est le plus grand WAN : un réseau de réseaux.

## 5. Internet, Intranet, Extranet
| Terme | Définition | Exemple |
|---|---|---|
| **Internet** | réseau **public mondial** de réseaux interconnectés | le Web |
| **Intranet** | réseau **privé** d'une organisation, accessible en interne | portail RH interne |
| **Extranet** | intranet ouvert à des **partenaires** autorisés | portail fournisseurs |

## 6. Réseau privé vs public
- **Adresses privées** (non routables sur Internet) : `10.0.0.0/8`, `172.16.0.0/12`, `192.168.0.0/16`.
- **Adresses publiques** : uniques sur Internet.
- Le passage privé ↔ public se fait par **NAT** (voir [20 - NAT et PAT](20%20-%20NAT%20et%20PAT.md)).

## 📌 À retenir
- LAN = local, WLAN = LAN sans fil, MAN = ville, WAN = distant/étendu.
- Internet = réseau mondial de réseaux ; intranet = privé.
- Un LAN = généralement un réseau IP et un domaine de broadcast.

## 🧪 Exercices
1. Classe : réseau Wi-Fi de la maison, liaison Paris–Dakar d'une entreprise, casque Bluetooth, réseau d'un campus.
2. Pourquoi une entreprise choisit-elle un VPN pour relier deux agences ?
3. Donne un exemple d'extranet.

<details><summary>Réponses</summary>

1. WLAN (ou LAN), WAN, PAN, LAN ou MAN.
2. Pour chiffrer le trafic sur Internet et éviter de louer une ligne dédiée, plus coûteuse.
3. Un portail où les fournisseurs consultent leurs commandes.
</details>

## 🔗 Liens
[[02 - Introduction aux réseaux]] · [04 - Topologies physiques et logiques](04%20-%20Topologies%20physiques%20et%20logiques.md) · [00 - Index](00%20-%20Index.md)
