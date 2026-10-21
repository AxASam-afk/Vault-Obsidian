---
titre: Active Directory et LDAP
formateur: Elimane NDIAYE
niveau: 2e année
tags: [cours, active-directory, ldap, kerberos]
---

# 25 — Active Directory et LDAP

Précédent : [24 - Windows Server](24%20-%20Windows%20Server.md) · Suivant : [[26 - Virtualisation avancée]]

## 1. Annuaire
Un **annuaire** est une **base de données hiérarchique** d'objets (utilisateurs, ordinateurs, groupes, imprimantes) optimisée pour la lecture. **Active Directory (AD DS)** est l'annuaire de Microsoft, qui centralise **authentification, autorisation et administration**.

## 2. Structure logique
```text
Forêt : corp.local
 ├─ Arbre / Domaine : corp.local
 │    ├─ OU=Compta        (utilisateurs, groupes, PC)
 │    ├─ OU=RH
 │    └─ OU=Serveurs
 └─ Domaine enfant : paris.corp.local
```
| Élément | Définition |
|---|---|
| **Domaine** | frontière d'administration et de réplication, avec ses utilisateurs et sa politique |
| **Arbre** | domaines partageant un espace de noms contigu (corp.local, paris.corp.local) |
| **Forêt** | ensemble d'arbres partageant schéma et catalogue global ; **frontière de sécurité** |
| **OU** | conteneur pour organiser, déléguer, appliquer des GPO |
| **Objets** | utilisateurs, groupes, **comptes d'ordinateurs** (les machines jointes ont un compte) |
| **Contrôleur de domaine (DC)** | serveur hébergeant l'AD et authentifiant |
| **Catalogue global (GC)** | index partiel de toute la forêt (port 3268) |
| **Sites** | topologie physique (sous-réseaux) pour optimiser la réplication |

**Groupes** : types *sécurité* / *distribution* ; étendues *domaine local, globale, universelle*. Bonne pratique **AGDLP** : **A**ccounts → **G**lobal groups → **D**omain **L**ocal groups → **P**ermissions.

## 3. DNS et Active Directory
AD **dépend du DNS** : les clients trouvent les DC grâce aux enregistrements **SRV** (`_ldap._tcp.dc._msdcs.corp.local`, `_kerberos._tcp…`). Panne d'AD fréquente = **panne DNS** (client pointant vers un DNS externe).

## 4. Authentification : Kerberos
Protocole de **tickets** (port 88), basé sur un **KDC** (sur le DC) et sur des **secrets partagés** (jamais de mot de passe en clair sur le réseau).
```text
1. Client → KDC (AS) : « je suis alice » (authentification) ─► TGT (Ticket Granting Ticket)
2. Client → KDC (TGS) : « je veux accéder à SRV01 », avec le TGT ─► ticket de service
3. Client → SRV01 : présente le ticket de service ─► accès accordé
```
Exigences : **heure synchronisée** (écart max 5 min, voir NTP) ; **SPN** corrects ; DNS fonctionnel. NTLM = mécanisme plus ancien, de repli.

## 5. LDAP (Lightweight Directory Access Protocol)
Protocole **standard** d'accès aux annuaires (AD, OpenLDAP, 389-DS). TCP **389**, **636** (LDAPS, TLS), 3268/3269 (GC).
- **DN** (Distinguished Name) : chemin complet d'un objet.
```text
CN=Jean Dupont,OU=Compta,DC=corp,DC=local
   └ CN : nom commun   OU : unité   DC : composants du domaine
```
- Opérations : `bind` (authentification), `search`, `add`, `modify`, `delete`.
- Exemple de recherche :
```bash
ldapsearch -H ldap://dc01.corp.local -D "alice@corp.local" -W \
  -b "DC=corp,DC=local" "(sAMAccountName=jdupont)" cn mail memberOf
```
Filtres : `(&(objectClass=user)(department=Compta))`.
Usage : authentifier des applications (Linux via SSSD, Wi-Fi 802.1X via RADIUS, VPN, GLPI…).
Sécurité : **LDAPS/StartTLS** obligatoire, compte de service à droits minimaux.

## 6. Réplication
Multi-maître : **chaque DC accepte les modifications** et les réplique aux autres (topologie calculée par le **KCC**, intra-site rapide, inter-site planifiée). Plusieurs DC = **redondance** ; **minimum 2 DC** par domaine.
**Rôles FSMO** (5) : schéma, nommage de domaines (forêt) ; PDC émulateur, RID, infrastructure (domaine). Vérifier : `netdom query fsmo`, `repadmin /replsummary`, `dcdiag`.

## 7. Jonction et GPO
Un poste **rejoint le domaine** (compte machine créé, DNS du domaine configuré). Les **GPO** ([24 - Windows Server](24%20-%20Windows%20Server.md)) s'appliquent ensuite aux OU.

## 8. Sécurité et bonnes pratiques
Comptes admin dédiés (niveaux / tiering), moindre privilège, MFA, mots de passe robustes, sauvegarde de l'AD (**System State**), DC patchés, désactiver NTLMv1/SMBv1, surveiller les groupes privilégiés (Domain Admins) et les journaux d'événements (4624/4625 connexions, 4740 verrouillage).

## 📌 À retenir
- AD = annuaire + Kerberos + DNS + GPO. Forêt > arbre > domaine > OU.
- LDAP = protocole d'accès aux annuaires ; DN = chemin complet.
- Kerberos : TGT puis ticket de service ; temps et DNS critiques.
- Au moins **2 DC** pour la disponibilité.

## 🧪 Exercices
1. Que se passe-t-il si l'écart d'horloge dépasse 5 min ?
2. Pourquoi les postes doivent-ils utiliser le DNS d'AD ?
3. Écris le DN de l'utilisatrice Awa Diop dans l'OU RH du domaine ecole.local.
<details><summary>Réponses</summary>1. L'authentification Kerberos échoue. 2. Pour trouver les DC via les enregistrements SRV. 3. `CN=Awa Diop,OU=RH,DC=ecole,DC=local`.</details>

## 🔗 Liens
[16 - DNS](16%20-%20DNS.md) · [[22 - Sécurité réseau]] · [21 - Wi-Fi](21%20-%20Wi-Fi.md) · [00 - Index](00%20-%20Index.md)
