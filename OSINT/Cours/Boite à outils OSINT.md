

## tags: [osint, outils, cours] aliases: [Outils OSINT, OSINT Tools, Boite à outils OSINT]


> [!info] À propos Catalogue des outils OSINT vus en cours, classés par domaine d'usage (Personne, Domaine, Infrastructure, Géolocalisation, Multi-usage). Chaque outil est accompagné de son lien officiel, son type (gratuit/payant) et son cas d'usage principal.

> [!warning] Cadre légal et éthique Plusieurs de ces outils (recherche faciale, people search, scraping de réseaux sociaux) touchent à des données personnelles. Leur usage doit toujours respecter le **RGPD**, le consentement et le cadre légal d'une mission autorisée (cours, CTF, pentest sous contrat).

---

## 👤 Personne (recherche sur un individu)

|Outil|Type|Utilité|Cas d'usage|
|---|---|---|---|
|[Epieos](https://epieos.com/)|Freemium|Recherche inversée email/téléphone : révèle profils et traces associées (Google ID, avis, comptes)|À partir d'un email/téléphone, retrouver des identifiants Google, avis, traces associées ; profiler une personne, vérifier l'appartenance d'un compte|
|[WhatsMyName](https://whatsmyname.app/)|Gratuit (OSS)|Énumération de noms d'utilisateur sur des centaines de sites|Tester un handle sur des centaines de services pour confirmer sa présence ou sa disponibilité|
|[Have I Been Pwned](https://haveibeenpwned.com/)|Gratuit|Vérifie si un email/téléphone apparaît dans des fuites de données connues|Évaluer l'exposition d'un compte (hygiène numérique, réponse à incident)|
|[PimEyes](https://pimeyes.com/)|Payant (essai limité)|Recherche d'images par reconnaissance faciale à l'échelle du web|Retrouver où une photo/un visage apparaît en ligne — **attention RGPD/consentement/lois locales**|
|[Lusha](https://www.lusha.com/)|Payant (freemium limité)|Enrichissement B2B : emails/téléphones de décisionnaires|Contacter des décideurs en prospection (vérifier la conformité marketing)|
|[Pipl](https://pipl.com/)|Payant|People search professionnel : agrège identités, emails, téléphones, profils sociaux, adresses|Vérifier/recouper l'identité d'une personne avec un score de confiance (due diligence)|
|[Sherlock](https://github.com/sherlock-project/sherlock)|Gratuit (OSS)|Recherche la présence d'un pseudonyme sur des centaines de plateformes|Relier des identités/pseudonymes entre plusieurs sites|
|[snscrape](https://github.com/JustAnotherArchivist/snscrape)|Gratuit (OSS)|Extraction de posts/profils (X/Twitter, Reddit…) sans API officielle|Analyse thématique, chronologique ou archivage de contenus sociaux|
|[Twint](https://github.com/twintproject/twint)|Gratuit (OSS)|Scraping X/Twitter sans API : tweets, followers, hashtags|Construire des timelines et archiver du contenu volatile (fiabilité variable selon les changements de la plateforme)|

---

## 🏢 Domaine (entreprise, site web, organisation)

|Outil|Type|Utilité|Cas d'usage|
|---|---|---|---|
|[crt.sh](https://crt.sh/)|Gratuit|Recherche de certificats dans les CT logs|Découvrir des sous-domaines via Certificate Transparency, veille sur les nouveaux certificats liés à un domaine|
|[DNSDumpster](https://dnsdumpster.com/)|Gratuit|Énumération DNS (A/MX/TXT/NS) et visualisation des sous-domaines|Cartographier rapidement l'espace DNS d'un domaine|
|[RapidDNS](https://rapiddns.io/)|Gratuit|DNS passif : sous-domaines, historiques, co-hébergements|Pivoter IP ↔ domaine, repérer des actifs oubliés|
|[SecurityTrails](https://securitytrails.com/)|Freemium|Historique DNS/WHOIS, sous-domaines, infrastructure d'un domaine|Repérer du _shadow IT_ et des co-hébergements|
|[Hunter.io](https://hunter.io/)|Freemium|Découvre le format d'emails d'une organisation|Lister/valider des adresses probables pour la prospection ou le contact ciblé|
|[OpenCorporates](https://opencorporates.com/)|Freemium|Plus grande base d'entités légales (registres officiels, API)|Obtenir structures d'entreprise, dirigeants, adresses, filiales|
|[ICIJ Offshore Leaks Database](https://offshoreleaks.icij.org/)|Gratuit|Base liant entités, personnes et juridictions (Panama/Pandora/Paradise/Bahamas Papers)|Identifier structures offshore, intermédiaires, bénéficiaires effectifs|
|[IntelX (Intelligence X)](https://intelx.io/)|Freemium|Moteur/archives web public, dark web, fuites, documents|Pivoter sur emails, domaines, IP, crypto ; archiver des pages sensibles|
|[Google (dorks avancés)](https://www.google.com/)|Gratuit|Recherche avancée via opérateurs (`site:`, `filetype:`, `intitle:`…)|Trouver fichiers exposés, panneaux d'admin, listings — voir [Dork Google](../DORK%20GOOGLE.md)|
|[Papers (Google Scholar / Semantic Scholar)](https://scholar.google.com/)|Gratuit|Recherche d'articles académiques, thèses, citations|Vérifier des expertises, trouver des sources et citations crédibles|
|[Wayback Machine](https://web.archive.org/)|Gratuit|Accès à des versions archivées de sites|Récupérer du contenu supprimé, comparer l'historique d'un site|
|[archive.today](https://archive.today/)|Gratuit|Capture d'un instantané figé d'une page web|Figer une preuve/archive, citer une source volatile|

---

## 🖥️ Infrastructure technique

|Outil|Type|Utilité|Cas d'usage|
|---|---|---|---|
|[DeHashed](https://www.dehashed.com/)|Payant|Recherche/monitoring dans des bases de fuites (emails, usernames, IP, hash), API disponible|Pivot pour réponse à incident et corrélations|
|[theHarvester](https://github.com/laramies/theHarvester)|Gratuit (OSS)|Récupère emails, sous-domaines, hôtes, bannières via sources publiques|Amorcer un footprinting et des pistes de contact|
|[Censys](https://search.censys.io/)|Freemium|Explore certificats TLS, hôtes et services indexés par scans Internet|Suivre l'évolution d'une infrastructure (renouvellements de certificats, nouveaux hôtes)|
|[Shodan](https://www.shodan.io/)|Freemium|Recense services/appareils exposés (bannières, ports, techno)|Cartographier la surface d'attaque, repérer des services mal configurés (RDP, Elasticsearch, caméras), surveiller l'IoT/ICS — voir [[PRT1]]|
|[OnionSearch](https://github.com/megadose/OnionSearch)|Gratuit (OSS)|Scrape des moteurs `.onion` pour découvrir URLs/services cachés (Tor)|Threat intel, suivi d'exfiltration/dumps|
|[Recon-ng](https://github.com/lanmaster53/recon-ng)|Gratuit (OSS)|Framework modulaire de recon web (modules, reporting, intégrations API)|Pipelines de reconnaissance personnalisés en CLI|
|[SpiderFoot](https://www.spiderfoot.net/)|Freemium (OSS + SaaS)|Collecte OSINT automatisée sur domaines/IP/emails/personnes avec corrélations|Scans récurrents avec scoring de risques|

---

## 🌍 Géolocalisation (GEOINT / IMINT)

|Outil|Type|Utilité|Cas d'usage|
|---|---|---|---|
|GeoInt Localisator _(github.com/Th0rr/GeoInt-Localisator)_|Gratuit (OSS)|Calcule rapidement des distances entre repères visibles sur une photo|Resserrer la position probable d'une scène|
|[Google Maps/Earth & Street View](https://maps.google.com/)|Gratuit (API payante)|Vérifie lieux, itinéraires, POI, détails visuels au sol|Valider un lieu exact, reconnaître des repères, mesurer distances/altitudes|
|[SunCalc](https://www.suncalc.org/)|Gratuit|Position du soleil et des ombres par lieu/date|Chronolocalisation : vérifier la cohérence lieu-date d'une image/vidéo|
|[Mapillary](https://www.mapillary.com/)|Gratuit|Imagerie de rue crowdsourcée avec API, sans floutage systématique (contrairement à Google Maps)|Lire signalisation/sens de circulation, chronolocaliser des scènes|
|[Sentinel Hub EO Browser](https://apps.sentinel-hub.com/eo-browser/)|Freemium|Imagerie satellite multi-sources (Sentinel, Landsat) et détection de changements|Suivre l'évolution d'une zone (construction, incendie) dans le temps|
|[ExifTool](https://exiftool.org/)|Gratuit (OSS)|Lit/édite les métadonnées (EXIF/XMP/IPTC) d'images et documents|Extraire GPS, dates, appareil, numéro de série ; vérifier des falsifications|
|InstaLocTrack _(github.com/bernsteining/instaloctrack)_|Gratuit (OSS)|Récupère les lieux géotagués d'un profil Instagram et les cartographie|Analyser routine/déplacements, corroborer des localisations|

---

## 🧩 Multi-usage / Frameworks

|Outil|Type|Utilité|Cas d'usage|
|---|---|---|---|
|[Holehe](https://github.com/megadose/holehe)|Gratuit (OSS)|Teste si un email est associé à des comptes sur de nombreux services|Profilage/présence en ligne de manière non intrusive|
|[OSINT Framework](https://osintframework.com/)|Gratuit|Répertoire structuré de ressources OSINT (arborescence par cas d'usage)|Identifier rapidement l'outil adapté à un besoin donné|
|[Maltego](https://www.maltego.com/)|Freemium|Graph/link analysis : relie personnes, domaines, IP, sociétés|Graphes d'investigation et rapports via des _transforms_|

---

## 🛠️ Outils complémentaires

|Outil|Utilité|
|---|---|
|[timeanddate](https://www.timeanddate.com/date/)|Calcule le nombre de jours entre deux dates, ou une date + un délai (utile pour reconstituer une chronologie)|
|[oanda](https://www.oanda.com/currency-converter/fr/)|Convertit une monnaie vers une autre à une date précise|

---

## 🔗 Related

- [Index OSINT Cours](INDEX.md) — Index du dossier OSINT/Cours
- [Index OSINT](../Index%20OSINT.md) — Index du dossier OSINT
- [Index principal](../../INDEX.md) — Table des matières générale du vault
- [OSINT — Partie 1](PRT1%20OSINT.md)
- [Dork Google](../DORK%20GOOGLE.md)
- [Exercices info sur image](../Exo/Exercices%20info%20sur%20image.md)
- [OSINT — Exercices (PRT1)](../Exo/PRT1%20-%20Exercices.md)
- #osint #outils