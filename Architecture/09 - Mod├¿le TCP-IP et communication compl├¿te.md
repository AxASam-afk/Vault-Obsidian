---
titre: Modèle TCP/IP et exemple de communication
formateur: Elimane NDIAYE
niveau: 1re année
tags: [cours, tcp-ip, encapsulation]
---

# 09 — Modèle TCP/IP et communication complète

Précédent : [[08 - Modèle OSI]] · Suivant : [10 - Switch, table MAC et ARP](10%20-%20Switch%2C%20table%20MAC%20et%20ARP.md)

## 1. Le modèle TCP/IP
Modèle **pratique** utilisé par Internet, en **4 couches** (parfois 5 en séparant physique et liaison).

## 2. Correspondance OSI / TCP-IP

| OSI | TCP/IP (4 couches) | Exemples |
|---|---|---|
| 7 Application<br>6 Présentation<br>5 Session | **Application** | HTTP, DNS, SSH, SMTP, DHCP |
| 4 Transport | **Transport** | TCP, UDP |
| 3 Réseau | **Internet** | IP, ICMP, ARP (à la frontière) |
| 2 Liaison<br>1 Physique | **Accès réseau** | Ethernet, Wi-Fi |

> [!note] OSI = référence pédagogique ; TCP/IP = modèle réellement implémenté.

## 3. Exemple complet : ouvrir `http://www.exemple.fr`
Situation : PC `192.168.1.10/24` (passerelle `192.168.1.1`, DNS `192.168.1.1`).

1. **DNS (application)** : le PC demande l'IP de `www.exemple.fr` → réponse `93.184.216.34`.
2. **Décision de routage (réseau)** : `93.184.216.34` est **hors** du LAN → il faut passer par la **passerelle**.
3. **ARP (liaison)** : le PC demande en **broadcast** « qui a 192.168.1.1 ? » → la passerelle répond avec sa **MAC**.
4. **TCP handshake (transport)** : SYN → SYN-ACK → ACK vers le port **80** (ou 443 en HTTPS).
5. **Requête HTTP (application)** : `GET / HTTP/1.1`.
6. **Encapsulation** : `[Eth: MAC dest = MAC passerelle][IP: dest 93.184.216.34][TCP: port 80][HTTP]`.
7. **Routage** : chaque routeur regarde l'**IP destination** et **réécrit la trame** (nouvelles MAC) à chaque saut. Les **IP source/destination ne changent pas** (sauf NAT), les **MAC changent à chaque saut**.
8. **Réponse** : chemin inverse, puis désencapsulation et affichage de la page.

```text
PC ──switch──► Routeur/Box ──► FAI ──► ... ──► Serveur web
MAC change à chaque saut ─ IP de bout en bout (NAT en sortie)
```

> [!tip] Règle d'or
> **MAC = saut par saut** (local) ; **IP = de bout en bout**.

## 📌 À retenir
- TCP/IP : Application, Transport, Internet, Accès réseau.
- Le PC utilise la **MAC de la passerelle** pour les destinations hors LAN.

## 🧪 Exercice
Dans une requête vers un serveur distant, quelle est l'adresse MAC de destination de la trame sortant du PC ?
<details><summary>Réponse</summary>La MAC de la **passerelle par défaut**, pas celle du serveur.</details>

## 🔗 Liens
[10 - Switch, table MAC et ARP](10%20-%20Switch%2C%20table%20MAC%20et%20ARP.md) · [15 - TCP, UDP et ports](15%20-%20TCP%2C%20UDP%20et%20ports.md) · [16 - DNS](16%20-%20DNS.md) · [19 - Routage](19%20-%20Routage.md) · [00 - Index](00%20-%20Index.md)
