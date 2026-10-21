---
titre: Diagnostic réseau (méthodologie)
formateur: Elimane NDIAYE
niveau: 2e année
tags: [cours, diagnostic, dépannage, wireshark, tcpdump]
---

# 30 — Diagnostic et dépannage réseau

Précédent : [29 - Supervision et logs](29%20-%20Supervision%20et%20logs.md) · Suivant : [[31 - Architecture d'entreprise et haute disponibilité]]

## 1. Méthodologie professionnelle
1. **Décrire le problème** précisément : qui ? quoi ? depuis quand ? Quel message d'erreur ?
2. **Collecter les faits** : périmètre (un poste ? un VLAN ? tout le site ?), **changements récents**, ce qui marche encore.
3. **Formuler des hypothèses** par probabilité et facilité de test.
4. **Tester une hypothèse à la fois**, noter les résultats.
5. **Isoler** la cause (couche, équipement, segment).
6. **Corriger** (avec plan de retour arrière si changement risqué).
7. **Vérifier** que le service est rétabli **côté utilisateur**.
8. **Documenter** (cause, solution, prévention) et améliorer la supervision.

### Approches
| Approche | Principe | Quand |
|---|---|---|
| **Bas → haut** (couche 1 → 7) | commencer par le physique | panne totale, doute matériel |
| **Haut → bas** (7 → 1) | commencer par l'application | un seul service KO |
| **Diviser pour régner** | tester au milieu du chemin (ex. la passerelle) et réduire | chemin complexe |
| **Comparer** | comparer avec une machine qui fonctionne | postes similaires |
| **Suivre le chemin** | de la source à la destination, saut par saut | traceroute |

## 2. Procédure type « un poste n'accède pas à un site »
```text
1. Lien physique / Wi-Fi actif ?              → LED, ip link, ipconfig
2. IP, masque, passerelle, DNS corrects ?     → ipconfig /all, ip a   (169.254 ? → DHCP)
3. ping 127.0.0.1                             → pile TCP/IP locale
4. ping IP du poste                           → interface locale
5. ping passerelle                            → LAN + ARP (ip neigh)
6. ping IP externe (8.8.8.8)                  → routage / NAT / FAI
7. ping nom (exemple.fr)                      → DNS
8. Port du service ouvert ?                   → nc -zv hôte 443, Test-NetConnection
9. Service applicatif OK ?                    → curl -v, logs serveur
```
**Lecture des résultats**
| Constat | Cause probable |
|---|---|
| Passerelle OK, `8.8.8.8` KO | route par défaut, NAT, FAI, firewall |
| `8.8.8.8` OK, nom KO | **DNS** |
| Ping OK, service KO | port bloqué, service arrêté, firewall |
| Tout KO, IP 169.254.x.x | **DHCP** / lien / VLAN |
| IP correcte mais isolé du LAN | mauvais **VLAN**, masque erroné, MAC/ARP |
| Intermittent | boucle STP, doublon d'IP, Wi-Fi, câble défectueux |
| Lent | congestion, duplex mismatch, DNS lent, perte de paquets |

## 3. Commandes
| Outil | Usage | Windows | Linux |
|---|---|---|---|
| Configuration IP | vérifier | `ipconfig /all` | `ip a`, `nmcli` |
| Joignabilité | tester | `ping` | `ping -c 4` |
| Chemin | sauts | `tracert` | `traceroute -n`, `mtr` |
| Routes | table | `route print` | `ip route` |
| ARP / voisins | MAC | `arp -a` | `ip neigh` |
| Ports | connexions | `netstat -ano` | `ss -tulpn` |
| Port distant | tester | `Test-NetConnection h -Port 443` | `nc -zv h 443` |
| DNS | résolution | `nslookup` | `dig`, `resolvectl` |
| Web | HTTP | `curl.exe -v` | `curl -v` |
| Logs | erreurs | Observateur d'événements | `journalctl -xe` |
Astuce **MTU** : `ping -f -l 1472 8.8.8.8` (Windows) / `ping -M do -s 1472 8.8.8.8` (Linux).

## 4. Analyse de paquets : Wireshark et tcpdump
**Quand ?** quand les outils simples ne suffisent pas : voir **ce qui circule réellement** (handshake TCP qui échoue, DHCP sans réponse, retransmissions, DNS erroné).

**tcpdump** (terminal, serveurs)
```bash
sudo tcpdump -i eth0 -n                      # tout (à éviter sans filtre)
sudo tcpdump -i eth0 -n host 192.168.1.10
sudo tcpdump -i eth0 -n port 53 or port 67
sudo tcpdump -i eth0 -n 'tcp[tcpflags] & tcp-syn != 0'
sudo tcpdump -i eth0 -w capture.pcap         # enregistrer pour Wireshark
```
**Wireshark** (graphique). Filtres d'affichage :
```text
ip.addr == 192.168.1.10
tcp.port == 443
dns          arp          dhcp          icmp
tcp.flags.syn == 1 && tcp.flags.ack == 0
tcp.analysis.retransmission
http.request.method == "GET"
eth.dst == ff:ff:ff:ff:ff:ff
```
**Lecture typique**
- SYN sans SYN-ACK → firewall/serveur/port fermé (RST = port fermé).
- Nombreuses **retransmissions** → perte, congestion.
- DHCP Discover sans Offer → serveur/relay absent.
- ARP Request sans Reply → hôte absent, mauvais VLAN.
- DNS sans réponse → serveur DNS injoignable/filtré.
> [!warning] Capturer uniquement sur des réseaux dont tu as l'autorisation ; les captures contiennent des données sensibles (mots de passe non chiffrés, cookies).

## 5. Scénarios corrigés
1. **« Internet ne marche pas mais le mail interne oui »** → ping 8.8.8.8 KO, passerelle OK → route par défaut/NAT côté routeur.
2. **« Le site est lent »** → mtr : perte au saut 5 → problème chez le FAI/lien saturé ; vérifier bande passante.
3. **« Impossible de joindre le serveur de fichiers »** → ping IP OK, `Test-NetConnection -Port 445` KO → firewall/service SMB.
4. **« Tout le réseau ralentit, LED clignotantes »** → tempête de broadcast → boucle ([11 - VLAN, trunk, STP et EtherChannel](11%20-%20VLAN%2C%20trunk%2C%20STP%20et%20EtherChannel.md)), vérifier STP.
5. **« Ping IP OK, nom KO »** → DNS ([16 - DNS](16%20-%20DNS.md)).

## 6. Réflexes
Un seul changement à la fois ; **ne pas supposer, vérifier** ; comparer avec un poste sain ; regarder **les logs** ; ne pas oublier le **physique** (câble, prise, alimentation) ; documenter.

## 📌 À retenir
- Méthode : décrire → hypothèses → tester → isoler → corriger → vérifier → documenter.
- Modèle OSI = guide de diagnostic. Ping progressif : local → passerelle → externe → nom → port.
- tcpdump/Wireshark pour vérifier ce qui passe réellement.

## 🧪 Exercices
1. `ping 8.8.8.8` OK, `ping google.com` KO. Cause ?
2. Un poste a 169.254.3.7. Première vérification ?
3. Quel filtre Wireshark pour voir uniquement les requêtes DNS ?
<details><summary>Réponses</summary>1. DNS. 2. Serveur DHCP/lien/VLAN/relay. 3. `dns`.</details>

## 🔗 Liens
[[08 - Modèle OSI]] · [15 - TCP, UDP et ports](15%20-%20TCP%2C%20UDP%20et%20ports.md) · [29 - Supervision et logs](29%20-%20Supervision%20et%20logs.md) · [00 - Index](00%20-%20Index.md)
