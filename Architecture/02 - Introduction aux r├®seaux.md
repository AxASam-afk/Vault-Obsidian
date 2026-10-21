---
titre: Introduction aux réseaux
formateur: Elimane NDIAYE
niveau: débutant
tags: [cours, réseau, fondamentaux]
---

# 02 — Introduction aux réseaux

Précédent : [01 - Vocabulaire de l'infrastructure (Cloud, DevOps, Virtualisation)](01%20-%20Vocabulaire%20de%20l%27infrastructure%20%28Cloud%2C%20DevOps%2C%20Virtualisation%29.md) · Suivant : [[03 - Types de réseaux (LAN, WLAN, WAN, MAN, Internet)]]

## 1. Qu'est-ce qu'un réseau ?

> [!abstract] Définition
> Un **réseau informatique** est un **ensemble d'équipements reliés entre eux** (par câble ou sans fil) **capables de communiquer** afin d'**échanger des données** et de **partager des ressources**.

Deux objectifs à retenir :
1. **échanger des données** (messages, fichiers, vidéo, voix) ;
2. **partager des ressources** (imprimantes, fichiers, applications, accès Internet).

## 2. Pourquoi créer un réseau ?

| Besoin | Sans réseau | Avec réseau |
|---|---|---|
| **Imprimante** | une imprimante par PC | une imprimante partagée par tous |
| **Fichiers** | copie par clé USB, versions multiples | serveur de fichiers central |
| **Applications** | installation sur chaque poste | application hébergée sur un serveur |
| **Internet** | une connexion par poste | une connexion partagée (routeur/box) |
| **Sauvegarde** | manuelle, poste par poste | centralisée et automatisée |
| **Administration** | intervention sur chaque machine | gestion centralisée (voir [Active Directory](25%20-%20Active%20Directory%20et%20LDAP.md)) |
| **Communication** | limitée | messagerie, visioconférence, VoIP |

## 3. Les équipements

### 3.1 Équipements terminaux (hôtes)
Ils **émettent ou reçoivent** les données, ce sont les extrémités de la communication.
- PC fixe, portable ;
- **téléphone**, tablette ;
- **serveur** ;
- imprimante réseau, scanner ;
- NAS (stockage réseau) ;
- caméra IP, téléphone IP ;
- télévision connectée, console de jeu ;
- objets connectés (IoT) : thermostat, montre, capteurs.

### 3.2 Équipements intermédiaires
Ils **relient** les terminaux et **acheminent** les données.

| Équipement | Rôle | Couche |
|---|---|---|
| **Switch** (commutateur) | relie les machines d'un même réseau local, transmet selon l'**adresse MAC** | 2 |
| **Routeur** | relie **des réseaux différents**, transmet selon l'**adresse IP** | 3 |
| **Point d'accès Wi-Fi** | connecte les appareils sans fil au réseau filaire | 1-2 |
| **Firewall** (pare-feu) | filtre le trafic selon des règles de sécurité | 3 à 7 |
| **Modem / box** | relie le réseau à l'opérateur (Internet) | 1 à 3 |
| *Hub* (obsolète) | répète le signal vers tous les ports | 1 |

### 3.3 Supports de transmission
- **Cuivre** : câble à paires torsadées RJ45 (Cat5e, Cat6), portée max **100 m** ;
- **Fibre optique** : longues distances, débits élevés ;
- **Radio** : Wi-Fi, 4G/5G, Bluetooth.

## 4. Exemple : un réseau à la maison (HOME)

```text
                 Internet
                    │
             ┌──────┴──────┐
             │  Box (routeur│  IP publique : 82.x.x.x  (côté Internet)
             │  + switch    │  IP privée   : 192.168.1.1 (côté maison)
             │  + Wi-Fi)    │
             └──┬───┬───┬───┘
        câble   │   │   │  Wi-Fi
   ┌────────────┘   │   └────────────┐
   │                │                │
 PC bureau       Imprimante       Téléphone
192.168.1.10    192.168.1.20     192.168.1.30
```

**Lien avec l'IP** : chaque appareil reçoit une **adresse IP privée** (souvent attribuée par [DHCP](17%20-%20DHCP.md)) dans le réseau local `192.168.1.0/24`. La box possède aussi une **IP publique** vers Internet, et le **NAT** traduit l'une vers l'autre.
Ta box réunit donc **plusieurs équipements** : modem, routeur, switch, point d'accès Wi-Fi, serveur DHCP.

Vérifier son IP :
```bash
# Windows
ipconfig
# Linux
ip a
```

## 5. Rôles : client, serveur, pair-à-pair
- **Client** : demande un service (ton navigateur).
- **Serveur** : fournit un service (un serveur web, DNS, fichiers).
- **Pair-à-pair (P2P)** : chaque machine est à la fois client et serveur (partage de fichiers entre deux PC).

## 📌 À retenir
- Réseau = équipements interconnectés qui **échangent des données** et **partagent des ressources**.
- **Switch** : dans un réseau ; **routeur** : entre réseaux.
- Une box domestique cumule plusieurs fonctions.

## 🧪 Exercices
1. Cite 5 équipements terminaux et 3 équipements intermédiaires.
2. Quelle différence entre un switch et un routeur ?
3. Sur ton réseau à la maison, quelle est l'IP de ta passerelle ?

## 🔗 Liens
[[03 - Types de réseaux (LAN, WLAN, WAN, MAN, Internet)]] · [04 - Topologies physiques et logiques](04%20-%20Topologies%20physiques%20et%20logiques.md) · [00 - Index](00%20-%20Index.md)
