---
titre: Architecture réseau d'entreprise et haute disponibilité
formateur: Elimane NDIAYE
niveau: 2e année
tags: [cours, architecture, redondance, haute-disponibilité, hsrp, vrrp]
---

# 31 — Architecture réseau d'entreprise et haute disponibilité

Précédent : [[30 - Diagnostic réseau]] · Suivant : [32 - Cloud, conteneurs et Infrastructure as Code](32%20-%20Cloud%2C%20conteneurs%20et%20Infrastructure%20as%20Code.md)

## 1. Modèle hiérarchique à 3 couches
```text
                 Internet / WAN
                     │
              [Firewall / Routeurs de bord]
                     │
              ┌──────┴──────┐
   CŒUR (Core)      ★ 2 switchs L3 redondants  : transport rapide, sans politique
              └──┬───────┬──┘
   DISTRIBUTION   ▲       ▲     : routage inter-VLAN, ACL, agrégation, redondance
              ┌──┴──┐ ┌──┴──┐
   ACCÈS      [SW]  [SW]  [SW]  : connexion des postes, VLAN, PoE, port security
              PC/Wi-Fi/IP tel.
```
- **Accès** : ports utilisateurs, VLAN, sécurité de port, PoE.
- **Distribution** : frontière L2/L3, routage inter-VLAN, politiques (ACL, QoS).
- **Cœur** : commutation ultra-rapide, haute disponibilité.
- **Cœur réduit (collapsed core)** : cœur + distribution fusionnés (PME/campus).
- **Spine-leaf** : datacenter, chaque *leaf* relié à tous les *spines* (latence prévisible).

## 2. Zones et segmentation
LAN utilisateurs, **serveurs**, **DMZ**, invités/Wi-Fi, gestion/administration, VoIP, IoT — chacune dans son **VLAN/sous-réseau** avec des **règles de firewall** entre zones ([[22 - Sécurité réseau]]).
Plan d'adressage : réserver des blocs par site/fonction (ex. `10.<site>.<vlan>.0/24`), **agréger** pour le routage ([13 - Subnetting, CIDR et VLSM](13%20-%20Subnetting%2C%20CIDR%20et%20VLSM.md)).

## 3. Redondance
Éliminer les **points uniques de défaillance (SPOF)** à tous les niveaux :
| Niveau | Mesures |
|---|---|
| Alimentation | double alimentation, onduleur (UPS), groupe électrogène |
| Liens | double lien, **EtherChannel/LACP**, double FAI |
| Switching | **RSTP**/STP, switchs redondants |
| Passerelle | **HSRP / VRRP / GLBP** |
| Routage | OSPF/BGP, chemins multiples (ECMP) |
| Serveurs | cluster, HA hyperviseur, load balancer |
| Données | RAID, réplication, sauvegardes |
| Site | site de secours (PRA/PCA) |

### Redondance de la passerelle : HSRP / VRRP
Deux routeurs partagent une **IP virtuelle** (ex. `192.168.10.1`) utilisée comme passerelle par les postes ; si le routeur **actif** tombe, le routeur **de secours** prend le relais sans reconfigurer les postes.
```text
R1 (actif, prio 110) ─┐
                      ├─ IP virtuelle 192.168.10.1 ── postes (passerelle unique)
R2 (standby, prio 100)┘
```
HSRP (Cisco), **VRRP** (standard ouvert), Linux : keepalived.

## 4. Haute disponibilité applicative
- **Répartiteur de charge** (load balancer : HAProxy, Nginx, F5) devant plusieurs serveurs identiques ; *health checks* ; **sessions persistantes** si nécessaire.
- **Clusters** actif/passif ou actif/actif ; stockage partagé/répliqué ; **quorum** ([[26 - Virtualisation avancée]]).
- **Réplication** de bases de données ; DNS et DHCP en double ([17 - DHCP](17%20-%20DHCP.md)).
- Mesure : disponibilité en « neuf » ([29 - Supervision et logs](29%20-%20Supervision%20et%20logs.md)).
- Distinguer **HA** (continuité automatique) et **sauvegarde/PRA** (reprise) ([28 - Sauvegarde et restauration](28%20-%20Sauvegarde%20et%20restauration.md)).

## 5. Bonnes pratiques d'architecture
Schéma **documenté** (L1/L2/L3), convention de nommage, gestion centralisée (AAA, logs, SNMP), **réseau de management séparé**, QoS pour la voix, capacité dimensionnée avec marge, standardisation des configurations (sauvegarde de configs), **tests de bascule** réguliers.

## 6. Exemple : PME de 200 postes sur 2 sites
```text
Siège : 2 switchs cœur/distribution L3 (VLAN 10 Users, 20 Wi-Fi, 30 Serveurs, 40 VoIP, 99 Mgmt)
        1 firewall HA (actif/passif) + 2 FAI, DMZ (web, reverse proxy)
Agence : 1 routeur + switch, VPN site-à-site IPsec vers le siège
Services : 2 DC AD (DNS/DHCP), cluster Proxmox 3 nœuds, NAS + sauvegarde hors site
```

## 📌 À retenir
- Modèle 3 couches : accès / distribution / cœur.
- Éliminer les SPOF ; HSRP/VRRP pour la passerelle ; LACP et STP pour les liens.
- HA ≠ sauvegarde.

## 🧪 Exercices
1. Où placer le routage inter-VLAN dans un modèle 3 couches ?
2. À quoi sert l'adresse IP virtuelle en VRRP ?
<details><summary>Réponses</summary>1. Couche distribution (switchs L3). 2. Servir de passerelle unique, qui bascule d'un routeur à l'autre sans reconfigurer les postes.</details>

## 🔗 Liens
[11 - VLAN, trunk, STP et EtherChannel](11%20-%20VLAN%2C%20trunk%2C%20STP%20et%20EtherChannel.md) · [19 - Routage](19%20-%20Routage.md) · [[26 - Virtualisation avancée]] · [00 - Index](00%20-%20Index.md)
