---
titre: Types de communication
formateur: Elimane NDIAYE
niveau: 1re année
tags: [cours, unicast, multicast, broadcast, anycast]
---

# 06 — Types de communication : Unicast, Multicast, Broadcast

Précédent : [05 - Ethernet (802.3) et adresse MAC](05%20-%20Ethernet%20%28802.3%29%20et%20adresse%20MAC.md) · Suivant : [[07 - Protocoles, débit, latence et encapsulation]]

Selon le **nombre de destinataires**, une communication réseau est de type **unicast**, **multicast**, **broadcast** (ou **anycast**).

## 1. Unicast — un vers un
> Un émetteur → **un seul** destinataire précis.

```text
   A ───────────► B          (C et D ne reçoivent rien)
   C      D
```
- **MAC dest** : l'adresse MAC de B.
- **IP dest** : l'adresse IP de B.
- **Exemples** : navigation web, SSH, envoi d'un email, appel VoIP entre deux personnes.

## 2. Broadcast — un vers tous
> Un émetteur → **tous** les équipements du réseau local.

```text
   A ───────────► B, C, D    (toute la « salle » entend)
```
- **MAC dest** : `FF:FF:FF:FF:FF:FF`.
- **IPv4 dest** : `255.255.255.255` (broadcast limité) ou l'adresse de broadcast du sous-réseau (ex. `192.168.1.255`).
- **Exemples** : requête [ARP](10%20-%20Switch%2C%20table%20MAC%20et%20ARP.md) (« qui a l'IP 192.168.1.20 ? »), `DHCP Discover`.
- Les **routeurs ne transmettent pas** le broadcast : il reste dans son **domaine de broadcast** (un LAN/VLAN).
- **IPv6 supprime le broadcast** et le remplace par le multicast.

> [!warning] Trop de broadcast = réseau lent
> Un grand réseau plat génère beaucoup de broadcasts. On le découpe en **VLAN** ou en sous-réseaux.

## 3. Multicast — un vers un groupe
> Un émetteur → **un groupe** de destinataires intéressés.

```text
   A ───────────► B, D       (seuls les membres du groupe reçoivent ; C ignore)
```
- **IPv4** : plage `224.0.0.0` à `239.255.255.255` (`224.0.0.0/4`).
- **MAC dest** : dérivée de l'IP, préfixe `01:00:5E` (IPv4) ou `33:33` (IPv6).
- **Exemples** :
  - `224.0.0.5` / `224.0.0.6` : OSPF ;
  - `224.0.0.251` : mDNS ;
  - IPTV, diffusion vidéo, mises à jour groupées.
- **Avantage** : un seul flux est envoyé, quel que soit le nombre de destinataires (économie de bande passante).
- Les machines rejoignent un groupe via **IGMP** (IPv4) ou **MLD** (IPv6).

## 4. Anycast — un vers le plus proche
> Une même adresse est portée par **plusieurs serveurs** ; la communication va vers **le plus proche** selon le routage.
- **Exemple** : les serveurs DNS racine et les résolveurs publics (ex. `8.8.8.8`).

## 5. Tableau de synthèse

| Type | Destinataires | MAC destination | IP destination (exemple) | Exemple d'usage |
|---|---|---|---|---|
| Unicast | 1 | MAC de l'hôte | `192.168.1.20` | Web, SSH |
| Broadcast | tous du LAN | `FF:FF:FF:FF:FF:FF` | `192.168.1.255` | ARP, DHCP Discover |
| Multicast | groupe | `01:00:5E:xx:xx:xx` | `224.0.0.5` | OSPF, IPTV |
| Anycast | le plus proche | — | même IP sur plusieurs sites | DNS, CDN |

## 6. Observer avec Wireshark
```text
eth.dst == ff:ff:ff:ff:ff:ff     # trames de broadcast
arp                              # requêtes/réponses ARP
eth.dst[0] & 1                   # trames multicast ou broadcast
```
Sous Linux : `sudo tcpdump -i eth0 -e broadcast`

## 📌 À retenir
- Unicast = 1→1 ; broadcast = 1→tous ; multicast = 1→groupe ; anycast = 1→le plus proche.
- Le broadcast s'arrête au **routeur**.
- IPv6 n'a **pas** de broadcast.

## 🧪 Exercices
1. Quel type de communication utilise une requête ARP ? Quelle MAC destination ?
2. Pourquoi le multicast est-il préférable au unicast pour diffuser une vidéo à 500 postes ?
3. `224.0.0.5` est-elle une adresse unicast, broadcast ou multicast ?

<details><summary>Réponses</summary>

1. Broadcast, `FF:FF:FF:FF:FF:FF`.
2. Un seul flux circule sur le réseau au lieu de 500 copies.
3. Multicast (OSPF).
</details>

## 🔗 Liens
[05 - Ethernet (802.3) et adresse MAC](05%20-%20Ethernet%20%28802.3%29%20et%20adresse%20MAC.md) · [[03 - Types de réseaux (LAN, WLAN, WAN, MAN, Internet)]] · [Domaine de broadcast](10%20-%20Switch%2C%20table%20MAC%20et%20ARP.md) · [00 - Index](00%20-%20Index.md)
