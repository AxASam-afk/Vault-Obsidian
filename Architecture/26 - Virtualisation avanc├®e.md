---
titre: Virtualisation avancée
formateur: Elimane NDIAYE
niveau: 2e année
tags: [cours, virtualisation, hyperviseur, vmware, proxmox, hyper-v, kvm]
---

# 26 — Virtualisation (approfondissement)

Précédent : [25 - Active Directory et LDAP](25%20-%20Active%20Directory%20et%20LDAP.md) · Suivant : [27 - Stockage et RAID](27%20-%20Stockage%20et%20RAID.md)

Définitions de base : [01 - Vocabulaire de l'infrastructure (Cloud, DevOps, Virtualisation)](01%20-%20Vocabulaire%20de%20l%27infrastructure%20%28Cloud%2C%20DevOps%2C%20Virtualisation%29.md).

## 1. Rappel
**Machine virtuelle (VM)** = ordinateur logiciel (CPU, RAM, disque, cartes réseau virtuels) exécuté par un **hyperviseur** sur un hôte physique.
- **Type 1** (bare-metal) : ESXi, Hyper-V, Proxmox VE (KVM), XenServer.
- **Type 2** (hébergé) : VirtualBox, VMware Workstation/Fusion.

## 2. Ressources virtuelles
| Ressource | Notion | Point d'attention |
|---|---|---|
| **vCPU** | cœur virtuel ordonnancé sur les cœurs physiques | trop de vCPU = **attente CPU** (CPU ready) ; commencer petit |
| **RAM** | allouée à la VM ; *ballooning*, *overcommit* possible | le *swap* de l'hôte ruine les performances |
| **Stockage** | **disque virtuel** (fichier : VMDK, VHDX, QCOW2, RAW) ; *thin* (alloué à l'usage) vs *thick* (préalloué) | surveiller le sur-provisionnement thin |
| **Réseau** | **switch virtuel** + cartes virtuelles (VirtIO, VMXNET3) | choisir le bon mode |

## 3. Réseau virtuel
```text
        Hôte physique
   ┌───────────────────────────┐
   │ VM1  VM2       VM3        │
   │  │    │         │         │
   │ ┌┴────┴─────────┴──┐      │
   │ │  Switch virtuel  │      │
   │ └────────┬─────────┘      │
   │      carte physique       │
   └──────────┬────────────────┘
              LAN physique
```
| Mode | Fonctionnement | Usage |
|---|---|---|
| **Bridge (pont)** | la VM est **sur le LAN physique**, avec sa propre IP/MAC | serveurs joignables par tous |
| **NAT virtuel** | la VM sort via l'IP de l'hôte ; **non joignable de l'extérieur** | poste de test avec accès Internet |
| **Host-only** | VM ↔ hôte uniquement | labo isolé |
| **Réseau interne** | VM ↔ VM uniquement | labo, segments isolés |
Support des **VLAN** (port groups, trunk vers le switch physique), NIC teaming.

## 4. Snapshots, templates, clones
- **Snapshot** : **état** de la VM à un instant (disque, RAM). Pratique avant une mise à jour. **Ce n'est PAS une sauvegarde** (même stockage, dégrade les performances si conservé longtemps).
- **Template (modèle)** : VM « golden image » prête à être **déployée** en série (généralisation : sysprep, cloud-init).
- **Clone** : copie d'une VM (lié ou complet).

## 5. Haute disponibilité (HA) et mobilité
- **Cluster** d'hôtes avec **stockage partagé** (SAN/NAS/vSAN/Ceph).
- **Migration à chaud** (vMotion, Live Migration, Proxmox Migrate) : déplacer une VM allumée sans coupure.
- **HA** : si un hôte tombe, les VM sont **redémarrées** sur un autre hôte.
- **Répartition de charge** (DRS) ; **réplication** vers un site secondaire.
- **Quorum** : nécessite un nombre impair de nœuds (ou un témoin) pour éviter le *split-brain*.

## 6. Comparatif des solutions
| Solution | Type | Points clés |
|---|---|---|
| **VMware vSphere/ESXi** | type 1 | référence en entreprise, vCenter, licences payantes |
| **Microsoft Hyper-V** | type 1 | intégré à Windows Server, PowerShell, Failover Cluster |
| **Proxmox VE** | type 1 | libre, KVM + LXC, interface web, cluster, Ceph |
| **KVM** | module noyau Linux | base de nombreuses solutions, géré par `libvirt`/`virsh` |
| **VirtualBox** | type 2 | gratuit, idéal en labo |

Conteneurs (LXC, Docker) : **partage du noyau**, plus légers qu'une VM (voir [32 - Cloud, conteneurs et Infrastructure as Code](32%20-%20Cloud%2C%20conteneurs%20et%20Infrastructure%20as%20Code.md)).

## 7. Bonnes pratiques
Dimensionner au plus juste, **isoler le réseau de gestion**, sauvegarder les VM (pas seulement des snapshots), surveiller CPU/RAM/IO/latence disque, mettre à jour l'hyperviseur, limiter les accès à l'hyperviseur, documenter.

## 📌 À retenir
- Hyperviseur type 1 en production ; type 2 pour tester.
- Modes réseau : bridge, NAT, host-only, interne.
- Snapshot ≠ sauvegarde ; HA + stockage partagé + migration à chaud.

## 🧪 Exercices
1. Quel mode réseau pour un serveur web VM accessible depuis le LAN ?
2. Pourquoi un snapshot n'est-il pas une sauvegarde ?
<details><summary>Réponses</summary>1. Bridge. 2. Il dépend du disque d'origine : si le stockage est perdu ou corrompu, le snapshot l'est aussi.</details>

## 🔗 Liens
[27 - Stockage et RAID](27%20-%20Stockage%20et%20RAID.md) · [28 - Sauvegarde et restauration](28%20-%20Sauvegarde%20et%20restauration.md) · [[31 - Architecture d'entreprise et haute disponibilité]] · [00 - Index](00%20-%20Index.md)
