---
titre: VLAN, trunk, STP et EtherChannel
formateur: Elimane NDIAYE
niveau: 1re-2e année
tags: [cours, vlan, trunk, stp, etherchannel, switching]
---

# 11 — VLAN, trunk, 802.1Q, STP, EtherChannel

Précédent : [10 - Switch, table MAC et ARP](10%20-%20Switch%2C%20table%20MAC%20et%20ARP.md) · Suivant : [12 - Adressage IPv4](12%20-%20Adressage%20IPv4.md)

## 1. VLAN (Virtual LAN)
Un VLAN **découpe logiquement** un switch en **plusieurs réseaux de couche 2 isolés**, chacun avec son **domaine de broadcast**.

**Pourquoi ?** sécurité (isoler compta / invités / serveurs), réduction du broadcast, organisation par service plutôt que par emplacement.

```text
Switch unique                    VLAN 10 (Compta)   192.168.10.0/24
┌───────────────────────┐        VLAN 20 (RH)       192.168.20.0/24
│ F0/1 F0/2 │ F0/3 F0/4 │  ───►  VLAN 30 (Serveurs) 192.168.30.0/24
│ VLAN10    │ VLAN20    │        Les VLAN ne communiquent qu'via un routeur (L3)
└───────────────────────┘
```
Plage : VLAN **1 à 4094** (1 = par défaut, à éviter pour la production ; 1002-1005 réservés).

## 2. Ports Access et Trunk
- **Access** : appartient à **un seul VLAN**, trames **non taguées** ; pour PC, imprimantes.
- **Trunk** : transporte **plusieurs VLAN** sur un seul lien (switch↔switch, switch↔routeur), trames **taguées** selon **802.1Q**.

### 802.1Q
Ajoute **4 octets** à la trame : **TPID** `0x8100` (16 bits), **PCP** (3 bits, priorité/QoS), **DEI** (1 bit), **VID** (12 bits = numéro de VLAN).
- **VLAN natif** : le VLAN dont les trames **circulent sans tag** sur un trunk (par défaut VLAN 1). Doit être **identique aux deux extrémités**, et idéalement un VLAN inutilisé.

### Configuration Cisco
```text
Switch(config)# vlan 10
Switch(config-vlan)# name COMPTA
Switch(config)# interface fa0/1
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 10
Switch(config)# interface g0/1
Switch(config-if)# switchport mode trunk
Switch(config-if)# switchport trunk allowed vlan 10,20,30
Switch(config-if)# switchport trunk native vlan 99
Switch# show vlan brief
Switch# show interfaces trunk
```
**Erreurs fréquentes** : VLAN non créé, port dans le mauvais VLAN, VLAN natif différent, VLAN absent de la liste autorisée du trunk.

## 3. Boucles et STP
Des liens redondants entre switchs créent des **boucles de couche 2**. Comme une trame Ethernet **n'a pas de TTL**, le broadcast tourne à l'infini → **tempête de broadcast**, table MAC instable, réseau saturé.

**STP (Spanning Tree Protocol, 802.1D)** rend le réseau **logiquement sans boucle** en **bloquant** certains ports.

### Fonctionnement
1. **Élection du Root Bridge** : plus petit **Bridge ID** = *priorité* (32768 par défaut, par pas de 4096) + *MAC*.
2. Sur chaque autre switch : **Root Port** = meilleur chemin (coût le plus faible) vers le root.
3. Sur chaque segment : **Designated Port** = port qui envoie le meilleur chemin.
4. Les autres ports : **Blocking/Alternate** (aucune donnée).

Les switchs échangent des **BPDU** (toutes les 2 s). Coûts classiques : 10 Mbit/s = 100, 100 Mbit/s = 19, 1 Gbit/s = 4, 10 Gbit/s = 2.

| Version | Norme | Convergence |
|---|---|---|
| STP | 802.1D | 30 à 50 s |
| **RSTP** | 802.1w | ~1 à 2 s |
| PVST+/Rapid-PVST+ | Cisco | 1 arbre par VLAN |
| MSTP | 802.1s | groupes de VLAN |

Bonnes pratiques : choisir le root (`spanning-tree vlan 1 priority 4096`), **PortFast** + **BPDU Guard** sur les ports d'accès.
```text
show spanning-tree
```

## 4. EtherChannel / agrégation de liens
Regroupe **plusieurs liens physiques en un lien logique** : plus de **débit** et de **redondance**, sans que STP ne bloque les liens.
- Protocoles : **LACP** (802.3ad / 802.1AX, standard) ; PAgP (Cisco).
- Le trafic est réparti par **hachage** (MAC/IP/ports) : un flux unique ne dépasse pas la vitesse d'**un** lien.
- Les liens doivent avoir la **même configuration** (vitesse, duplex, VLAN).
```text
interface range g0/1-2
 channel-group 1 mode active       # LACP
show etherchannel summary
```
Sous Linux : *bonding* / *teaming*.

## 📌 À retenir
- VLAN = domaine de broadcast logique ; access = 1 VLAN ; trunk = plusieurs (802.1Q, tag 4 octets).
- STP évite les boucles (root bridge, ports bloqués) ; RSTP converge vite.
- EtherChannel/LACP = agrégation de liens.
- Communication entre VLAN → **routage inter-VLAN** ([19 - Routage](19%20-%20Routage.md)).

## 🧪 Exercices
1. Combien de bits pour l'identifiant VLAN dans 802.1Q ? Combien de VLAN utilisables ?
2. Pourquoi une boucle L2 est-elle dangereuse ?
3. Deux switchs, deux liens entre eux, sans STP : que se passe-t-il ?
<details><summary>Réponses</summary>1. 12 bits, 4094. 2. Pas de TTL → broadcasts qui tournent indéfiniment. 3. Tempête de broadcast.</details>

## 🔗 Liens
[10 - Switch, table MAC et ARP](10%20-%20Switch%2C%20table%20MAC%20et%20ARP.md) · [19 - Routage](19%20-%20Routage.md) · [[31 - Architecture d'entreprise et haute disponibilité]] · [00 - Index](00%20-%20Index.md)
