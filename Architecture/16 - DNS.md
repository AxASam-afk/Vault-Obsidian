---
titre: DNS
formateur: Elimane NDIAYE
niveau: 1re-2e année
tags: [cours, dns, résolution]
---

# 16 — DNS (Domain Name System)

Précédent : [15 - TCP, UDP et ports](15%20-%20TCP%2C%20UDP%20et%20ports.md) · Suivant : [17 - DHCP](17%20-%20DHCP.md)

## 1. Rôle
Le DNS est l'**annuaire d'Internet** : il traduit un **nom** (`www.exemple.fr`) en **adresse IP** (et inversement). Port **53** (UDP surtout, TCP pour les gros transferts/zones), couche application.

## 2. Hiérarchie
```text
                    .   (racine)
        ┌───────────┼───────────┐
      .com        .fr         .org       ← TLD
        │           │
   exemple.com   exemple.fr              ← domaine
        │
   www.exemple.fr                        ← hôte (FQDN)
```
- **Root servers** : 13 identités (a à m), répliquées par **anycast** dans le monde.
- **TLD** : `.com`, `.fr`, `.org`, `.sn`… (génériques ou géographiques).
- **FQDN** : nom complet, `www.exemple.fr.` (le point final = racine).
- **Zone** : partie de l'espace de noms gérée par un serveur/administrateur, décrite dans un **fichier de zone**.

## 3. Types de serveurs
| Type | Rôle |
|---|---|
| **Résolveur récursif** (cache) | interroge les autres serveurs pour le client (FAI, 8.8.8.8, 1.1.1.1, DNS d'entreprise) |
| **Serveur autoritatif** | détient **la vraie réponse** pour une zone |
| Serveur racine / TLD | orientent vers le bon serveur |
| Forwarder | transmet les requêtes à un autre résolveur |

## 4. Résolution de nom (récursive + itérative)
Le PC veut `www.exemple.fr` :
1. Cache local / fichier `hosts` → sinon
2. **Requête récursive** au résolveur (`192.168.1.1`).
3. Le résolveur (si pas en cache) interroge **itérativement** : **racine** → « voir `.fr` » ; **TLD .fr** → « voir les NS d'exemple.fr » ; **autoritatif** → « `93.184.216.34` ».
4. Il **met en cache** (selon le **TTL**) et répond au PC.

## 5. Enregistrements (Resource Records)
| Type | Rôle | Exemple |
|---|---|---|
| **A** | nom → IPv4 | `www  A  93.184.216.34` |
| **AAAA** | nom → IPv6 | `www  AAAA  2001:db8::1` |
| **CNAME** | alias vers un autre nom | `blog  CNAME  www.exemple.fr.` |
| **MX** | serveur de messagerie (avec priorité) | `@  MX 10  mail.exemple.fr.` |
| **NS** | serveurs de noms de la zone | `@  NS  ns1.exemple.fr.` |
| **TXT** | texte libre (SPF, DKIM, vérifications) | `@  TXT "v=spf1 mx -all"` |
| **PTR** | IP → nom (reverse) | `34.216.184.93.in-addr.arpa. PTR www…` |
| **SOA** | infos d'autorité de la zone (serial, refresh…) | |
| **SRV** | localise un service (AD, SIP) | `_ldap._tcp…` |

## 6. Reverse DNS
Résolution **IP → nom** via la zone `in-addr.arpa` (IPv4, octets inversés) ou `ip6.arpa`. Utile pour les logs, l'anti-spam.

## 7. TTL et cache
**TTL** (Time To Live) = durée (s) pendant laquelle une réponse peut rester en cache. TTL long = moins de requêtes mais changements lents à se propager ; TTL court = changement rapide (migration).

## 8. Diagnostic
```bash
nslookup www.exemple.fr              # Windows/Linux
nslookup -type=MX exemple.fr 8.8.8.8
dig www.exemple.fr                   # Linux
dig +short A www.exemple.fr
dig MX exemple.fr @1.1.1.1
dig +trace www.exemple.fr            # suit la délégation depuis la racine
dig -x 93.184.216.34                 # reverse
ipconfig /displaydns  |  ipconfig /flushdns     # cache Windows
resolvectl query exemple.fr          # Linux (systemd-resolved)
```
**Problèmes courants** : serveur DNS incorrect dans la config IP ; « ping IP marche mais ping nom échoue » = **problème DNS** ; cache obsolète ; enregistrement manquant ; port 53 bloqué.

## 9. Sécurité
Empoisonnement de cache / spoofing, amplification DDoS, tunnel DNS. Défenses : **DNSSEC** (signatures), résolveurs restreints, **DoT/DoH** (chiffrement), filtrage.

## 📌 À retenir
- DNS : nom ↔ IP ; hiérarchie racine → TLD → domaine.
- Récursif (résolveur) vs autoritatif (zone).
- A/AAAA/CNAME/MX/NS/TXT/PTR ; TTL contrôle le cache.
- `ping IP` OK mais `ping nom` KO → DNS.

## 🧪 Exercices
1. Différence entre un serveur récursif et autoritatif ?
2. Quel enregistrement pour indiquer le serveur mail ?
3. Comment vérifier le DNS quand un site est inaccessible par nom ?
<details><summary>Réponses</summary>1. Le récursif cherche pour le client ; l'autoritatif détient la zone. 2. MX. 3. `nslookup`/`dig`, comparer avec 8.8.8.8, vider le cache.</details>

## 🔗 Liens
[17 - DHCP](17%20-%20DHCP.md) · [25 - Active Directory et LDAP](25%20-%20Active%20Directory%20et%20LDAP.md) · [[30 - Diagnostic réseau]] · [00 - Index](00%20-%20Index.md)
