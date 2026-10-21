---
titre: Windows Server
formateur: Elimane NDIAYE
niveau: 1re-2e année
tags: [cours, windows-server, gpo, powershell, smb]
---

# 24 — Windows Server

Précédent : [23 - Linux pour l'infrastructure](23%20-%20Linux%20pour%20l%27infrastructure.md) · Suivant : [25 - Active Directory et LDAP](25%20-%20Active%20Directory%20et%20LDAP.md)

## 1. Présentation
**Windows Server** est le système d'exploitation serveur de Microsoft (2019, 2022, 2025). Éditions : Standard, Datacenter. Modes d'installation : **avec interface graphique (Desktop Experience)** ou **Server Core** (sans GUI, plus léger, plus sûr, administré en PowerShell/à distance).

## 2. Rôles et fonctionnalités
- **Rôle** : service fourni par le serveur (AD DS, DNS, DHCP, serveur de fichiers, IIS, Hyper-V…).
- **Fonctionnalité** : capacité complémentaire (Sauvegarde, Clustering, BitLocker…).
- Outils : **Gestionnaire de serveur**, **Windows Admin Center**, **PowerShell**.
```powershell
Get-WindowsFeature
Install-WindowsFeature -Name DNS,DHCP -IncludeManagementTools
```

## 3. Active Directory (aperçu)
Annuaire centralisé : **domaine**, **contrôleur de domaine (DC)**, **utilisateurs, groupes, OU**, **GPO**. Détails dans [25 - Active Directory et LDAP](25%20-%20Active%20Directory%20et%20LDAP.md).

## 4. Utilisateurs, groupes, OU, GPO
- **Utilisateurs et groupes** : outil « Utilisateurs et ordinateurs Active Directory » (ADUC).
- **OU** (unité d'organisation) : conteneur logique (par service/site) auquel on **délègue** des droits et on **applique des GPO**.
- **GPO** (Group Policy Object) : stratégies appliquées **automatiquement** aux utilisateurs/ordinateurs : mots de passe, fond d'écran, lecteurs réseau, restrictions, scripts, logiciels, pare-feu.
- Ordre d'application : **Local → Site → Domaine → OU** (**LSDOU**), la plus proche l'emporte.
```powershell
gpupdate /force
gpresult /r
```

## 5. DNS Windows
Rôle DNS intégré à AD : zones **intégrées à AD** (répliquées), **enregistrements SRV** pour localiser les DC. Les clients du domaine doivent **utiliser le DNS du domaine**. Console `dnsmgmt.msc`, cmdlets `Add-DnsServerResourceRecordA`.

## 6. DHCP Windows
Rôle DHCP : **étendues** (scopes), exclusions, réservations, options (003 routeur, 006 DNS), **autorisation dans AD** pour éviter les serveurs pirates, **basculement** (failover) pour la haute disponibilité. ([17 - DHCP](17%20-%20DHCP.md))
```powershell
Add-DhcpServerv4Scope -Name "LAN" -StartRange 192.168.1.100 -EndRange 192.168.1.200 -SubnetMask 255.255.255.0
```

## 7. SMB : partages de fichiers
- Protocole **SMB** (TCP 445). **Deux niveaux de droits** : permissions de **partage** + permissions **NTFS** ; le résultat est le **plus restrictif**.
- Bonne pratique : partage **Tout le monde: Contrôle total** ou Utilisateurs authentifiés, et gérer finement en **NTFS** via des **groupes** (AGDLP).
```powershell
New-SmbShare -Name Compta -Path D:\Compta -FullAccess "CORP\GG-Compta"
net use Z: \\SRV01\Compta
```

## 8. PowerShell
Shell orienté **objets**, syntaxe `Verbe-Nom`.
```powershell
Get-Help Get-Service
Get-Service | Where-Object Status -eq Running
Get-NetIPAddress ; Get-NetRoute ; Test-NetConnection srv01 -Port 445
New-ADUser -Name "Jean Dupont" -SamAccountName jdupont -Enabled $true `
  -AccountPassword (Read-Host -AsSecureString) -Path "OU=Compta,DC=corp,DC=local"
Get-ADUser -Filter * | Export-Csv users.csv
Add-ADGroupMember -Identity GG-Compta -Members jdupont
```
Aide : `Get-Command`, `Get-Member`. Scripts `.ps1` (attention à la stratégie d'exécution).

## 9. Administration distante
| Méthode | Port | Usage |
|---|---|---|
| **RDP** | 3389 | bureau à distance |
| **WinRM / PowerShell Remoting** | 5985 (HTTP) / 5986 (HTTPS) | `Enter-PSSession -ComputerName SRV01`, `Invoke-Command` |
| **RSAT** | — | outils d'administration sur un poste Windows |
| **Windows Admin Center** | 443 | console web |
| **SSH** | 22 | OpenSSH natif |

## 📌 À retenir
- Rôles = services ; Server Core = sans GUI.
- GPO : LSDOU ; SMB = partage + NTFS (le plus restrictif).
- PowerShell et administration distante (WinRM, RDP).

## 🧪 Exercices
1. Différence entre rôle et fonctionnalité ?
2. Un utilisateur a « Lecture » sur le partage et « Modification » en NTFS. Quel droit effectif ?
<details><summary>Réponses</summary>1. Rôle = service principal ; fonctionnalité = capacité complémentaire. 2. Lecture (le plus restrictif).</details>

## 🔗 Liens
[25 - Active Directory et LDAP](25%20-%20Active%20Directory%20et%20LDAP.md) · [16 - DNS](16%20-%20DNS.md) · [17 - DHCP](17%20-%20DHCP.md) · [00 - Index](00%20-%20Index.md)
