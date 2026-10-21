---
titre: TCP, UDP et ports
formateur: Elimane NDIAYE
niveau: 1re année
tags: [cours, tcp, udp, ports, transport]
---

# 15 — Couche transport : TCP, UDP, ports

Précédent : [14 - IPv6](14%20-%20IPv6.md) · Suivant : [16 - DNS](16%20-%20DNS.md)

## 1. Rôle du transport
La couche 4 assure la **communication entre applications** (de processus à processus) grâce aux **ports** et fournit selon le cas fiabilité, ordre, contrôle de flux.

## 2. Ports et sockets
- **Port** : nombre de **16 bits** (0–65535) qui identifie **un service/une application** sur une machine.
- **Socket** = **IP + port** (ex. `192.168.1.10:51234`). Une connexion = 5-uplet (protocole, IP src, port src, IP dst, port dst).

| Plage | Nom | Usage |
|---|---|---|
| 0–1023 | **Ports connus** (well-known) | services standards (80, 443, 22…) |
| 1024–49151 | Ports **enregistrés** | applications (3306 MySQL, 3389 RDP) |
| 49152–65535 | Ports **dynamiques/éphémères** | port source choisi par le client |

Exemple : navigateur `192.168.1.10:51234` → serveur web `93.184.216.34:443`.

## 3. TCP (Transmission Control Protocol)
**Orienté connexion, fiable, ordonné.**
- **Three-way handshake** (ouverture) :
```text
Client                  Serveur
  │── SYN (seq=x) ─────────►│
  │◄─ SYN+ACK (seq=y,ack=x+1)│
  │── ACK (ack=y+1) ────────►│   connexion établie
```
- **Numéros de séquence** : ordonnent les segments.
- **ACK** : accusé de réception ; sans ACK → **retransmission**.
- **Contrôle de flux** : fenêtre (*window size*) annoncée par le récepteur.
- **Contrôle de congestion** : slow start, évitement de congestion (adapte le débit au réseau).
- **Fermeture** : `FIN`/`ACK` (4 temps) ; `RST` = coupure brutale.
- Drapeaux (flags) : SYN, ACK, FIN, RST, PSH, URG.

## 4. UDP (User Datagram Protocol)
**Sans connexion, non fiable, léger** : pas de handshake, pas d'ACK, pas d'ordre garanti. En-tête de **8 octets**.
Usages : **DNS**, **DHCP**, VoIP, streaming, jeux, NTP, SNMP, QUIC/HTTP3.

## 5. Comparaison
| | **TCP** | **UDP** |
|---|---|---|
| Connexion | oui (handshake) | non |
| Fiabilité | oui (ACK, retransmission) | non |
| Ordre | garanti | non garanti |
| Contrôle flux/congestion | oui | non |
| En-tête | 20 octets min | 8 octets |
| Vitesse | plus lent | plus rapide |
| Exemples | HTTP(S), SSH, SMTP, FTP | DNS, DHCP, VoIP, NTP |

## 6. Ports usuels (à connaître)
| Service | Port | Transport |
|---|---|---|
| FTP | 20 (données), 21 (contrôle) | TCP |
| SSH / SFTP | 22 | TCP |
| Telnet | 23 | TCP |
| SMTP | 25 (587 soumission, 465 SMTPS) | TCP |
| DNS | 53 | UDP (et TCP) |
| DHCP | 67 (serveur), 68 (client) | UDP |
| HTTP | 80 | TCP |
| Kerberos | 88 | TCP/UDP |
| NTP | 123 | UDP |
| POP3 / IMAP | 110 / 143 | TCP |
| SNMP | 161 (requêtes), 162 (traps) | UDP |
| LDAP / LDAPS | 389 / 636 | TCP |
| HTTPS | 443 | TCP (et UDP pour HTTP/3) |
| SMB | 445 | TCP |
| Syslog | 514 | UDP |
| RDP | 3389 | TCP |

## 7. Commandes
```bash
ss -tuln          # Linux : ports en écoute (t=TCP u=UDP l=listen n=numérique)
netstat -ano      # Windows : connexions + PID
nc -zv 192.168.1.5 443   # tester un port
```

## 📌 À retenir
- TCP = fiable (handshake SYN/SYN-ACK/ACK) ; UDP = rapide, sans garantie.
- Port = 16 bits ; socket = IP:port.
- DNS/DHCP/NTP/SNMP en UDP.

## 🧪 Exercices
1. Pourquoi la VoIP préfère-t-elle UDP ?
2. Quels sont les 3 messages du handshake TCP ?
3. Quel port pour HTTPS ? RDP ? SSH ?
<details><summary>Réponses</summary>1. Un paquet en retard est inutile, mieux vaut l'ignorer que retransmettre. 2. SYN, SYN-ACK, ACK. 3. 443, 3389, 22.</details>

## 🔗 Liens
[[08 - Modèle OSI]] · [18 - Protocoles essentiels](18%20-%20Protocoles%20essentiels.md) · [[22 - Sécurité réseau]] · [00 - Index](00%20-%20Index.md)
