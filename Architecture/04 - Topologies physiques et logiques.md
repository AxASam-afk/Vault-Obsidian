---
titre: Topologies physiques et logiques
formateur: Elimane NDIAYE
niveau: débutant
tags: [cours, réseau, topologie]
---

# 04 — Topologies physiques et logiques

Précédent : [[03 - Types de réseaux (LAN, WLAN, WAN, MAN, Internet)]] · Suivant : [05 - Ethernet (802.3) et adresse MAC](05%20-%20Ethernet%20%28802.3%29%20et%20adresse%20MAC.md)

> [!abstract] Définitions
> - **Topologie physique** : la **disposition réelle** des équipements et des câbles.
> - **Topologie logique** : le **chemin suivi par les données (trames, paquets)** à travers la topologie physique.

Deux réseaux physiquement identiques peuvent se comporter différemment sur le plan logique.

## 1. Topologies physiques

### Point à point
Deux équipements reliés directement.
```text
PC ───────── PC
```

### Bus (linéaire)
Toutes les machines partagent **un même câble**.
```text
PC ─── PC ─── PC ─── PC
```
- ✅ simple, peu de câble ; ❌ une coupure paralyse tout, collisions, **obsolète**.

### Étoile
Chaque machine est reliée à un **équipement central** (switch).
```text
        PC
        │
PC ──── Switch ──── PC
        │
        PC
```
- ✅ facile à administrer, une panne de câble n'isole qu'une machine ; ❌ le switch est un **point unique de défaillance**.
- **Topologie dominante aujourd'hui** dans les LAN Ethernet.

### Anneau
Chaque machine est reliée à deux voisines, formant une boucle.
```text
PC ───── PC
│         │
PC ───── PC
```
- ✅ trafic organisé ; ❌ une rupture peut casser l'anneau (sauf double anneau). Utilisé historiquement (Token Ring) et en fibre (FDDI, SDH).

### Maillée (mesh)
**Plusieurs chemins** possibles entre les équipements.
```text
PC ─────── PC
│ ╲     ╱  │
│   ╲ ╱    │
│   ╱ ╲    │
PC ─────── PC       (maillage complet)
```
- **Complet** : chaque nœud est relié à tous les autres → **n(n−1)/2 liens**.
- **Partiel** : quelques liens redondants seulement.
- ✅ très résilient ; ❌ coûteux, complexe. Exemples : Internet, cœur de réseau, WAN.

### Arbre et hybride
- **Arbre** : étoiles hiérarchisées (switch d'accès → distribution → cœur).
- **Hybride** : combinaison de plusieurs topologies (cas réel en entreprise).

## 2. Topologies logiques

| Topologie logique | Fonctionnement | Exemple |
|---|---|---|
| **Broadcast / bus logique** | tout signal envoyé est vu par **tous** | Ethernet avec **hub**, Wi-Fi (support partagé) |
| **Commutée (switched)** | le switch envoie la trame **uniquement** vers le destinataire | Ethernet moderne avec switch |
| **Anneau logique** | passage d'un **jeton** de machine en machine | Token Ring |

> [!example] Physique ≠ logique
> Un réseau Ethernet à **hub** est physiquement en **étoile** mais logiquement en **bus** (tout le monde reçoit tout). Avec un **switch**, il reste physiquement en étoile, mais les échanges deviennent **point à point** logiquement.

## 3. Comparatif

| Topologie | Coût | Résilience | Usage actuel |
|---|---|---|---|
| Bus | faible | faible | disparu |
| Étoile | moyen | moyenne | LAN d'entreprise, maison |
| Anneau | moyen | moyenne | opérateurs (fibre) |
| Maillée | élevé | élevée | cœur de réseau, Internet |
| Hybride | variable | bonne | réseaux d'entreprise |

## 📌 À retenir
- Physique = câblage ; logique = circulation des données.
- L'étoile domine dans les LAN ; le maillage assure la redondance (voir [STP](11%20-%20VLAN%2C%20trunk%2C%20STP%20et%20EtherChannel.md) pour éviter les boucles).
- Maillage complet : **n(n−1)/2** liens.

## 🧪 Exercices
1. Combien de liens pour un maillage complet de 6 machines ?
2. Quel est le point faible d'une topologie en étoile ?
3. Un réseau à hub est-il en étoile ou en bus ? (physique / logique)

<details><summary>Réponses</summary>

1. 6×5/2 = **15** liens.
2. Le switch central : s'il tombe, tout s'arrête.
3. Étoile physiquement, bus logiquement.
</details>

## 🔗 Liens
[[02 - Introduction aux réseaux]] · [05 - Ethernet (802.3) et adresse MAC](05%20-%20Ethernet%20%28802.3%29%20et%20adresse%20MAC.md) · [00 - Index](00%20-%20Index.md)
