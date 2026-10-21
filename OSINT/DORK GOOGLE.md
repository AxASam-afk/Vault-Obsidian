## title: Dork Google tags: [osint, google-dorking, important] aliases: [Google Dorking, Google Hacking]

# 🔍 Dork Google

> [!info] Définition Le **Google Dorking** (ou _Google Hacking_) consiste à utiliser des **opérateurs de recherche avancés** sur Google pour affiner ou détourner l'usage classique du moteur de recherche, afin de trouver des informations précises, souvent non destinées à être facilement trouvées (fichiers, pages de connexion, données exposées, etc.).

> [!warning] Cadre légal et éthique Le Google Dorking en lui-même est **légal** (on utilise uniquement des fonctionnalités publiques de Google). En revanche, **accéder, exploiter ou divulguer** des données trouvées grâce à un dork sans autorisation peut être **illégal** (accès frauduleux à un système, atteinte à la vie privée, etc.). À utiliser uniquement dans un cadre autorisé (CTF, pentest sous contrat, veille OSINT légale).

## 📌 Opérateurs de base

|Opérateur|Rôle|Exemple|
|---|---|---|
|`site:`|Limite la recherche à un domaine ou sous-domaine précis|`site:exemple.com`|
|`filetype:` / `ext:`|Cherche un type de fichier précis|`filetype:pdf`|
|`intitle:`|Le mot doit apparaître dans le **titre** de la page|`intitle:"index of"`|
|`allintitle:`|Tous les mots doivent être dans le titre|`allintitle:login admin`|
|`inurl:`|Le mot doit apparaître dans l'**URL**|`inurl:admin`|
|`allinurl:`|Tous les mots doivent être dans l'URL|`allinurl:wp-content uploads`|
|`intext:`|Le mot doit apparaître dans le **corps du texte**|`intext:"mot de passe"`|
|`allintext:`|Tous les mots doivent être dans le texte|`allintext:user password`|
|`cache:`|Affiche la version en cache de Google d'une page|`cache:exemple.com`|
|`related:`|Trouve des sites similaires à un domaine|`related:exemple.com`|
|`link:`|Cherche les pages qui pointent vers une URL (peu fiable aujourd'hui)|`link:exemple.com`|
|`define:`|Donne la définition d'un mot|`define:osint`|
|`numrange:` ou `X..Y`|Cherche dans une plage de nombres|`camera 2020..2023`|

---

## 🧩 Opérateurs de combinaison / syntaxe

|Symbole|Rôle|Exemple|
|---|---|---|
|`" "`|Recherche exacte de l'expression|`"confidentiel interne"`|
|`-`|Exclut un mot des résultats|`site:exemple.com -inurl:blog`|
|`OR` ou `\|`|L'un ou l'autre des termes|`filetype:pdf OR filetype:docx`|
|`AND`|Les deux termes doivent apparaître (souvent implicite)|`login AND admin`|
|`*`|Joker, remplace un mot inconnu|`"mot de passe *"`|
|`( )`|Regroupe des opérateurs pour prioriser|`site:exemple.com (inurl:admin OR inurl:login)`|

---

## 🎯 Dorks par catégorie

### 🔑 Pages de connexion / panneaux d'administration

```
inurl:admin intitle:login
inurl:wp-admin
intitle:"login" inurl:portal
inurl:"/admin/login.php"
```

### 📂 Fichiers et répertoires exposés

```
intitle:"index of /" 
intitle:"index of" "parent directory"
intitle:"index of" backup
filetype:sql "INSERT INTO"
filetype:env "DB_PASSWORD"
filetype:log "error"
```

### 🔐 Identifiants, mots de passe, données sensibles

```
filetype:txt "password"
intext:"password" filetype:log
"password" filetype:xls
inurl:config.php intext:"DB_PASSWORD"
filetype:sql intext:"password"
```

### 📸 Caméras et objets connectés

```
intitle:"webcamXP 5"
inurl:"view/view.shtml"
intitle:"Live View / - AXIS"
inurl:"/control/userimage.html"
```

### 📄 Documents internes / confidentiels

```
filetype:pdf intext:"confidentiel"
filetype:doc intext:"interne uniquement"
intitle:"index of" "confidential"
site:exemple.com filetype:xlsx
```

### 🌐 Sous-domaines et infrastructure

```
site:*.exemple.com -www
site:exemple.com inurl:dev
site:exemple.com inurl:test
site:exemple.com inurl:staging
```

### 🧑 Réseaux sociaux / profils (OSINT personnes)

```
site:linkedin.com "Nom Prénom"
site:pastebin.com "email@exemple.com"
intext:"@exemple.com" filetype:xls
```

### 🖥️ Serveurs et services mal configurés

```
intitle:"phpinfo()"
intitle:"Index of" "apache" "server-status"
inurl:"/server-status"
intitle:"Welcome to nginx!"
```

### 🗃️ Bases de données exposées

```
filetype:sql "phpMyAdmin"
intitle:"phpMyAdmin" "Welcome to phpMyAdmin"
inurl:"pma/index.php"
```

---

## 🛠️ Outils liés au Dorking

- **Google Hacking Database (GHDB)** — base de dorks maintenue par Exploit-DB
- **Dorkbot / DorkSearch** — automatisation de recherches de dorks
- **Shodan / Censys** — équivalents "dorking" pour objets connectés et serveurs (hors Google)

---

## ✅ Bonnes pratiques

- [ ] Toujours combiner `site:` avec un autre opérateur pour rester précis et légal
- [ ] Ne jamais tenter d'exploiter une faille trouvée sans autorisation
- [ ] Documenter ses recherches dans le cadre d'une mission (traçabilité)
- [ ] Utiliser des dorks dans un cadre de veille ou de CTF avant tout

---

## 🔗 Related

- [Index OSINT](Index%20OSINT.md) — Index du dossier OSINT
- [Index principal](../INDEX.md) — Table des matières générale du vault
- [Boîte à outils OSINT](Cours/Boite%20%C3%A0%20outils%20OSINT.md)
- [OSINT — Partie 1](Cours/PRT1%20OSINT.md)
- [Exercices info sur image](Exo/Exercices%20info%20sur%20image.md)
- [OSINT — Exercices (PRT1)](Exo/PRT1%20-%20Exercices.md)
- #osint #google-dorking