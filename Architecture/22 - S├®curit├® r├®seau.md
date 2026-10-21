---
titre: Sécurité réseau
formateur: Elimane NDIAYE
niveau: 2e année
tags: [cours, sécurité, firewall, vpn, tls, pki]
---

# 22 — Sécurité réseau

Précédent : [21 - Wi-Fi](21%20-%20Wi-Fi.md) · Suivant : [23 - Linux pour l'infrastructure](23%20-%20Linux%20pour%20l%27infrastructure.md)

> [!warning] Approche défensive
> Les attaques sont présentées pour comprendre **leur principe, leur détection et leur prévention**.

## 1. Principes fondamentaux : la triade CIA
| | Signification | Exemple de mesure |
|---|---|---|
| **C**onfidentialité | seuls les autorisés accèdent aux données | chiffrement, contrôle d'accès |
| **I**ntégrité | données non altérées | hachage, signatures |
| **D**isponibilité (Availability) | service accessible | redondance, sauvegarde, anti-DDoS |

Autres principes : **moindre privilège**, **défense en profondeur**, **segmentation**, **Zero Trust** (« ne jamais faire confiance, toujours vérifier »), surface d'attaque minimale.

## 2. AAA : authentification, autorisation, traçabilité
- **Authentification** : prouver son identité (mot de passe, clé, biométrie, **MFA** = plusieurs facteurs : ce que je sais / possède / suis).
- **Autorisation** : ce que j'ai le droit de faire (droits, groupes, ACL).
- **Accounting** : journalisation des actions.
- Protocoles : RADIUS, TACACS+, Kerberos, LDAP.

## 3. Comptes et privilèges
Comptes **nominatifs**, comptes **admin séparés** des comptes courants, pas de comptes partagés, **moindre privilège**, revue régulière des droits, rotation des secrets, gestionnaire de mots de passe, désactivation des comptes inutiles.

## 4. Pare-feu (firewall)
Filtre le trafic selon des règles (IP, ports, protocole, application).
| Type | Fonctionnement |
|---|---|
| **Sans état** (stateless) | examine chaque paquet isolément (ACL) |
| **À états** (stateful) | suit les **connexions** : autorise automatiquement le retour du trafic légitime |
| **NGFW** | + inspection applicative, IPS, filtrage web, identité |
| **WAF** | protège les applications web |
Principe : **deny all** par défaut, puis autoriser le nécessaire. Règles lues **de haut en bas**, la première correspondance s'applique.

## 5. ACL (Access Control Lists)
Liste ordonnée de règles `permit`/`deny` sur un routeur/switch L3, avec un `deny any` **implicite** à la fin.
```text
R1(config)# ip access-list extended WEB-ONLY
R1(config-ext-nacl)# permit tcp 192.168.10.0 0.0.0.255 any eq 80
R1(config-ext-nacl)# permit tcp 192.168.10.0 0.0.0.255 any eq 443
R1(config-ext-nacl)# deny ip any any log
R1(config)# interface g0/0
R1(config-if)# ip access-group WEB-ONLY in
```
Standard (source seule) vs étendue (source, destination, protocole, ports). Placer les ACL **étendues près de la source**, les **standard près de la destination**.

## 6. DMZ et segmentation
**DMZ** : zone intermédiaire pour les services **exposés** (web, mail, reverse proxy), isolée du LAN interne.
```text
Internet ──[Firewall]──┬── DMZ (serveur web)
                       ├── LAN utilisateurs (VLAN 10)
                       └── Serveurs internes (VLAN 30)
```
Règles types : Internet → DMZ (443) ✔ ; Internet → LAN ✘ ; DMZ → LAN ✘ (ou très restreint) ; LAN → DMZ ✔.
**Segmentation** (VLAN, sous-réseaux, micro-segmentation) : limite la **propagation** d'une compromission.

## 7. VPN
Tunnel **chiffré** à travers un réseau non fiable.
| Type | Usage |
|---|---|
| **Site-à-site** | relie deux sites (WAN sur Internet) |
| **Accès distant** | un utilisateur nomade se connecte au réseau de l'entreprise |

- **IPsec** : suite de protocoles au niveau réseau. **IKE** (UDP 500, 4500 avec NAT-T) négocie ; **ESP** (protocole 50) chiffre/authentifie ; modes **tunnel** / transport.
- **TLS VPN** (OpenVPN, SSL-VPN), **WireGuard** (moderne, léger, UDP).

## 8. TLS, certificats et PKI
**TLS** (successeur de SSL) : chiffre, authentifie et garantit l'intégrité (HTTPS, LDAPS, SMTPS…).
Poignée de main simplifiée (TLS 1.3) : le client propose ses paramètres → le serveur présente son **certificat** → échange de clés (éphémère) → clé de session **symétrique** pour chiffrer.
- **Certificat X.509** : lie une **clé publique** à une identité (nom de domaine), **signé par une autorité de certification (CA)**.
- **PKI** : ensemble CA racine → CA intermédiaire → certificats finaux ; **chaîne de confiance**, validité, **révocation** (CRL/OCSP).
- Chiffrement **symétrique** (AES) rapide vs **asymétrique** (RSA, ECC) pour l'échange de clés/signatures ; **hachage** (SHA-256) pour l'intégrité.
```bash
openssl s_client -connect exemple.fr:443 -servername exemple.fr
curl -vI https://exemple.fr
```
Erreurs fréquentes : certificat **expiré**, nom non correspondant, CA inconnue, chaîne incomplète, horloge incorrecte.

## 9. Attaques courantes (compréhension, détection, prévention)
| Attaque | Principe général | Détection | Prévention |
|---|---|---|---|
| **ARP spoofing** | fausses réponses ARP pour se faire passer pour la passerelle | changements de MAC dans le cache ARP, alertes IDS | **DAI** (Dynamic ARP Inspection), DHCP snooping, port security, segmentation |
| **DNS spoofing / empoisonnement** | fausses réponses DNS redirigeant vers un faux serveur | réponses incohérentes, logs DNS | **DNSSEC**, résolveurs de confiance, DoT/DoH |
| **DoS / DDoS** | saturer un service (ressources, bande passante) via un ou **plusieurs** sources | pics anormaux de trafic/connexions | limitation de débit, **anti-DDoS/CDN**, SYN cookies, redondance |
| **MITM** (homme du milieu) | s'interposer entre deux parties pour lire/modifier | avertissements de certificat, ARP/DNS anormaux | **TLS** partout, HSTS, VPN, 802.1X |
| **Brute force** | essais massifs de mots de passe | échecs de connexion répétés dans les logs | **MFA**, verrouillage/temporisation, `fail2ban`, mots de passe forts, clés SSH |
| **Phishing / ingénierie sociale** | tromper l'humain | signalements, analyse de messages | formation, filtrage mail, MFA |
| **Ransomware** | chiffre les données contre rançon | activité disque anormale | sauvegardes **immuables**/hors ligne, EDR, segmentation, correctifs |
| **Rogue DHCP / VLAN hopping** | faux serveur DHCP / sortie du VLAN | alertes snooping | DHCP snooping, trunks désactivés par défaut, VLAN natif dédié |

## 10. Défense et bonnes pratiques
1. **Mises à jour** régulières (OS, firmwares).
2. **Moindre privilège** + **MFA**.
3. **Segmentation** + firewall **deny-by-default**.
4. Désactiver les services et ports inutiles ; protocoles **chiffrés** uniquement.
5. **Journalisation centralisée** et supervision ([29 - Supervision et logs](29%20-%20Supervision%20et%20logs.md)).
6. **Sauvegardes** testées ([28 - Sauvegarde et restauration](28%20-%20Sauvegarde%20et%20restauration.md)).
7. Sécurité des switchs : **port security**, BPDU guard, DHCP snooping, DAI, désactiver les ports inutilisés.
8. Durcissement SSH/RDP, pas d'exposition directe sur Internet.
9. Sensibilisation des utilisateurs, plan de réponse aux incidents.

## 📌 À retenir
- CIA ; AAA ; moindre privilège ; défense en profondeur.
- Firewall stateful, ACL, DMZ, segmentation, VPN, TLS/PKI.
- Chaque attaque a sa **contre-mesure** ciblée.

## 🧪 Exercices
1. Que signifie l'`deny any` implicite d'une ACL ?
2. Quelle protection contre l'ARP spoofing sur un switch ?
3. Pourquoi placer les serveurs web en DMZ ?
<details><summary>Réponses</summary>1. Tout ce qui n'est pas explicitement autorisé est refusé. 2. DAI (avec DHCP snooping), port security. 3. Pour limiter l'impact d'une compromission d'un serveur exposé sur le LAN interne.</details>

## 🔗 Liens
[18 - Protocoles essentiels](18%20-%20Protocoles%20essentiels.md) · [20 - NAT et PAT](20%20-%20NAT%20et%20PAT.md) · [[31 - Architecture d'entreprise et haute disponibilité]] · [00 - Index](00%20-%20Index.md)
