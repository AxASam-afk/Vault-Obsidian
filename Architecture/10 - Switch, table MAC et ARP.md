---
titre: Switch, table MAC et ARP
formateur: Elimane NDIAYE
niveau: 1re année
tags: [cours, switch, mac, arp, broadcast]
---

# 10 — Switch, table MAC, domaines et ARP

Précédent : [[09 - Modèle TCP-IP et communication complète]] · Suivant : [11 - VLAN, trunk, STP et EtherChannel](11%20-%20VLAN%2C%20trunk%2C%20STP%20et%20EtherChannel.md)

## 1. Le switch (commutateur)
Équipement de **couche 2** qui relie les machines d'un LAN et **transmet les trames uniquement vers le bon port**, en s'appuyant sur les **adresses MAC**. Différence avec un **hub** : le hub répète tout partout (collisions).

## 2. La table MAC (CAM)
Le switch **apprend** : pour chaque trame reçue, il associe **MAC source → port d'arrivée**.

| MAC | Port | VLAN |
|---|---|---|
| AAAA.AAAA.AAAA | Fa0/1 | 1 |
| BBBB.BBBB.BBBB | Fa0/2 | 1 |

Chaque entrée expire (~300 s par défaut) si elle n'est plus vue.

### Les 4 actions du switch
1. **Learning** : apprend la MAC source.
2. **Forwarding** : MAC destination connue → envoi **sur le port concerné**.
3. **Flooding** : MAC destination **inconnue** (ou broadcast/multicast) → envoi sur **tous les ports sauf celui d'entrée**.
4. **Filtering** : ne renvoie pas vers le port d'où vient la trame si la destination y est.

```text
A ─Fa0/1─┐
B ─Fa0/2─┤ Switch     A envoie à B :
C ─Fa0/3─┘            1. apprend A→Fa0/1
                      2. B inconnu → flood sur Fa0/2 et Fa0/3
                      3. B répond → apprend B→Fa0/2
                      4. ensuite : A↔B en direct, C ne voit plus rien
```
```text
show mac address-table          # Cisco
bridge fdb show                 # Linux
```

## 3. Collisions et domaines

| Notion | Définition |
|---|---|
| **Domaine de collision** | zone où deux émissions simultanées entrent en **collision**. Hub : tout est un seul domaine. **Switch : un domaine par port** (full-duplex → plus de collisions). |
| **Domaine de broadcast** | zone atteinte par un **broadcast**. **Switch : un seul** (sauf VLAN). **Routeur : sépare** les domaines de broadcast. |

Un **VLAN** = un domaine de broadcast distinct (voir [11 - VLAN, trunk, STP et EtherChannel](11%20-%20VLAN%2C%20trunk%2C%20STP%20et%20EtherChannel.md)).

## 4. ARP (Address Resolution Protocol)
Rôle : **trouver l'adresse MAC** correspondant à une **adresse IP** dans le LAN.

1. **ARP Request** (broadcast `FF:FF:FF:FF:FF:FF`) : « Qui a 192.168.1.20 ? Répondez à 192.168.1.10 ».
2. **ARP Reply** (unicast) : « 192.168.1.20 est à `AA:BB:CC:DD:EE:20` ».
3. Résultat gardé dans le **cache ARP**.

```bash
arp -a           # Windows/Linux : cache ARP
ip neigh         # Linux
arp -d *         # (Windows, admin) vider le cache
```
Ethertype ARP : `0x0806`. **IPv6** remplace ARP par **NDP** ([14 - IPv6](14%20-%20IPv6.md)).
**Sécurité** : l'ARP n'est pas authentifié → risque d'*ARP spoofing* ([[22 - Sécurité réseau]]).

## 📌 À retenir
- Le switch **apprend** les MAC sources et **commute** selon les MAC destination ; inconnue → **flood**.
- Switch : 1 domaine de collision par port, 1 domaine de broadcast ; routeur : coupe le broadcast.
- ARP : IP → MAC, en broadcast puis réponse unicast.

## 🧪 Exercices
1. Un switch reçoit une trame dont la MAC destination n'est pas dans sa table. Que fait-il ?
2. Combien de domaines de collision et de broadcast avec 1 switch de 8 ports (sans VLAN) ?
<details><summary>Réponses</summary>1. Il la diffuse sur tous les ports sauf celui d'entrée. 2. 8 collisions (1 par port), 1 broadcast.</details>

## 🔗 Liens
[05 - Ethernet (802.3) et adresse MAC](05%20-%20Ethernet%20%28802.3%29%20et%20adresse%20MAC.md) · [06 - Types de communication (Unicast, Multicast, Broadcast)](06%20-%20Types%20de%20communication%20%28Unicast%2C%20Multicast%2C%20Broadcast%29.md) · [00 - Index](00%20-%20Index.md)
