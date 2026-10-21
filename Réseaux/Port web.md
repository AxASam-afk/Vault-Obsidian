
---
	 tags: [important, reseau, port, web] aliases: [Port Web, Ports réseau, Ports TCP/UDP]

# 🔌 Port Web

> [!info] Définition Un **port** est un numéro (de **0 à 65535**) qui identifie un **service ou une application** précise sur une machine, en complément de l'adresse IP. Une adresse IP identifie la machine, le port identifie **le service auquel on s'adresse** sur cette machine. Analogie : l'IP = l'adresse d'un immeuble, le port = le numéro d'appartement.

---

## 🧱 TCP vs UDP

|Protocole|Caractéristiques|Usage typique|
|---|---|---|
|**TCP**|Connecté, fiable, avec accusé de réception et contrôle d'erreur|Web (HTTP/HTTPS), email, transfert de fichiers|
|**UDP**|Non connecté, plus rapide, sans garantie de livraison|Streaming, jeux en ligne, DNS, VoIP|

Un même numéro de port peut être utilisé en TCP **et** en UDP pour des services différents (ex : le port 53 en TCP et UDP pour le DNS).

---

## 📊 Catégories de ports

|Plage|Nom|Rôle|
|---|---|---|
|**0 – 1023**|Ports **connus / réservés** (_well-known ports_)|Services standards (HTTP, FTP, SSH…), nécessitent des droits admin pour être ouverts|
|**1024 – 49151**|Ports **enregistrés** (_registered ports_)|Attribués à des applications spécifiques (ex : MySQL 3306)|
|**49152 – 65535**|Ports **dynamiques / privés** (_ephemeral ports_)|Attribués temporairement par le système côté client lors d'une connexion|

---

## 🌍 Ports web les plus importants

|Port|Protocole|Service|
|---|---|---|
|**80**|TCP|**HTTP** — trafic web non chiffré|
|**443**|TCP|**HTTPS** — trafic web chiffré (TLS/SSL)|
|**8080**|TCP|HTTP alternatif (proxy, serveurs de dev, Tomcat…)|
|**8443**|TCP|HTTPS alternatif|

> [!tip] En OSINT/reconnaissance, scanner le **80** et le **443** en premier permet souvent de découvrir rapidement un serveur web actif — et **8080/8443** pour des panneaux d'admin ou services web secondaires.

---

## 📚 Autres ports courants à connaître

|Port|Protocole|Service|
|---|---|---|
|**21**|TCP|FTP (contrôle)|
|**20**|TCP|FTP (données)|
|**22**|TCP|SSH (connexion sécurisée à distance)|
|**23**|TCP|Telnet (non sécurisé)|
|**25**|TCP|SMTP (envoi d'emails)|
|**53**|TCP/UDP|DNS|
|**67 / 68**|UDP|DHCP (attribution automatique d'IP)|
|**110**|TCP|POP3 (réception d'emails)|
|**143**|TCP|IMAP (réception d'emails)|
|**389**|TCP|LDAP|
|**445**|TCP|SMB (partage de fichiers Windows)|
|**3306**|TCP|MySQL|
|**3389**|TCP|RDP (Bureau à distance Windows)|
|**5432**|TCP|PostgreSQL|

---

## 🔎 Voir les ports ouverts d'une machine (OSINT / réseau)

| Méthode                                                                    | Outil                                   |
| -------------------------------------------------------------------------- | --------------------------------------- |
| Scanner localement les connexions actives                                  | `netstat -ano` (voir [COMMANDES CMD](COMMANDES%20CMD.md)) |
| Scanner les ports d'une machine distante                                   | Nmap                                    |
| Trouver les services/ports exposés d'une IP publique sans scanner soi-même | **Shodan** (voir [PRT1 OSINT](../OSINT/Cours/PRT1%20OSINT.md))              |
| Vérifier un port précis rapidement                                         | Sites de "port checker" en ligne        |

---

## ✅ À retenir

- [ ] Le port complète l'IP pour désigner **un service précis** sur une machine
- [ ] **80** = HTTP, **443** = HTTPS → les deux ports web à connaître en premier
- [ ] TCP = fiable/connecté, UDP = rapide/non fiable
- [ ] Les ports **0–1023** sont réservés aux services standards
- [ ] En reconnaissance OSINT, Shodan permet de voir les ports ouverts d'une IP sans avoir à la scanner directement

---

## 🔗 Liens

- [IP](IP.md)
- [PRT1 OSINT](../OSINT/Cours/PRT1%20OSINT.md)
- 
- #important #reseau #port #web