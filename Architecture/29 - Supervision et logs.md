---
titre: Supervision et logs
formateur: Elimane NDIAYE
niveau: 2e année
tags: [cours, supervision, monitoring, snmp, syslog]
---

# 29 — Supervision (monitoring) et logs

Précédent : [28 - Sauvegarde et restauration](28%20-%20Sauvegarde%20et%20restauration.md) · Suivant : [[30 - Diagnostic réseau]]

## 1. Pourquoi superviser ?
Détecter les problèmes **avant les utilisateurs**, mesurer la **disponibilité**, anticiper les besoins (capacité), diagnostiquer, prouver le respect des **SLA**, détecter les incidents de sécurité.

## 2. Vocabulaire
| Terme | Définition | Exemple |
|---|---|---|
| **Métrique** | **valeur mesurée** dans le temps | CPU = 87 % |
| **Événement** | **fait** survenu à un instant | « service nginx arrêté à 10:02 » |
| **Log** | **enregistrement textuel** d'événements par un système/application | ligne de `/var/log/auth.log` |
| **Alerte** | **notification** déclenchée quand une règle/seuil est franchi | mail « CPU > 90 % depuis 5 min » |
| **Incident** | **interruption ou dégradation** d'un service, à traiter | « le site web est inaccessible » |
Chaîne : **métriques/logs → événement → alerte → incident → résolution**. Une alerte n'est pas forcément un incident (fausse alerte) ; un incident peut être dû à plusieurs alertes.

## 3. Que superviser ?
| Domaine | Indicateurs |
|---|---|
| **Disponibilité** | ping, ports/services répondent, HTTP 200 |
| **CPU** | % utilisation, charge (load average), attente CPU |
| **RAM** | utilisée/libre, swap |
| **Disque** | espace libre (%), IOPS, latence, santé SMART, RAID |
| **Réseau** | bande passante (in/out), erreurs, pertes, latence, état des liens |
| **Applicatif** | temps de réponse, taux d'erreurs, files d'attente, transactions, certificats (expiration) |
| **Sauvegardes** | succès/échec des jobs |
Concepts : **seuils** *warning/critical*, **supervision de boîte noire** (tests de l'extérieur) vs **boîte blanche** (agents), *golden signals* : latence, trafic, erreurs, saturation.

**Disponibilité** :
| SLA | Indisponibilité/an |
|---|---|
| 99 % | ~3,65 jours |
| 99,9 % | ~8 h 45 |
| 99,99 % | ~52 min |
| 99,999 % | ~5 min |

## 4. SNMP
Interroger les équipements : **manager** ↔ **agent** (UDP 161), **traps** (UDP 162, l'équipement prévient), **MIB/OID** (ex. `1.3.6.1.2.1.1.5.0` = nom de l'équipement). **SNMPv3** (authentification + chiffrement) recommandé ([18 - Protocoles essentiels](18%20-%20Protocoles%20essentiels.md)).
```bash
snmpget -v2c -c public 192.168.1.1 1.3.6.1.2.1.1.5.0
snmpwalk -v3 -l authPriv -u superv -a SHA -A ... -x AES -X ... 192.168.1.1
```

## 5. Syslog
Protocole/format de **journalisation centralisée** (UDP 514, TCP 514/6514 TLS).
- **Facility** (source : kern, auth, daemon…) + **severity** (gravité) :
| 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 |
|---|---|---|---|---|---|---|---|
| Emergency | Alert | Critical | Error | Warning | Notice | Info | Debug |
- Centralisation : `rsyslog`, `syslog-ng`, ELK/OpenSearch, Graylog, Loki.
```text
# /etc/rsyslog.d/50-remote.conf
*.* @@logsrv.corp.local:514
```
Intérêts : corrélation, conservation, preuve, résistance à la suppression locale. **NTP indispensable** pour horodater de manière cohérente.

## 6. Outils
| Outil | Usage |
|---|---|
| **Zabbix**, Nagios/Centreon, PRTG | supervision classique (hôtes, services, SNMP) |
| **Prometheus + Grafana** | métriques et tableaux de bord (cloud, conteneurs) |
| **ELK / Graylog / Loki** | logs |
| **Wazuh / SIEM** | sécurité, corrélation |
| Windows | Observateur d'événements (ID 4624/4625/4740…), Performance Monitor |

## 7. Bonnes pratiques
Superviser ce qui **compte pour le service**, seuils réalistes (éviter la **fatigue d'alertes**), alertes **actionnables** avec procédure associée, tableaux de bord clairs, **rétention** et rotation des logs, protéger les logs, **supervision de la supervision**, historique pour planifier la capacité.

## 📌 À retenir
- Métrique (valeur) ≠ événement (fait) ≠ log (trace) ≠ alerte (notification) ≠ incident (impact service).
- SNMP pour interroger, syslog pour centraliser les logs.
- Superviser disponibilité, CPU, RAM, disque, réseau, application.

## 🧪 Exercices
1. Classe : « CPU à 95 % », « ligne d'échec de connexion SSH », « e-mail à l'astreinte », « le site ne répond plus ».
2. À quoi sert un trap SNMP ?
<details><summary>Réponses</summary>1. Métrique, log, alerte, incident. 2. À prévenir le manager immédiatement sans attendre l'interrogation.</details>

## 🔗 Liens
[[30 - Diagnostic réseau]] · [[22 - Sécurité réseau]] · [23 - Linux pour l'infrastructure](23%20-%20Linux%20pour%20l%27infrastructure.md) · [00 - Index](00%20-%20Index.md)
