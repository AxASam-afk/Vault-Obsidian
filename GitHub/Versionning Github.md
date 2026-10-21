## tags: [cours, git, github, gitlab, versionning] aliases: [Git, GIT, Cours Git]

# 🌳 GIT

> [!info] Au programme
> 
> - Un peu de théorie
> - Les commandes de base pour versionner du code
> - Les branches et comment isoler différents développements
> - GIT en local vs GIT distant
> - Utiliser GitHub et GitLab pour travailler en équipe

---

## Pourquoi utiliser Git ?

> [!important] À quoi sert Git, précisément ? **Git** est un logiciel de **contrôle de version distribué** : il enregistre dans le temps l'historique complet des modifications apportées à un ensemble de fichiers (le code d'un projet), et permet de revenir en arrière, de comparer des versions, et de faire travailler plusieurs personnes sur le même code sans qu'elles s'écrasent mutuellement le travail.

### Objectifs

- **Garder un historique propre** de toutes les modifications : le travail est organisé sous forme de **commits documentés**
- **Travailler à plusieurs sans se marcher dessus** : indispensable pour les projets en équipe (chacun peut modifier le code en parallèle, puis les modifications sont fusionnées de façon maîtrisée)

---

## Un peu de théorie

### Le Git repository

> [!important] Définition Un **repository** (dépôt) Git est l'espace où Git stocke tout l'historique des versions d'un projet : fichiers, commits, branches, métadonnées. Chaque repository est **autonome** — par exemple, un repository local existe sur chaque poste de développement, indépendamment des autres.

