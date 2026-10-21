---
titre: Stockage et RAID
formateur: Elimane NDIAYE
niveau: 2e année
tags: [cours, stockage, raid, nas, san]
---

# 27 — Stockage et RAID

Précédent : [[26 - Virtualisation avancée]] · Suivant : [28 - Sauvegarde et restauration](28%20-%20Sauvegarde%20et%20restauration.md)

## 1. Supports
| Type | Fonctionnement | Caractéristiques |
|---|---|---|
| **HDD** | plateaux magnétiques tournants | grande capacité, peu cher, **lent** (latence en ms), fragile |
| **SSD (SATA)** | mémoire flash | rapide, silencieux, usure des cellules |
| **SSD NVMe** | flash via **PCIe** | très rapide (Go/s, centaines de milliers d'IOPS) |

## 2. Modes de connexion
| Modèle | Description | Accès | Protocoles |
|---|---|---|---|
| **DAS** | stockage directement relié au serveur | bloc | SATA, SAS |
| **NAS** | stockage **réseau** partageant des **fichiers** | fichier | **SMB**, **NFS** |
| **SAN** | réseau **dédié** offrant des **blocs** (vus comme des disques locaux) | bloc | **iSCSI**, Fibre Channel |
```text
NAS : PC ──LAN──► [NAS]  \\nas\partage  (fichiers)
SAN : Serveur ──réseau stockage──► [Baie SAN] LUN = « disque » vu par le serveur (blocs)
```
Usage : NAS pour partages de fichiers/sauvegardes ; SAN pour hyperviseurs, bases de données. **Stockage objet** (S3) : cloud, scalable.

## 3. Performances
| Mesure | Définition |
|---|---|
| **IOPS** | opérations d'entrée/sortie par seconde (petits accès aléatoires : bases de données) |
| **Débit** | Mo/s (gros transferts séquentiels : copies, sauvegardes) |
| **Latence** | temps de réponse d'une opération (ms) ; critique pour la fluidité |
Ordres de grandeur : HDD ≈ 100–200 IOPS ; SSD SATA ≈ 50–100 k ; NVMe ≈ 500 k et +.

## 4. RAID (Redundant Array of Independent Disks)
Regroupe plusieurs disques pour la **performance** et/ou la **tolérance de panne**. Variables : **n** disques de capacité **C**.

| RAID | Principe | Disques min | Capacité utile | Tolérance | Perf. |
|:-:|---|:-:|---|---|---|
| **0** | **striping** : données réparties, aucune redondance | 2 | n × C | **aucune** ❌ | lecture/écriture ↑↑ |
| **1** | **mirroring** : copie identique | 2 | C (n/2×C) | n−1 disque(s) (au moins 1) | lecture ↑ |
| **5** | striping + **parité distribuée** | 3 | (n−1) × C | **1 disque** | lecture ↑, écriture ↓ (calcul de parité) |
| **6** | striping + **double parité** | 4 | (n−2) × C | **2 disques** | écriture ↓↓ |
| **10** | miroirs (1) **puis** striping (0) | 4 | n/2 × C | 1 disque par miroir | très bon partout |

**Exemple : 4 disques de 2 To**
- RAID 0 : **8 To** (aucune tolérance) · RAID 5 : **6 To** (1 panne) · RAID 6 : **4 To** (2 pannes) · RAID 10 : **4 To** (1 panne par miroir).

```text
RAID 1        RAID 5 (parité tournante)       RAID 10
[A][A]        [A1][A2][Ap]                    ([A][A]) ─ stripe ─ ([B][B])
[B][B]        [B1][Bp][B2]
              [Cp][C1][C2]
```
Points d'attention : **reconstruction** longue (RAID 5 sur gros disques HDD = risque d'une 2ᵉ panne pendant la reconstruction → préférer RAID 6/10), **disque de spare** (secours à chaud), contrôleur matériel vs logiciel (`mdadm`, ZFS, Storage Spaces).

> [!danger] RAID n'est PAS une sauvegarde
> RAID protège **uniquement d'une panne de disque**. Il **ne protège pas** contre : suppression accidentelle, corruption, **ransomware**, erreur humaine, incendie/vol, panne du contrôleur. Un fichier supprimé l'est **immédiatement sur tous les disques**. Il faut **toujours** une vraie sauvegarde ([28 - Sauvegarde et restauration](28%20-%20Sauvegarde%20et%20restauration.md)).

## 5. Commandes utiles (Linux)
```bash
lsblk ; df -h ; du -sh /var/*
cat /proc/mdstat ; sudo mdadm --detail /dev/md0
sudo smartctl -a /dev/sda      # santé du disque (SMART)
```

## 📌 À retenir
- HDD (capacité) vs SSD/NVMe (vitesse) ; NAS = fichiers, SAN = blocs.
- RAID 0/1/5/6/10 : savoir calculer capacité et tolérance.
- **RAID ≠ sauvegarde.**

## 🧪 Exercices
1. 6 disques de 4 To en RAID 5 et en RAID 6 : capacité utile ?
2. Quel RAID pour une base de données très sollicitée en écriture ?
<details><summary>Réponses</summary>1. RAID 5 : 20 To ; RAID 6 : 16 To. 2. RAID 10.</details>

## 🔗 Liens
[[26 - Virtualisation avancée]] · [28 - Sauvegarde et restauration](28%20-%20Sauvegarde%20et%20restauration.md) · [29 - Supervision et logs](29%20-%20Supervision%20et%20logs.md) · [00 - Index](00%20-%20Index.md)
