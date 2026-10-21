---
titre: Vocabulaire de l'infrastructure
formateur: Elimane NDIAYE
niveau: débutant
tags: [cours, cloud, devops, virtualisation, saas, iaas]
---

# 01 — Vocabulaire de l'infrastructure

Suivant : [[02 - Introduction aux réseaux]] · Index : [00 - Index](00%20-%20Index.md)

> [!question] Pourquoi commencer par là ?
> Ces mots reviennent dans tout le cours. Les comprendre dès le départ évite de se perdre plus tard.

## 1. Infrastructure informatique
**Définition** : l'ensemble des ressources matérielles et logicielles qui permettent de faire fonctionner les services d'une organisation : serveurs, stockage, **réseau** ([[02 - Introduction aux réseaux]]), systèmes d'exploitation, hyperviseurs, sécurité, supervision.

**Exemple** : quand tu ouvres ta messagerie, des serveurs, un réseau, du stockage et des logiciels travaillent ensemble en arrière-plan. Tout cela, c'est l'infrastructure.

**On-premise** (« sur site ») : l'infrastructure est dans les locaux de l'entreprise, qui l'achète et la gère. Le **cloud** est présenté plus bas.

---

## 2. La virtualisation

**Définition** : technique qui permet de faire fonctionner **plusieurs systèmes isolés (machines virtuelles, VM) sur une seule machine physique**, en partageant ses ressources (CPU, RAM, disque, réseau).

Le logiciel qui rend cela possible s'appelle l'**hyperviseur**.

```text
SANS virtualisation            AVEC virtualisation
┌───────────────┐              ┌────┐ ┌────┐ ┌────┐
│  1 serveur    │              │VM 1│ │VM 2│ │VM 3│
│  1 application│              │Web │ │BDD │ │AD  │
└───────────────┘              ├────┴─┴────┴─┴────┤
  (souvent sous-utilisé)       │    Hyperviseur    │
                               ├───────────────────┤
                               │ 1 serveur physique│
                               └───────────────────┘
```

| Type | Description | Exemples |
|---|---|---|
| **Type 1** (bare-metal) | installé directement sur le matériel, utilisé en production | VMware ESXi, Hyper-V, Proxmox VE, KVM |
| **Type 2** (hébergé) | s'exécute comme une application dans un OS existant, utile pour tester | VirtualBox, VMware Workstation |

**Avantages** : économie de matériel, isolation, snapshots, déploiement rapide, migration de VM.
**Exemple concret** : un TP avec 3 VM (un serveur Linux, un serveur Windows, un client) sur un seul PC portable avec VirtualBox.

---

## 3. Le cloud (cloud computing)

**Définition (NIST)** : accès **à la demande, via un réseau**, à des ressources informatiques partagées (serveurs, stockage, applications) qui peuvent être provisionnées rapidement avec un minimum d'effort de gestion.

