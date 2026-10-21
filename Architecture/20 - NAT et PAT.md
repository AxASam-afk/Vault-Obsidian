---
titre: NAT et PAT
formateur: Elimane NDIAYE
niveau: 1re-2e année
tags: [cours, nat, pat]
---

# 20 — NAT et PAT

Précédent : [19 - Routage](19%20-%20Routage.md) · Suivant : [21 - Wi-Fi](21%20-%20Wi-Fi.md)

## 1. Pourquoi NAT ?
Les adresses **privées** ([12 - Adressage IPv4](12%20-%20Adressage%20IPv4.md)) ne sont pas routables sur Internet. Le **NAT** (Network Address Translation) **traduit** les adresses privées en adresses **publiques** à la sortie du réseau, ce qui permet à tout un LAN de **partager peu d'IP publiques** (réponse à la pénurie IPv4).

## 2. Vocabulaire Cisco
- **Inside local** : IP privée de l'hôte interne. **Inside global** : IP publique qui le représente.
- **Outside global** : IP publique du serveur distant.

## 3. Types
| Type | Principe | Usage |
|---|---|---|
| **NAT statique** | 1 IP privée ↔ 1 IP publique **fixe** | serveur publié (web, mail) |
| **NAT dynamique** | IP privées ↔ **pool** d'IP publiques | rare |
| **PAT** (NAT overload) | **plusieurs IP privées → 1 IP publique**, différenciées par le **port** | **cas le plus courant** (box, entreprise) |

### PAT en exemple
```text
PC1 192.168.1.10:50001 ──►┐                       ┌─► 93.184.216.34:443
PC2 192.168.1.11:50001 ──►┤ Routeur PAT           │
                          │ IP publique 82.1.2.3   │
Traduction :  82.1.2.3:40001 ↔ 192.168.1.10:50001
              82.1.2.3:40002 ↔ 192.168.1.11:50001
```
Le routeur mémorise ces correspondances dans sa **table de traduction** et renvoie la réponse à la bonne machine.

### Configuration Cisco (PAT)
```text
R1(config)# access-list 1 permit 192.168.1.0 0.0.0.255
R1(config)# interface g0/0
R1(config-if)# ip nat inside
R1(config)# interface g0/1
R1(config-if)# ip nat outside
R1(config)# ip nat inside source list 1 interface g0/1 overload
show ip nat translations
```
Linux (nftables/iptables) : `iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE`.

## 4. Redirection de ports (port forwarding)
Rend un service interne accessible de l'extérieur : `IP_publique:8080 → 192.168.1.50:80`.

## 5. NAT et sécurité
Le NAT **n'est pas un pare-feu** : il masque les IP internes et bloque les connexions entrantes non sollicitées **de fait**, mais la sécurité vient du **firewall stateful**. En IPv6, l'adressage global est de bout en bout ; on protège par firewall.

## 6. Limites
- **Rupture du bout en bout** : difficile pour VoIP/P2P/IPsec (contournement : STUN, NAT-T).
- Complexifie le diagnostic et la journalisation (traçabilité).
- Nombre de ports limité (~64 000 par IP publique).
- **CGNAT** chez l'opérateur : pas de redirection de ports possible.

## 📌 À retenir
- Statique 1:1, dynamique pool, **PAT = plusieurs → 1 via les ports**.
- NAT ≠ sécurité. NAT retarde IPv6, sans l'éviter.

## 🧪 Exercices
1. Pourquoi le PAT a-t-il besoin des ports ?
2. Comment publier un serveur web interne sur Internet ?
<details><summary>Réponses</summary>1. Pour distinguer les connexions des différents hôtes partageant la même IP. 2. NAT statique ou port forwarding + règle firewall (idéalement en DMZ).</details>

## 🔗 Liens
[19 - Routage](19%20-%20Routage.md) · [[22 - Sécurité réseau]] · [14 - IPv6](14%20-%20IPv6.md) · [00 - Index](00%20-%20Index.md)
