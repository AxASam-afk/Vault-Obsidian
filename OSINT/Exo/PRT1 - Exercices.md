
---
	 tags: [osint, cours, exercices] aliases: [Exercices OSINT PRT1]

# 🧪 OSINT — Exercices (PRT1)

> [!info] Contexte Série d'exercices pratiques appliquant les méthodes vues dans [PRT1 OSINT](../Cours/PRT1%20OSINT.md) (recherche d'entreprise, reverse image search, recherche de pseudo, WHOIS/RDAP, [DORK GOOGLE](../DORK%20GOOGLE.md)).

---

## 1️⃣ Retrouver le SIRET — ESGI Lille

### Méthodologie

1. Rechercher sur Google : `école ESGI Lille`
2. Relever l'adresse : **65 Rue Philippe-Laurent Roland, 59000 Lille**
3. Aller sur **Pappers** et coller l'adresse **sans la ville** (juste la rue et le code postal)
4. Un premier résultat apparaît : l'entreprise **Maetris**, mais son adresse ne correspond pas exactement ![[Pasted image 20260915104727.png]]
5. En descendant dans les résultats, on trouve l'entreprise correspondant réellement à la bonne adresse 
6. ![[Pasted image 20260915104818.png]]

### Résultat

> [!success] SIRET trouvé **423 806 355 00255**

---

## 2️⃣ Reverse Image Search — Retrouver le nom d'un lac

**Image de départ :** [photo_exercice_2.png](../../IMAGES/photo_exercice_2.png)

### Méthodologie

1. Passer l'image dans **Yandex Images** (moteur le plus performant pour les paysages/bâtiments)
2. Yandex propose des images visuellement similaires grâce à sa reconnaissance de motifs (relief, végétation, couleur de l'eau…)
3. Comparer les résultats similaires pour identifier une source fiable mentionnant le lieu
4. Remonter à la page source pour confirmer le nom et le pays

### Résultat

> [!success] Lieu identifié **Lac Goygol**, **Azerbaïdjan** Source : https://eastroute.com/fr/azerbaidjan/lake-goygol

---

## 3️⃣ Recherche d'informations sur un pseudonyme

**Objectif :** vérifier la disponibilité/présence d'un pseudo sur au moins **5 plateformes**.

### Sites utilisés

- https://whatsmyname.me/
- https://instantusername.com/

### Méthodologie

1. Saisir le pseudonyme cible dans l'outil (WhatsMyName ou InstantUsername)
2. Relever toutes les plateformes où le pseudo est **pris** (compte existant probable)
3. Vérifier manuellement chaque plateforme pertinente (photo de profil, bio, activité) pour confirmer qu'il s'agit bien de la même personne
4. Noter les **variations du pseudo** (majuscules, chiffres, underscores) et relancer la recherche si besoin
5. Croiser les informations trouvées entre les différentes plateformes pour construire un profil cohérent

---

## 4️⃣ Retrouver l'IP du domaine ESGI.fr

### Outils utilisés

- https://who.is/whois/esgi.fr
- https://client.rdap.org/?type=domain&object=Esgi.fr&follow-referral=1
- https://www.nslookup.io/domains/esgi.fr/dns-records/

### Méthodologie

1. Interroger le domaine `esgi.fr` via WHOIS et RDAP
2. Confirmer l'adresse IP trouvée en la recherchant à son tour sur WHOIS-IP
3. Comparer les résultats entre les deux méthodes pour valider la cohérence des données ![[Pasted image 20260915121502.png]]

### Résultat

> [!success] IP trouvée **20.74.67.225** → [voir sur who.is](https://who.is/whois-ip/ip-address/20.74.67.225)

> [!note] Hébergement L'adresse IP est hébergée chez **Azure**, donc par **Microsoft**.

---

## 5️⃣ Identifier un lieu à partir d'un indice visuel

Trouver le nom de la ville américaine qui est devant cette photo 
![[photo_exercice_6.jpg|161]]
En fesanr un reverse image search avec google qui me montre qu'il ne s'agit pas réelement de la tours eiffel mais bien d'un replique ce situant a Slobovia en roumanie,
![[Pasted image 20260916100348.png|291]]
ensuite il suffit de recherche sur Maps Slobovia, on voit rapidement que c'est un petit village et que la tour s'appelle simplement Tour Eiffel Replica, et on vois ensuite que le batiment s'agit de Dallas 
![[Pasted image 20260916100423.png|312]]
donnc le nom de la ville Américaine a trouve étais dallas 

---

## 🔗 Liens

- [PRT1 OSINT](../Cours/PRT1%20OSINT.md)
- [DORK GOOGLE](../DORK%20GOOGLE.md)
- #osint #osint/prt1 #exercices

