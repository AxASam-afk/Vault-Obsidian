
---
	tags: [important, reseau, ip] aliases: [IP, Adresse IP, IPv4, IPv6]

# 🌐 Adresse IP

> [!info] Définition Une **adresse IP** (Internet Protocol) est un identifiant numérique unique attribué à chaque appareil connecté à un réseau utilisant le protocole IP. Elle permet d'identifier et de localiser une machine pour lui envoyer/recevoir des données.

---

## 🔢 IPv4

- Codée sur **32 bits**, écrite en **4 octets** séparés par des points : `192.168.1.1`
- Chaque octet va de **0 à 255**
- Espace total : environ **4,3 milliards** d'adresses possibles (insuffisant aujourd'hui → épuisement des IPv4, d'où IPv6)

### Structure : réseau + hôte

Une adresse IPv4 se décompose en **partie réseau** et **partie hôte**, définie par le **masque de sous-réseau** (ex : `255.255.255.0` ou `/24`).

### Classes historiques d'IPv4

|Classe|Plage|Masque par défaut|Usage|
|---|---|---|---|
|**A**|0.0.0.0 – 127.255.255.255|/8|Très grands réseaux|
|**B**|128.0.0.0 – 191.255.255.255|/16|Réseaux moyens|
|**C**|192.0.0.0 – 223.255.255.255|/24|Petits réseaux|
|**D**|224.0.0.0 – 239.255.255.255|—|Multicast|
|**E**|240.0.0.0 – 255.255.255.255|—|Réservé/expérimental|

> [!note] Le système de classes est aujourd'hui largement remplacé par le **CIDR** (adressage sans classe).

---

## 🔒 IP publique vs IP privée

|Type|Rôle|Plages réservées (privées)|
|---|---|---|
|**IP publique**|Visible et routable sur Internet, unique au monde|—|
|**IP privée**|Utilisée en interne (LAN), non routable sur Internet|`10.0.0.0 – 10.255.255.255` (/8)<br>`172.16.0.0 – 172.31.255.255` (/12)<br>`192.168.0.0 – 192.168.255.255` (/16)|

- La traduction entre IP privée (interne) et IP publique (visible sur Internet) se fait via le **NAT** (Network Address Translation), généralement effectué par la box/routeur.

### Autres plages spéciales

|Plage|Usage|
|---|---|
|`127.0.0.0/8`|**Loopback** (localhost, `127.0.0.1`)|
|`169.254.0.0/16`|**APIPA** — adresse auto-attribuée quand le DHCP échoue|
|`0.0.0.0`|Adresse "non spécifiée" / toutes les interfaces|
|`255.255.255.255`|Adresse de **broadcast** limité|

---

## 🧮 CIDR et masque de sous-réseau

- **CIDR** (_Classless Inter-Domain Routing_) : notation `IP/n` où `n` = nombre de bits réservés à la partie réseau
- Exemple : `192.168.1.0/24` → masque `255.255.255.0` → 256 adresses possibles (254 utilisables pour des hôtes)

|Notation CIDR|Masque|Nb d'adresses|
|---|---|---|
|/8|255.0.0.0|16 777 216|
|/16|255.255.0.0|65 536|
|/24|255.255.255.0|256|
|/30|255.255.255.252|4|
|/32|255.255.255.255|1 (hôte unique)|

---

## 🌍 IPv6

- Codée sur **128 bits**, écrite en **8 groupes hexadécimaux** séparés par `:` : `2001:0db8:85a3:0000:0000:8a2e:0370:7334`
- Créée pour pallier l'épuisement des adresses IPv4
- Peut être **compressée** : les zéros consécutifs sont remplacés par `::` (une seule fois dans l'adresse) → `2001:db8:85a3::8a2e:370:7334`
- Pas de NAT nécessaire en théorie (assez d'adresses pour que chaque appareil ait une IP publique unique)

### Types d'adresses IPv6

|Type|Préfixe|Rôle|
|---|---|---|
|**Unicast global**|`2000::/3`|Équivalent d'une IP publique|
|**Link-local**|`fe80::/10`|Communication locale uniquement (non routée)|
|**Loopback**|`::1`|Équivalent de `127.0.0.1`|
|**Multicast**|`ff00::/8`|Diffusion à plusieurs destinataires|

---

## 🧭 Géolocalisation et informations sur une IP (OSINT)

Une IP peut donner plusieurs informations exploitables en OSINT :

| Info                                   | Outil / méthode                                                                   |
| -------------------------------------- | --------------------------------------------------------------------------------- |
| **Propriétaire / FAI / hébergeur**     | WHOIS, RDAP (voir [PRT1 OSINT](../OSINT/Cours/PRT1%20OSINT.md))                                                       |
| **Géolocalisation approximative**      | IPinfo.io, MaxMind, IP2Location                                                   |
| **Services et ports ouverts**          | **Shodan** (voir [PRT1 OSINT](../OSINT/Cours/PRT1%20OSINT.md))                                                        |
| **Réputation / activité malveillante** | AbuseIPDB, VirusTotal                                                             |
| **Résolution nom ↔ IP**                | `nslookup`, `ping`, WHOIS (voir [COMMANDES CMD](COMMANDES%20CMD.md)) |

> [!warning] La géolocalisation par IP donne en général la position du **FAI/datacenter**, pas l'adresse exacte de l'utilisateur — à prendre avec prudence.

---

## 🛠️ Commandes utiles liées à l'IP

|Commande|Rôle|
|---|---|
|`ipconfig` (Windows) / `ifconfig` ou `ip a` (Linux)|Affiche l'IP locale de la machine|
|`ping`|Vérifie qu'une IP répond|
|`tracert` / `traceroute`|Affiche le chemin réseau vers une IP|
|`nslookup`|Traduit un domaine en IP (et inversement)|
|`whois <ip>`|Donne le propriétaire/registre d'une IP|

_(Détail complet dans [COMMANDES CMD](COMMANDES%20CMD.md))_

---

## ✅ À retenir

- [ ] IPv4 = 32 bits, IPv6 = 128 bits
- [ ] IP privée ≠ IP publique, converties via **NAT**
- [ ] Le **CIDR** définit la taille du réseau (`/24`, `/16`…)
- [ ] Une IP publique donne des indices exploitables en OSINT (hébergeur, localisation, services exposés via Shodan)

---

## 🔗 Liens

- [PRT1 OSINT](../OSINT/Cours/PRT1%20OSINT.md)
- #important #reseau #ip