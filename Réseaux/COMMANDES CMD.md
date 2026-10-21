
---
	tags: [osint, windows, cmd, important] aliases: [CMD Windows, Commandes Windows, Invite de commandes, Commandes CMD Windows]
# 🖥️ Commandes CMD Windows

> [!info] Définition L'**invite de commandes (CMD)** est l'interpréteur de commandes natif de Windows. Elle permet de gérer le système, le réseau, les fichiers et les processus sans passer par l'interface graphique. En OSINT et en investigation réseau, elle sert surtout à récupérer des informations système et réseau (adresse IP, résolution DNS, routes, connexions actives...).

> [!tip] Ouvrir CMD `Win + R` → taper `cmd` → Entrée Pour l'exécuter en administrateur : `Win + X` → _Terminal (Admin)_ ou clic droit → _Exécuter en tant qu'administrateur_

---

## 🌐 Réseau

|Commande|Rôle|Exemple|
|---|---|---|
|`ipconfig`|Affiche la configuration IP locale|`ipconfig /all`|
|`ipconfig /release`|Libère l'adresse IP actuelle|—|
|`ipconfig /renew`|Renouvelle l'adresse IP|—|
|`ipconfig /flushdns`|Vide le cache DNS local|—|
|`ping`|Teste la connectivité vers une machine/domaine|`ping exemple.com`|
|`tracert`|Trace le chemin réseau vers une cible|`tracert exemple.com`|
|`nslookup`|Résout un nom de domaine en IP (et inversement)|`nslookup exemple.com`|
|`netstat`|Liste les connexions et ports actifs|`netstat -ano`|
|`arp -a`|Affiche la table ARP (IP ↔ MAC du réseau local)|—|
|`route print`|Affiche la table de routage|—|
|`getmac`|Affiche l'adresse MAC de la machine|—|
|`netsh wlan show profiles`|Liste les réseaux Wi-Fi enregistrés|—|
|`netsh wlan show profile "SSID" key=clear`|Affiche le mot de passe d'un Wi-Fi enregistré|—|

---

## 👤 Système et utilisateurs

|Commande|Rôle|Exemple|
|---|---|---|
|`systeminfo`|Infos complètes sur le système|—|
|`whoami`|Affiche l'utilisateur courant|`whoami /all`|
|`hostname`|Affiche le nom de la machine|—|
|`net user`|Liste les comptes utilisateurs locaux|`net user`|
|`net user <nom>`|Détails d'un compte utilisateur|`net user admin`|
|`net localgroup administrators`|Liste les membres du groupe admin|—|
|`tasklist`|Liste les processus en cours|`tasklist /v`|
|`taskkill`|Arrête un processus|`taskkill /PID 1234 /F`|
|`driverquery`|Liste les pilotes installés|—|

---

## 📁 Fichiers et répertoires

|Commande|Rôle|Exemple|
|---|---|---|
|`dir`|Liste le contenu d'un dossier|`dir /a` (fichiers cachés)|
|`cd`|Change de dossier|`cd C:\Users`|
|`tree`|Affiche l'arborescence d'un dossier|`tree /f`|
|`copy`|Copie un fichier|`copy a.txt b.txt`|
|`move`|Déplace/renomme un fichier|`move a.txt C:\Backup`|
|`del`|Supprime un fichier|`del fichier.txt`|
|`attrib`|Affiche/modifie les attributs d'un fichier|`attrib +h fichier.txt` (le cache)|
|`find`|Recherche du texte dans un fichier|`find "mot" fichier.txt`|
|`where`|Localise un exécutable dans le PATH|`where python`|

---

## 🔎 Investigation / OSINT

|Commande|Rôle|Exemple|
|---|---|---|
|`nslookup -type=mx`|Récupère les serveurs mail d'un domaine|`nslookup -type=mx exemple.com`|
|`nslookup -type=txt`|Récupère les enregistrements TXT (SPF, vérifications)|`nslookup -type=txt exemple.com`|
|`netstat -ano \| find "ESTABLISHED"`|Filtre les connexions actives établies|—|
|`net view`|Liste les machines visibles sur le réseau local|—|
|`net share`|Liste les partages réseau actifs|—|
|`wmic useraccount get name,sid`|Liste les comptes et leurs SID|—|

---

## ⚙️ Astuces utiles

- `cls` → efface l'écran
- `exit` → ferme l'invite de commandes
- `help` → liste toutes les commandes disponibles
- `<commande> /?` → affiche l'aide détaillée d'une commande (ex : `ping /?`)
- `>` redirige la sortie vers un fichier : `ipconfig /all > infos.txt`
- `|` enchaîne deux commandes (pipe) : `tasklist | find "chrome"`

---

## ✅ Bonnes pratiques

- [ ] Toujours exécuter en administrateur les commandes qui modifient le système ou le réseau
- [ ] Ne jamais lancer une commande dont on ne comprend pas l'effet sur une machine qui n'est pas la sienne
- [ ] Documenter les commandes utilisées lors d'une investigation (traçabilité)

---

## 🔗 Liens

- [PRT1 OSINT](../OSINT/Cours/PRT1%20OSINT.md) — Cours OSINT, Partie 1
- #osint #windows #cmd