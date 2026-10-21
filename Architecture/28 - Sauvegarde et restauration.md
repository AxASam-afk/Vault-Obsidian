---
titre: Sauvegarde et restauration
formateur: Elimane NDIAYE
niveau: 2e année
tags: [cours, sauvegarde, rpo, rto, pra, pca]
---

# 28 — Sauvegarde et restauration

Précédent : [27 - Stockage et RAID](27%20-%20Stockage%20et%20RAID.md) · Suivant : [29 - Supervision et logs](29%20-%20Supervision%20et%20logs.md)

## 1. Pourquoi sauvegarder ?
Pannes matérielles, erreurs humaines, **ransomware**, corruption, sinistre (incendie, inondation), vol, obligations légales. **Une sauvegarde non testée n'est pas une sauvegarde.** Ne pas confondre avec RAID, snapshot ou réplication.

## 2. La règle 3-2-1
- **3** copies des données (production + 2 sauvegardes) ;
- sur **2** supports différents ;
- dont **1** copie **hors site** (ou cloud).
Variante moderne **3-2-1-1-0** : + 1 copie **hors ligne/immuable** (protection ransomware) + **0** erreur après vérification.

## 3. Types de sauvegarde
| Type | Contenu | Espace | Vitesse sauvegarde | Restauration |
|---|---|---|---|---|
| **Complète** | **tout** | important | lente | **simple** (1 jeu) |
| **Incrémentale** | changements depuis la **dernière sauvegarde** (quelle qu'elle soit) | faible | rapide | **longue** (complète + toutes les incrémentales) |
| **Différentielle** | changements depuis la **dernière complète** | moyen (croît) | moyenne | moyenne (complète + dernière différentielle) |
```text
Lun  Mar  Mer  Jeu  Ven
 C    I1   I2   I3   I4      Restauration jeudi : C + I1 + I2 + I3
 C    D1   D2   D3   D4      Restauration jeudi : C + D3
```
Stratégie courante : **complète hebdomadaire + incrémentales/différentielles quotidiennes**. Snapshots de stockage et sauvegardes **image** de VM, sauvegardes applicatives cohérentes (BDD).
Outils : Veeam, Bacula/Bareos, `rsync`, `restic`/`borg`, Windows Server Backup, Proxmox Backup Server.

## 4. RPO et RTO
| | Signification | Question |
|---|---|---|
| **RPO** (Recovery Point Objective) | **perte de données maximale** acceptable | « jusqu'à quand peut-on revenir en arrière ? » |
| **RTO** (Recovery Time Objective) | **durée maximale** d'interruption acceptable | « en combien de temps doit-on redémarrer ? » |
```text
        RPO                   RTO
◄──────────────►      ◄──────────────►
dernière sauvegarde   PANNE          service rétabli
```
**Exemple** : sauvegarde toutes les 24 h → RPO ≈ 24 h. Pour un RPO de 15 min, il faut de la **réplication/journalisation** fréquente. Un RTO de 1 h exige des procédures et une infra prêtes.

## 5. Restauration et tests
- Types : fichier, VM complète, bare-metal, base de données, **restauration granulaire** (un objet AD).
- **Tester régulièrement** la restauration (procédure documentée, mesure du temps réel = RTO, vérification d'intégrité).
- Chiffrer les sauvegardes, contrôler leurs accès, protéger contre la suppression (immutabilité), **isoler** du domaine de production.
- Surveiller les jobs (alertes en cas d'échec) ([29 - Supervision et logs](29%20-%20Supervision%20et%20logs.md)).

## 6. PRA et PCA
| | **PCA** — Plan de Continuité d'Activité | **PRA** — Plan de Reprise d'Activité |
|---|---|---|
| Objectif | **maintenir** l'activité **sans interruption** | **reprendre** l'activité après un sinistre |
| Approche | redondance, haute disponibilité (actif/actif), sites multiples | restauration, site de secours, procédures |
| Interruption | quasi nulle | acceptée dans la limite du RTO |
| Exemple | cluster + 2 sites synchrones | restauration sur site de secours en 4 h |
Éléments clés : analyse d'**impact** (BIA), inventaire des services critiques, priorités de reprise, rôles, contacts, **exercices** périodiques.

## 📌 À retenir
- 3-2-1 (+ copie immuable) ; complète / incrémentale / différentielle.
- RPO = perte de données tolérée ; RTO = durée d'arrêt tolérée.
- **Tester la restauration.** PCA = continuer ; PRA = redémarrer.

## 🧪 Exercices
1. Sauvegarde complète le dimanche, incrémentales chaque jour. Panne mercredi soir : que restaurer ?
2. Objectif : perdre au plus 1 h de données. Une sauvegarde nocturne suffit-elle ?
<details><summary>Réponses</summary>1. Dimanche + lundi + mardi + mercredi (incrémentales). 2. Non (RPO 24 h) : sauvegardes/réplication plus fréquentes.</details>

## 🔗 Liens
[27 - Stockage et RAID](27%20-%20Stockage%20et%20RAID.md) · [[26 - Virtualisation avancée]] · [[31 - Architecture d'entreprise et haute disponibilité]] · [00 - Index](00%20-%20Index.md)
