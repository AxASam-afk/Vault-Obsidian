---
titre: DHCP
formateur: Elimane NDIAYE
niveau: 1re-2e année
tags: [cours, dhcp, dora]
---

# 17 — DHCP (Dynamic Host Configuration Protocol)

Précédent : [16 - DNS](16%20-%20DNS.md) · Suivant : [18 - Protocoles essentiels](18%20-%20Protocoles%20essentiels.md)

## 1. Pourquoi DHCP ?
Configurer manuellement l'IP, le masque, la passerelle et le DNS de chaque machine est long et source d'erreurs (doublons d'IP). Le **serveur DHCP** distribue **automatiquement** ces paramètres. Port **UDP 67** (serveur) / **68** (client).

## 2. Processus DORA
```text
Client (0.0.0.0)                          Serveur DHCP
   │── DHCP DISCOVER (broadcast) ──────────►│  « Y a-t-il un serveur ? »
   │◄─ DHCP OFFER (unicast/broadcast) ──────│  « Je te propose 192.168.1.50 »
   │── DHCP REQUEST (broadcast) ───────────►│  « J'accepte 192.168.1.50 »
   │◄─ DHCP ACK ────────────────────────────│  « Confirmé, bail de 8 h »
```
| Étape | Détail |
|---|---|
| **Discover** | broadcast `255.255.255.255`, source `0.0.0.0` |
| **Offer** | IP proposée + masque, passerelle, DNS, durée |
| **Request** | choix de l'offre (broadcast, informe les autres serveurs) |
| **ACK** | validation ; le client configure son interface |

## 3. Bail (lease)
L'IP est **prêtée** pour une durée. Le client tente de **renouveler à 50 %** (T1) puis **87,5 %** (T2) du bail. À expiration : IP libérée.

## 4. Étendue (scope), exclusions, réservations
- **Scope/pool** : plage distribuée (ex. `192.168.1.100–200`).
- **Exclusions** : adresses non distribuées (serveurs, imprimantes en IP fixe).
- **Réservation** : associe une **MAC** à une **IP fixe** distribuée par DHCP (idéal pour imprimantes/serveurs).

## 5. Options DHCP courantes
| Option | Contenu |
|:-:|---|
| 1 | masque de sous-réseau |
| 3 | routeur (passerelle) |
| 6 | serveurs DNS |
| 15 | nom de domaine |
| 42 | serveurs NTP |
| 51 | durée du bail |
| 66/67 | serveur TFTP / fichier de boot (PXE) |
| 121 | routes statiques |

## 6. DHCP Relay
Le broadcast ne franchit pas un routeur. Un **relais DHCP** (*ip helper-address* sur Cisco, agent de relais) reçoit le Discover d'un VLAN/sous-réseau et le **transmet en unicast** à un serveur DHCP central.
```text
interface vlan 20
 ip helper-address 10.0.0.5
```

## 7. Diagnostic
```bash
ipconfig /release && ipconfig /renew        # Windows
ipconfig /all                               # serveur DHCP, bail
sudo dhclient -r && sudo dhclient           # Linux (selon distribution)
journalctl -u systemd-networkd | grep -i dhcp
sudo tcpdump -i eth0 port 67 or port 68
```
| Symptôme | Cause probable |
|---|---|
| IP **169.254.x.x** | aucun serveur DHCP joignable |
| Mauvaise plage/passerelle | **serveur DHCP pirate** (rogue) |
| « Plus d'adresses » | pool épuisé, baux trop longs |
| Marche sur un VLAN pas sur l'autre | relay absent |

## 8. Sécurité
**Rogue DHCP** et **DHCP starvation** ; défense : **DHCP snooping** sur les switchs (ports de confiance), limitation d'adresses par port.

## 📌 À retenir
- DORA = Discover, Offer, Request, ACK ; UDP 67/68.
- Le bail se renouvelle à 50 %.
- Relay pour franchir un routeur ; réservation = IP fixe via DHCP.

## 🧪 Exercices
1. Pourquoi le Discover est-il en broadcast ?
2. À quoi sert une réservation DHCP ?
3. Un PC reçoit 169.254.10.5 : que vérifier ?
<details><summary>Réponses</summary>1. Le client n'a pas d'IP et ignore où est le serveur. 2. Toujours la même IP pour une MAC, sans configuration manuelle. 3. Serveur DHCP, câble/VLAN, relay, pool.</details>

## 🔗 Liens
[12 - Adressage IPv4](12%20-%20Adressage%20IPv4.md) · [14 - IPv6](14%20-%20IPv6.md) · [24 - Windows Server](24%20-%20Windows%20Server.md) · [00 - Index](00%20-%20Index.md)
