
---
	 tags: [osint, cours] aliases: [OSINT PRT1, Partie 1 OSINT]

# 🕵️ OSINT — Partie 1

> [!info] Définition **OSINT** = **O**pen **S**ource **INT**elligence (Renseignement de sources ouvertes). Il s'agit de collecter et d'analyser des informations **publiquement accessibles** (sites web, réseaux sociaux, registres officiels, moteurs de recherche…) pour en tirer du renseignement exploitable.

## 🧭 Les 3 piliers de l'OSINT

1. **Sources ouvertes** — l'information doit être publique et légalement accessible
2. **Collecte et analyse rigoureuse** — méthode structurée, vérification, recoupement
3. **Objectif stratégique** — la collecte sert toujours un but précis (enquête, veille, sécurité…)

---

## 🏢 Trouver des informations sur une entreprise

### Sources officielles

- [**INPI**](https://www.inpi.fr/) 
- [**INSEE**](https://www.insee.fr/fr/accueil) 
- [**Pappers** ](https://www.pappers.fr/)

> [!tip] Ces sources permettent notamment de retrouver le **SIRET** d'une entreprise.

### SIREN vs SIRET

- **SIREN** : identifiant unique de l'**entreprise** (9 chiffres)
- **SIRET** : identifiant unique de chaque **établissement** de l'entreprise (SIREN + 5 chiffres). Une entreprise a un seul SIREN mais peut avoir plusieurs SIRET (un par établissement/adresse).

### Méthodologie

- [ ] Privilégier les **sources officielles** (INPI, INSEE, Pappers…)
- [ ] **Croiser au moins 2 sources** d'information avant de valider une donnée
- [ ] **Confirmer l'adresse** via un outil cartographique (Google Maps, etc.)
- [ ] **Identifier le siège social**
- [ ] **Évaluer la fiabilité** de chaque source
- [ ] **Documenter ses sources** (traçabilité de la recherche)

---

## 🔎 Google Dorks

> [!note] Voir la note dédiée : [DORK GOOGLE](../DORK%20GOOGLE.md) _(Définition et liste complète des opérateurs et dorks utiles regroupés dans une note séparée.)_

---

## 🎭 Social Engineering (Ingénierie sociale)

> [!question] Définition à compléter L'ingénierie sociale consiste à manipuler des personnes pour obtenir des informations confidentielles ou un accès, en exploitant la confiance, l'erreur humaine ou la persuasion plutôt qu'une faille technique.

---

## 🖼️ Reverse Image Search (recherche d'image inversée)

Permet de retrouver l'origine, le contexte ou des occurrences similaires d'une image.

### Moteurs spécialisés

|Moteur|Spécialité|
|---|---|
|**Google Images**|Recherche générale, bon pour les objets|
|**Yandex**|Très performant pour les **bâtiments** et la reconnaissance de lieux|
|**Bing Visual Search**|Bon pour les **détails architecturaux**|
|**TinEye**|Spécialisé dans la recherche d'occurrences exactes d'une image sur le web|

---

## 👤 Remonter la piste d'un pseudonyme (username OSINT)

### Outils

- **WhatsMyName** — vérifie la présence d'un pseudo sur de nombreuses plateformes
- _(autres sites à proposer/compléter : Sherlock, Namechk, Maigret…)_

### Méthode

- [ ] Identifier les **variations du pseudonyme** (fautes, chiffres, séparateurs…)
- [ ] **Prioriser les plateformes pertinentes** selon le contexte de l'enquête
- [ ] **Scorer les informations** trouvées (fiabilité, pertinence)
- [ ] **Corréler les informations** entre plateformes
- [ ] Construire un **schéma temporel d'activité** (dates de création, dernières activités…)
- [ ] Analyser le **style d'écriture** et le **vocabulaire** utilisés (permet parfois de relier plusieurs comptes à une même personne)

---

## 🌐 WHOIS & RDAP

> [!question] Firewall (à définir) Un firewall (pare-feu) est un système qui filtre le trafic réseau entrant et sortant selon des règles de sécurité définies, afin de bloquer les connexions non autorisées.

### Traduire un nom de domaine en adresse IP

```
nslookup exemple.com
```

### WHOIS vs RDAP

- **WHOIS** : protocole historique d'interrogation des bases de données d'enregistrement de noms de domaine/IP. Format de réponse non standardisé selon les registres.
- **RDAP** (_Registration Data Access Protocol_) : protocole plus récent qui remplace progressivement WHOIS, avec des réponses **structurées (JSON)**, standardisées et une meilleure gestion des permissions d'accès aux données.

### Types d'enregistrements DNS

|Type|Rôle|
|---|---|
|**A**|Traduit un nom de domaine en adresse **IPv4** _(à compléter)_|
|**AAAA**|Traduit un nom de domaine en adresse **IPv6**|
|**NS**|Indique le **Name Server** (serveur de noms) faisant autorité sur le domaine|

---

## 🛰️ Shodan

> [!question] Définition à compléter Shodan est un moteur de recherche qui indexe les **objets et services connectés à Internet** (serveurs, caméras, IoT, routeurs…) plutôt que des pages web, en scannant les ports ouverts et bannières de services.

🔗 https://www.shodan.io/

### Exemple — IP de l'ESGI (exercice 4)

Résultat obtenu en interrogeant Shodan avec l'IP de l'ESGI vue en exercice :

![[Pasted image 20260915123636.png]]

🔗 https://www.shodan.io/host/20.74.67.225

---

## ☁️ Définir le Cloud

|Fournisseur|Éditeur|
|---|---|
|**AWS**|Amazon|
|**Azure**|Microsoft|
|**GCP**|Google|

---

## 🔌 Port web

> [!note] Voir la note dédiée : [Port web](../../R%C3%A9seaux/Port%20web.md) _(Définition et liste complète des ports utiles regroupés dans une note séparée.)

---

## 🔗 Liens

- [DORK GOOGLE](../DORK%20GOOGLE.md)
- [PRT1 - Exercices](../Exo/PRT1%20-%20Exercices.md)
- #osint #osint/prt1