- Chaque **repository Git est autonome** (un sur chaque PC par exemple)
- Git échange des données entre repositories par **synchronisation** (ce n'est **pas automatique** — il faut explicitement envoyer/récupérer les changements)

### Les commits

> [!important] Définition Un **commit** est un ensemble de modifications apportées à un ou plusieurs fichiers du code. Chaque commit possède un identifiant unique : le **SHA-1** (empreinte de hachage calculée à partir de son contenu).

Concrètement, un commit contient :

- Un **index** qui liste tous les fichiers ajoutés, modifiés ou supprimés
- Des **métadonnées** : un titre (_commit message_), un hash (SHA-1), un auteur, une date de création…

> [!note] Historique des changements Les commits sont **liés les uns aux autres** (chaque commit référence son commit parent) et forment ainsi un **historique** de tous les changements du projet — semblable à une chaîne : `Initial commit → 1er changement → 2ème changement → …`

### Les espaces dans Git

Chaque modification passe par **3 zones** avant d'être définitivement versionnée : les modifications y sont en réalité "sauvegardées" 3 fois, à 3 étapes différentes.

|Espace|Rôle|
|---|---|
|**Working directory** (zone de travail)|C'est le dossier où se trouvent tous vos fichiers, tels que vous les éditez au quotidien|
|**Staging area** (zone d'index)|Zone intermédiaire qui permet de sélectionner précisément les modifications que l'on souhaite inclure dans le prochain commit|
|**Repository**|Le code **commité** (versionné) dans le dépôt Git local — l'historique définitif|

---

## Installation de Git

|Système|Méthode|
|---|---|
|**Windows / macOS**|https://git-scm.com/downloads|
|**Ubuntu / Debian**|`sudo apt install git gitk`|
|**CentOS / RedHat**|`sudo yum install git gitk`|

### Configuration minimale

```
git config --global user.name "Prénom Nom"
git config --global user.email "email@exemple.com"
```

---

## Les commandes de base

### Créer un repository Git

```
mkdir mon_code && cd mon_code
pwd
git init
```

→ `Initialized empty Git repository in .git/`

### Du fichier au commit — étape par étape

1. Créer/éditer un fichier (`nano mon_fichier.txt`)
2. `git status` → le fichier apparaît en **untracked**
3. `git add mon_fichier.txt` → le fichier passe dans la **staging area**
4. `git commit` (avec un message de commit) → les changements sont **versionnés** dans le repository
5. `git status` → _"nothing to commit, working tree clean"_

### Visualiser l'historique

- `git log` — affiche l'historique des commits en ligne de commande
- `gitk` — interface graphique livrée avec Git pour visualiser l'arbre de commits, les branches et les différences

### Tableau récapitulatif — commandes terminal

|Commande|Zone concernée|Rôle|
|---|---|---|
|`git init`|—|Initialise un nouveau repository Git|
|`git status`|Toutes|Affiche l'état des fichiers (modifiés, indexés, non suivis…)|
|`git diff`|Working directory ↔ Index|Affiche les différences non indexées|
|`git diff --staged`|Index ↔ Repository|Affiche les différences déjà indexées mais pas encore commitées|
|`git add <fichier>`|Working directory → Staging area|Ajoute un fichier modifié à l'index|
|`git add -A`|Working directory → Staging area|Ajoute **tous** les fichiers modifiés à l'index|
|`git commit`|Staging area → Repository|Versionne les modifications indexées (avec un message de commit)|
|`git restore <fichier>`|Working directory|Annule les modifications non indexées d'un fichier (remplace l'ancien `git checkout -- <fichier>`)|
|`git restore --staged <fichier>`|Staging area|Désindexe un fichier (remplace l'ancien `git reset HEAD <fichier>`)|
|`git log`|Repository|Affiche l'historique des commits|
|`gitk`|Repository|Interface graphique de visualisation de l'historique|

---

## Les branches

> [!important] Définition Une **branche** est un **pointeur vers un commit**. Elle permet de développer une fonctionnalité en isolation du reste du code, sans impacter les autres branches tant qu'on ne les fusionne pas.

- **Branche principale** : `master` (ou `main` selon les conventions récentes)
- **Branche courante** : désignée par le pointeur spécial **HEAD**
- **Bonne pratique** : en général, **une branche par fonctionnalité** en cours de développement

### Gestion des branches

|Commande|Rôle|
|---|---|
|`git branch`|Affiche la liste des branches|
|`git branch dev`|Crée la branche `dev`|
|`git checkout dev`|Déplace HEAD vers la branche `dev`|
|`git checkout -b ma_branche <branche>`|Crée une nouvelle branche à partir de `<branche>` et déplace HEAD dessus|

### git reset — les 3 méthodes

`git reset` déplace une branche (et son pointeur HEAD) vers un autre commit, mais l'effet sur la staging area et le working directory diffère selon l'option utilisée :

|Méthode|Repository (HEAD/branche)|Staging area (index)|Working directory|
|---|---|---|---|
|`git reset --soft <sha1>`|Déplacé vers le commit cible|**Conservée** (les changements des commits annulés restent indexés)|**Inchangé**|
|`git reset --mixed <sha1>` _(défaut)_|Déplacé vers le commit cible|**Réinitialisée** (les changements des commits annulés repassent en modifications non indexées)|**Inchangé**|
|`git reset --hard <sha1>`|Déplacé vers le commit cible|**Réinitialisée**|**Réinitialisé** (⚠️ perte définitive des modifications non commitées)|

---

## Manipulation de branches

### Merge de branches

> [!important] Définition Le **merge** intègre les modifications d'une branche dans la branche courante.

Exemple : fusionner le code de `dev` dans la branche "officielle" `master` :

```
git checkout master
git merge dev
```

### Gestion des conflits (lors d'un merge)

Lors d'un merge, certaines modifications peuvent entrer en conflit et Git ne sait pas toujours les résoudre automatiquement — une **intervention manuelle** est alors nécessaire.

```
git merge dev
# Auto-merging fichier.txt
# CONFLICT (content): Merge conflict in fichier.txt
```

Le fichier en conflit contient des marqueurs à résoudre à la main :

```
<<<<<<< HEAD
    /* Lignes originales */
=======
    /* lignes à appliquer en conflit */
>>>>>>> ma_branche
```

Résolution :

```
nano fichier.txt   # corriger manuellement
git add fichier.txt
git commit
```

### Rebase de branches

> [!important] Définition Le **rebase** permet de repositionner les commits d'une branche par-dessus les derniers commits d'une autre branche (ex : replacer `dev` au-dessus de `master`), pour garder un historique linéaire.

```
git checkout dev
git rebase master
```

> [!warning] Conflits lors d'un rebase Lors d'un rebase, Git essaie d'appliquer les commits **un par un** — un conflit peut donc apparaître sur chacun d'eux, l'un après l'autre :
> 
> ```
> git rebase master
> # conflit sur le 1er commit
> nano fichier.txt
> git add fichier.txt
> git rebase --continue
> # conflit sur le 2e commit...
> ```

### Cherry-pick de commit

Permet d'appliquer un commit précis d'une branche sur une autre, sans fusionner toute la branche :

```
git checkout master
git cherry-pick <sha1>
```

### Autres commandes utiles

- **`.gitignore`** — fichier qui liste les fichiers/dossiers à exclure volontairement du suivi Git (fichiers temporaires, secrets, dépendances…)
- **`git revert`** — annule les changements d'un commit en créant un **nouveau commit inverse**, sans réécrire l'historique (contrairement à `reset`)

---

## Les dépôts distants

> [!important] Objectif Centraliser les données d'un projet sur un **dépôt Git distant**, hébergé sur un serveur, pour permettre le travail collaboratif.

- Un dépôt distant joue le rôle de **serveur** central
- Chaque développeur possède une **copie locale** du repository (un **clone**)
- Les différents repositories **synchronisent leurs commits** entre eux
- La synchronisation se fait via des **commandes explicites**, elle n'est **pas automatique**

Dépôts distants les plus connus : **GitHub** et **GitLab**.

### Tableau récapitulatif — commandes dépôt distant (GitHub/GitLab)

|Commande|Rôle|
|---|---|
|`git clone <url>`|Récupère une copie complète d'un repository distant (protocoles possibles : GIT natif, HTTP/HTTPS, SSH)|
|`git remote`|Liste les remotes configurés (ex : `origin`, créé automatiquement lors d'un clone)|
|`git remote -v`|Affiche l'URL de chaque remote (fetch et push)|
|`git branch -a`|Liste toutes les branches, locales et distantes (préfixées `remotes/origin/…`)|
|`git fetch`|Récupère les commits du dépôt distant **sans** modifier les branches locales|
|`git pull`|Équivaut à `git fetch` + `git merge` : récupère et fusionne directement dans la branche courante|
|`git pull -r`|Équivaut à `git fetch` + `git rebase`|
|`git push`|Envoie les commits locaux vers le dépôt distant (uniquement les commits absents du serveur ; n'efface jamais les commits distants en plus)|

### Schéma de synthèse

```
working directory → (git add) → staging area (index) → (git commit) → git repository (local) → (git push / git pull / git fetch) → remote git repository (GitHub / GitLab)
```

---

## GitHub et GitLab — travailler en équipe

- **GitHub** et **GitLab** sont des plateformes d'hébergement de dépôts Git distants, pensées pour le travail collaboratif
- Elles ajoutent à Git : gestion des accès/droits par équipe, pull/merge requests (revue de code avant fusion), issues (suivi de tâches/bugs), intégration continue, wikis
- Voir aussi : [Histoire de Github](Histoire%20de%20Github.md)

---

## 🔗 Liens

- [Histoire de Github](Histoire%20de%20Github.md)
- #git #github #gitlab