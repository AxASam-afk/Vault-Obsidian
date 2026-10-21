---
titre: Routage
formateur: Elimane NDIAYE
niveau: 1re-2e année
tags: [cours, routage, ospf, bgp, inter-vlan]
---

# 19 — Routage

Précédent : [18 - Protocoles essentiels](18%20-%20Protocoles%20essentiels.md) · Suivant : [20 - NAT et PAT](20%20-%20NAT%20et%20PAT.md)

## 1. Pourquoi router ?
Un switch relie des machines d'**un même réseau**. Pour communiquer **entre réseaux différents** (sous-réseaux, VLAN, Internet), il faut un **routeur** (couche 3) qui transmet les paquets en fonction de l'**adresse IP destination**.

```text
LAN 192.168.1.0/24 ── [R1] ──── 10.0.0.0/30 ──── [R2] ── LAN 192.168.2.0/24
```

## 2. Table de routage
Liste de règles « pour aller vers **tel réseau**, passer par **tel next-hop / interface** ».
```text
Codes: C connectée, S statique, O OSPF, B BGP, * route par défaut
C  192.168.1.0/24 is directly connected, G0/0
S  192.168.2.0/24 [1/0] via 10.0.0.2
S* 0.0.0.0/0 [1/0] via 203.0.113.1
```
Consultation : `show ip route` (Cisco), `ip route` (Linux), `route print` (Windows).

- **Route connectée** : réseau directement rattaché à une interface (ajoutée automatiquement si l'interface est *up*).
- **Route statique** : ajoutée à la main.
- **Route par défaut** : `0.0.0.0/0`, « pour tout ce que je ne connais pas ».
- **Dynamique** : apprise via un protocole de routage.

## 3. Comment choisit-on une route ?
1. **Longest Prefix Match** : la route au **préfixe le plus long** (la plus précise) gagne.
2. Si égalité : **distance administrative** la plus basse (fiabilité de la source).
3. Si égalité : **métrique** la plus faible (coût du chemin).

**Exemple** : destination `10.1.1.5` ; routes `10.0.0.0/8`, `10.1.0.0/16`, `10.1.1.0/24` → gagne **/24**.

| Source | Distance administrative (Cisco) |
|---|:-:|
| Connectée | 0 |
| Statique | 1 |
| eBGP | 20 |
| OSPF | 110 |
| RIP | 120 |
| iBGP | 200 |

## 4. Routage statique
```text
R1(config)# ip route 192.168.2.0 255.255.255.0 10.0.0.2
R1(config)# ip route 0.0.0.0 0.0.0.0 203.0.113.1        # par défaut
```
Linux : `sudo ip route add 192.168.2.0/24 via 10.0.0.2` · Windows : `route add 192.168.2.0 mask 255.255.255.0 10.0.0.2`
+ Simple, prévisible, léger. − Pas d'adaptation aux pannes, lourd à grande échelle.
Chaque routeur doit avoir la **route aller ET retour**.

## 5. Routage dynamique
Les routeurs **échangent leurs routes** automatiquement et s'adaptent aux pannes.
| Catégorie | Protocoles |
|---|---|
| **IGP** (à l'intérieur d'un AS) | RIP, OSPF, EIGRP, IS-IS |
| **EGP** (entre AS) | **BGP** |
| Types | vecteur de distance (RIP) / **état de liens** (OSPF) / chemin (BGP) |

### RIP
Vecteur de distance, métrique = **nombre de sauts** (max 15). Simple mais lent et limité ; peu utilisé aujourd'hui.

### OSPF (Open Shortest Path First)
- Protocole **à état de liens**, standard ouvert, **algorithme de Dijkstra (SPF)**.
- Métrique = **coût** = bande passante de référence (100 Mbit/s par défaut) / bande passante du lien.
- Voisins via paquets **Hello** (multicast `224.0.0.5`), **aires** (area 0 = backbone), **DR/BDR** sur les réseaux multi-accès.
- Convergence rapide, hiérarchique.
```text
R1(config)# router ospf 1
R1(config-router)# router-id 1.1.1.1
R1(config-router)# network 192.168.1.0 0.0.0.255 area 0
R1(config-router)# network 10.0.0.0 0.0.0.3 area 0
show ip ospf neighbor / show ip route ospf
```
États de voisinage : Down → Init → 2-Way → ExStart → Exchange → Loading → **Full**.

### BGP (notions)
Protocole du **routage inter-domaines**, qui **fait fonctionner Internet** : échange de routes entre **systèmes autonomes (AS)** identifiés par un numéro (ASN). Vecteur de chemin, TCP **179**, décisions par **politiques** (attributs AS-PATH, local-pref…). **eBGP** entre AS, **iBGP** au sein d'un AS. Une entreprise l'utilise pour un **multi-homing** (2 FAI).

## 6. Routage inter-VLAN
Les VLAN sont isolés en L2 ; un **équipement L3** est nécessaire.

### Router-on-a-stick
Un seul lien **trunk** vers le routeur, avec des **sous-interfaces** 802.1Q (une par VLAN).
```text
R1(config)# interface g0/0.10
R1(config-subif)# encapsulation dot1Q 10
R1(config-subif)# ip address 192.168.10.1 255.255.255.0
R1(config)# interface g0/0.20
R1(config-subif)# encapsulation dot1Q 20
R1(config-subif)# ip address 192.168.20.1 255.255.255.0
```
La passerelle du VLAN 10 = `192.168.10.1`.

### Switch de niveau 3 (SVI)
Le switch L3 possède une **interface virtuelle par VLAN** (`interface vlan 10` + IP) et route à vitesse matérielle (`ip routing`). Solution standard en entreprise.

## 📌 À retenir
- Le routeur choisit par **préfixe le plus long**, puis **AD**, puis **métrique**.
- Statique = simple ; OSPF = dynamique interne ; BGP = Internet.
- Inter-VLAN : router-on-a-stick ou switch L3 (SVI).

## 🧪 Exercices
1. Destination 192.168.1.20 : routes `192.168.0.0/16` (OSPF) et `192.168.1.0/24` (statique). Laquelle est choisie ?
2. Pourquoi faut-il une route de retour ?
3. Coût OSPF d'un lien 100 Mbit/s (réf. 100) ?
<details><summary>Réponses</summary>1. /24 (préfixe le plus long). 2. Sinon la réponse ne revient pas. 3. 1.</details>

## 🔗 Liens
[11 - VLAN, trunk, STP et EtherChannel](11%20-%20VLAN%2C%20trunk%2C%20STP%20et%20EtherChannel.md) · [13 - Subnetting, CIDR et VLSM](13%20-%20Subnetting%2C%20CIDR%20et%20VLSM.md) · [20 - NAT et PAT](20%20-%20NAT%20et%20PAT.md) · [[31 - Architecture d'entreprise et haute disponibilité]] · [00 - Index](00%20-%20Index.md)
