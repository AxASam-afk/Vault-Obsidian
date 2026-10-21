---
titre: Modèle OSI
formateur: Elimane NDIAYE
niveau: 1re année
tags: [cours, osi, modèle]
---

# 08 — Le modèle OSI

Précédent : [[07 - Protocoles, débit, latence et encapsulation]] · Suivant : [[09 - Modèle TCP-IP et communication complète]]

## 1. Pourquoi un modèle en couches ?
Le modèle **OSI** (Open Systems Interconnection, ISO) découpe la communication en **7 couches**. Chaque couche a un rôle précis et ne dialogue qu'avec les couches voisines.

**Intérêts** : comprendre, **diagnostiquer** ("à quelle couche est le problème ?"), permettre l'interopérabilité entre constructeurs.

## 2. Les 7 couches

| N° | Couche | Rôle | PDU | Protocoles / exemples | Équipements |
|:-:|---|---|---|---|---|
| 7 | **Application** | interface avec l'utilisateur/applications | données | HTTP, DNS, SMTP, SSH | proxy, firewall applicatif |
| 6 | **Présentation** | format, chiffrement, compression | données | TLS (en partie), JPEG, ASCII | — |
| 5 | **Session** | ouverture/maintien/fermeture de sessions | données | RPC, NetBIOS | — |
| 4 | **Transport** | transport de bout en bout, ports, fiabilité | segment | TCP, UDP | firewall |
| 3 | **Réseau** | adressage logique, routage | paquet | IP, ICMP, OSPF | **routeur**, switch L3 |
| 2 | **Liaison de données** | adressage MAC, livraison locale | trame | Ethernet, ARP, 802.1Q, Wi-Fi | **switch**, point d'accès |
| 1 | **Physique** | transmission des bits | bits | câbles, fibre, ondes radio, RJ45 | hub, câbles, répéteur |

**Moyen mnémotechnique (7→1)** : « **A**ttention, **P**as **S**i **T**u **R**ates **L**e **P**rogramme » (Application, Présentation, Session, Transport, Réseau, Liaison, Physique).

## 3. Détail des couches

- **Couche 1 — Physique** : signaux électriques, lumineux ou radio ; câblage, connecteurs, débits. *Panne typique* : câble débranché ou coupé.
- **Couche 2 — Liaison** : trames, adresses MAC, contrôle d'erreurs (FCS), commutation, VLAN. *Panne typique* : boucle, mauvais VLAN, doublon de MAC.
- **Couche 3 — Réseau** : adressage IP, routage entre réseaux. *Panne typique* : mauvaise IP, masque ou passerelle, route manquante.
- **Couche 4 — Transport** : ports, segmentation, fiabilité (TCP) ou rapidité (UDP). *Panne typique* : port bloqué par un firewall.
- **Couches 5-6-7** : sessions, chiffrement, applications. *Panne typique* : service arrêté, mauvais certificat, erreur HTTP 500.

## 4. Diagnostic par couche
```text
Le site ne s'ouvre pas :
7  Le service web répond-il ?          → curl, logs
4  Le port 443 est-il joignable ?      → telnet/nc, ss
3  Puis-je pinguer l'IP ? la passerelle ? → ping, traceroute
2  Suis-je bien dans le bon VLAN ? MAC visible ? → arp -a
1  Le câble est-il branché ? lien actif ?  → LED, ip link
```

## 📌 À retenir
- 7 couches : Application, Présentation, Session, Transport, Réseau, Liaison, Physique.
- Switch = couche 2 ; routeur = couche 3 ; TCP/UDP = couche 4.
- PDU : données → segment → paquet → trame → bits.

## 🧪 Exercices
1. À quelle couche travaille un switch classique ? Un routeur ?
2. Sur quelle couche se trouve le protocole ARP ? (rôle : lier IP et MAC → entre 2 et 3, classée en couche 2)
3. Un câble RJ45 débranché : couche ?

<details><summary>Réponses</summary>

1. 2 ; 3. 3. Couche 1.
</details>

## 🔗 Liens
[[09 - Modèle TCP-IP et communication complète]] · [10 - Switch, table MAC et ARP](10%20-%20Switch%2C%20table%20MAC%20et%20ARP.md) · [[30 - Diagnostic réseau]] · [00 - Index](00%20-%20Index.md)
