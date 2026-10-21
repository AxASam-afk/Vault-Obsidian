---
titre: Protocoles réseau essentiels
formateur: Elimane NDIAYE
niveau: 1re-2e année
tags: [cours, protocoles, http, ssh, smtp, snmp, smb, rdp]
---

# 18 — Protocoles réseau essentiels

Précédent : [17 - DHCP](17%20-%20DHCP.md) · Suivant : [19 - Routage](19%20-%20Routage.md)

Pour DNS et DHCP, voir [16 - DNS](16%20-%20DNS.md) et [17 - DHCP](17%20-%20DHCP.md).

## Fiches rapides

### HTTP / HTTPS
- **Rôle** : échanger des pages et des API web. **Couche 7**, TCP **80** / **443** (HTTP/3 : QUIC sur UDP 443).
- **Fonctionnement** : requête (`GET`, `POST`, `PUT`, `DELETE`) → réponse avec **code** : 2xx OK, 3xx redirection, 4xx erreur client (404, 403), 5xx erreur serveur (500, 502, 503).
- **HTTPS** = HTTP dans **TLS** : chiffrement, intégrité, authentification du serveur par **certificat**.
- **Exemple** : `curl -I https://exemple.fr`.
- **Problèmes** : certificat expiré/invalide, mauvais nom, redirections en boucle.
- **Sécurité** : toujours HTTPS, HSTS, en-têtes de sécurité.

### SSH
- **Rôle** : administration distante **chiffrée** (remplace Telnet). TCP **22**.
- **Fonctionnement** : échange de clés, authentification par **mot de passe** ou **clé publique**.
- **Exemple** : `ssh admin@192.168.1.5` ; `ssh -i ~/.ssh/id_ed25519 user@srv` ; `ssh-keygen -t ed25519`.
- **Sécurité** : désactiver le login root et les mots de passe, clés uniquement, `fail2ban`, changer/limiter l'accès.

### FTP / SFTP
- **FTP** : transfert de fichiers, TCP **21** (contrôle) + **20** (données ; mode passif = ports dynamiques). **Non chiffré** → à éviter.
- **SFTP** : transfert **via SSH** (port 22). FTPS = FTP + TLS.
- **Exemple** : `sftp user@srv`, `scp fichier user@srv:/tmp/`.

### SMTP / POP3 / IMAP (messagerie)
| Protocole | Rôle | Ports |
|---|---|---|
| **SMTP** | **envoi** / relais de courriels | 25 (entre serveurs), 587 (soumission), 465 |
| **POP3** | **récupération** (télécharge et supprime) | 110 / 995 (TLS) |
| **IMAP** | récupération **synchronisée** (boîte sur le serveur) | 143 / 993 (TLS) |
- DNS associé : **MX**, SPF/DKIM/DMARC (TXT).
- **Problèmes** : port 25 bloqué, relais refusé, spam/blacklist.

### NTP
- **Rôle** : synchroniser l'**heure** (crucial pour logs, Kerberos, certificats). UDP **123**.
- Hiérarchie de **strates**. **Exemple** : `chronyc sources`, `timedatectl`, `w32tm /query /status`.

### SNMP
- **Rôle** : **superviser/administrer** les équipements (switchs, routeurs, serveurs). UDP **161** (requêtes), **162** (traps).
- Concepts : **manager**, **agent**, **MIB/OID**, *community* (v1/v2c) ; **SNMPv3** = authentification + chiffrement.
- **Exemple** : `snmpwalk -v2c -c public 192.168.1.1`.
- **Sécurité** : changer la communauté `public`, préférer **v3**, limiter par ACL.

### ICMP
- **Rôle** : messages de **contrôle et d'erreur** de la couche 3 (protocole IP n° 1, **pas de port**).
- **Types** : Echo Request/Reply (ping, 8/0), Destination Unreachable (3), Time Exceeded (11, utilisé par traceroute).
- **Exemples** : `ping`, `traceroute`/`tracert`.
- **Sécurité** : ne pas tout bloquer (PMTU discovery) ; limiter le débit.

### LDAP
- **Rôle** : interroger/modifier un **annuaire** (utilisateurs, groupes). TCP **389**, **636** (LDAPS), 3268 (catalogue global AD). Voir [25 - Active Directory et LDAP](25%20-%20Active%20Directory%20et%20LDAP.md).

### SMB
- **Rôle** : **partage de fichiers et imprimantes** Windows. TCP **445**.
- **Exemple** : `\\srv\partage` ; `net use Z: \\srv\partage` ; Linux : `smbclient`, Samba.
- **Sécurité** : désactiver **SMBv1**, ne jamais exposer le 445 sur Internet, signer/chiffrer.

### RDP
- **Rôle** : **bureau à distance** Windows. TCP (et UDP) **3389**.
- **Sécurité** : jamais exposé directement sur Internet ; VPN ou passerelle RDG, **NLA**, MFA, verrouillage des comptes.

## Tableau récapitulatif
| Protocole | Couche | Transport | Port(s) | Chiffré ? |
|---|:-:|---|---|:-:|
| DNS | 7 | UDP/TCP | 53 | non (DoT/DoH oui) |
| DHCP | 7 | UDP | 67/68 | non |
| HTTP | 7 | TCP | 80 | non |
| HTTPS | 7 | TCP | 443 | oui |
| SSH/SFTP | 7 | TCP | 22 | oui |
| FTP | 7 | TCP | 21/20 | non |
| SMTP | 7 | TCP | 25/587/465 | STARTTLS/TLS |
| POP3/IMAP | 7 | TCP | 110-995/143-993 | avec TLS |
| NTP | 7 | UDP | 123 | non |
| SNMP | 7 | UDP | 161/162 | v3 oui |
| ICMP | 3 | IP 1 | — | non |
| LDAP/LDAPS | 7 | TCP | 389/636 | LDAPS oui |
| SMB | 7 | TCP | 445 | v3 possible |
| RDP | 7 | TCP | 3389 | oui |

## 📌 À retenir
- Préférer les versions **chiffrées** : HTTPS, SSH/SFTP, IMAPS, LDAPS, SNMPv3.
- Chaque protocole = un **port** à connaître pour le **diagnostic** et le **pare-feu**.

## 🧪 Exercice
Associe : 22, 25, 53, 123, 161, 445, 3389 → SSH, SMTP, DNS, NTP, SNMP, SMB, RDP.

## 🔗 Liens
[15 - TCP, UDP et ports](15%20-%20TCP%2C%20UDP%20et%20ports.md) · [[22 - Sécurité réseau]] · [29 - Supervision et logs](29%20-%20Supervision%20et%20logs.md) · [00 - Index](00%20-%20Index.md)