**Caractéristiques essentielles** :
1. libre-service à la demande ;
2. accès large via le réseau (Internet) ;
3. mutualisation des ressources ;
4. **élasticité** (monter ou descendre en charge rapidement) ;
5. service **mesuré** (paiement à l'usage).

> [!note] Le cloud, c'est « l'ordinateur de quelqu'un d'autre »
> Il repose sur de la **virtualisation** et sur des **réseaux**. Sans ces deux briques, pas de cloud.

### Modèles de déploiement
| Modèle | Description | Exemple |
|---|---|---|
| Public | ressources partagées, hébergées par un fournisseur | AWS, Azure, Google Cloud |
| Privé | dédié à une organisation | infrastructure VMware/Proxmox interne |
| Hybride | mélange des deux | AD interne + services Azure |

---

## 4. Les modèles de service : IaaS, PaaS, SaaS

### IaaS — Infrastructure as a Service
Le fournisseur loue l'**infrastructure** (VM, stockage, réseau virtuel). **Tu gères** l'OS, les logiciels et les données.
- **Exemples** : AWS EC2, Azure Virtual Machines, Google Compute Engine, OVHcloud, Scaleway.
- **Cas d'usage** : héberger un serveur web Linux sans acheter de serveur.

### PaaS — Platform as a Service
Le fournisseur fournit une **plateforme** prête à l'emploi (OS, exécution, base de données). **Tu déploies** ton code.
- **Exemples** : Azure App Service, Heroku, Google App Engine.

### SaaS — Software as a Service
Le fournisseur fournit une **application complète**, utilisée via navigateur ou app. **Tu utilises** le service.
- **Exemples** : Microsoft 365, Gmail, Salesforce, Slack, Zoom.

### Qui gère quoi ?
| Couche | On-premise | IaaS | PaaS | SaaS |
|---|:-:|:-:|:-:|:-:|
| Application | Vous | Vous | Vous | Fournisseur |
| Données | Vous | Vous | Vous | Vous |
| Exécution / middleware | Vous | Vous | Fournisseur | Fournisseur |
| Système d'exploitation | Vous | Vous | Fournisseur | Fournisseur |
| Virtualisation | Vous | Fournisseur | Fournisseur | Fournisseur |
| Serveurs, stockage, réseau physique | Vous | Fournisseur | Fournisseur | Fournisseur |

> [!tip] Analogie du repas
> **On-premise** = tu cuisines tout chez toi. **IaaS** = tu loues une cuisine équipée. **PaaS** = tu commandes les ingrédients et la recette est prête. **SaaS** = tu manges au restaurant.

---

## 5. DevOps

**Définition** : culture et ensemble de pratiques qui rapprochent les équipes de **développement (Dev)** et d'**exploitation (Ops)** pour livrer des logiciels **plus vite, plus souvent et plus fiablement**.

Ce n'est **pas** un outil ni un métier unique : c'est une **manière de travailler**, appuyée sur des outils.

```text
   Planifier → Coder → Construire → Tester → Livrer → Déployer → Exploiter → Superviser
       ▲                                                                       │
       └───────────────────────── retours d'expérience ◄───────────────────────┘
```

**Pratiques clés** :
- **CI/CD** : intégration continue (tests automatiques à chaque modification) et livraison/déploiement continus ;
- **Infrastructure as Code (IaC)** : décrire l'infrastructure dans des fichiers versionnés (Terraform, Ansible) plutôt que la configurer à la main ;
- **Automatisation** des tâches répétitives ;
- **Supervision** et retour d'information continus ;
- **Conteneurs** (Docker) pour empaqueter une application avec ses dépendances.

**Exemple** : un développeur pousse du code sur Git → un pipeline lance les tests → construit une image Docker → la déploie automatiquement sur un serveur → la supervision alerte en cas d'erreur.

## 📌 À retenir
- Virtualisation = plusieurs VM sur une machine physique, grâce à l'**hyperviseur**.
- Cloud = ressources à la demande via un réseau, souvent construit sur la virtualisation.
- **IaaS** = infrastructure, **PaaS** = plateforme, **SaaS** = application.
- DevOps = culture Dev + Ops, avec automatisation, CI/CD et IaC.

## 🧪 Exercices
1. Classe en IaaS / PaaS / SaaS : Google Docs, une VM Azure, Heroku, Outlook 365, un serveur Linux chez OVHcloud.
2. Pourquoi la virtualisation est-elle indispensable au cloud ?
3. Cite deux avantages et deux limites du cloud public.

<details><summary>Réponses</summary>

1. SaaS, IaaS, PaaS, SaaS, IaaS.
2. Elle permet de partager un serveur physique entre plusieurs clients de façon isolée, de créer des ressources en quelques secondes et de les facturer à l'usage.
3. Avantages : élasticité, pas d'achat de matériel. Limites : dépendance au fournisseur, coûts variables, conformité et localisation des données.
</details>

## 🔗 Liens
[[02 - Introduction aux réseaux]] · [[03 - Types de réseaux (LAN, WLAN, WAN, MAN, Internet)]] · [00 - Index](00%20-%20Index.md)
