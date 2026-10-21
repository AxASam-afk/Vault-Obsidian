
---

## Partie 1 – Création de la topologie

![[Pasted image 20260930122119.png]]

|Équipement|Port PC|Port switch|
|---|---|---|
|PC-A|FastEthernet0|Fa0/1|
|PC-B|FastEthernet0|Fa0/2|
|PC-C|FastEthernet0|Fa0/3|

### Question 1 – Quelle est la topologie physique utilisée ?

- [ ] a. Bus
- [ ] b. Anneau
- [x] c. Étoile
- [ ] d. Maillée

> [!success] Justification: Toutes les machines sont reliées **individuellement** à un équipement central (le switch SW1) par leur propre câble. Le switch est le point central : si un câble est coupé, seule la machine concernée est isolée. C'est la définition d'une topologie en **étoile**.

---

## Partie 2 – Configuration IPv4

|Machine|Adresse IP|Masque|
|---|---|---|
|PC-A|192.168.10.10|255.255.255.0|
|PC-B|192.168.10.20|255.255.255.0|
|PC-C|192.168.10.30|255.255.255.0|

### Question 2 – Quel est le réseau auquel appartiennent les trois machines ?

> [!success] Le réseau **192.168.10.0/24** (masque 255.255.255.0).
> 
> - Adresse réseau : `192.168.10.0`
> - Adresse de broadcast : `192.168.10.255`
> - Plage utilisable : `192.168.10.1` à `192.168.10.254`

### Question 3 – Les trois machines peuvent-elles communiquer directement ? Pourquoi ?

> [!success] **Oui.** Les trois machines ont la même partie réseau (`192.168.10`) grâce au masque /24, donc elles sont dans le **même réseau IP**. Elles sont aussi reliées au même switch (même domaine de broadcast / même VLAN 1 par défaut). Aucun routeur n'est nécessaire : la communication se fait directement au niveau 2 (Ethernet) via le switch.

---

## Partie 3 – Tester la communication

```
ping 192.168.10.20
ping 192.168.10.30
```

### Question 4 – Tableau des résultats

| Test        | Résultat (réussi / échoué, nombre de réponses)                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| ----------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| PC-A → PC-B | Réussi – Pinging 192.168.10.20 with 32 bytes of data: <br><br>Reply from 192.168.10.20: bytes=32 time<1ms TTL=128<br>Reply from 192.168.10.20: bytes=32 time<1ms TTL=128<br>Reply from 192.168.10.20: bytes=32 time<1ms TTL=128<br>Reply from 192.168.10.20: bytes=32 time<1ms TTL=128<br><br>Ping statistics for 192.168.10.20:<br>Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),<br>Approximate round trip times in milli-seconds:<br>Minimum = 0ms, Maximum = 0ms, Average = 0ms |
| PC-A → PC-C | Réussi – Pinging 192.168.10.30 with 32 bytes of data: <br><br>Reply from 192.168.10.20: bytes=32 time<1ms TTL=128<br>Reply from 192.168.10.20: bytes=32 time<1ms TTL=128<br>Reply from 192.168.10.20: bytes=32 time<1ms TTL=128<br>Reply from 192.168.10.20: bytes=32 time<1ms TTL=128<br><br>Ping statistics for 192.168.10.20:<br>Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),<br>Approximate round trip times in milli-seconds:<br>Minimum = 0ms, Maximum = 0ms, Average = 0ms |


### Question 5 – Quelle adresse identifie la machine distante au niveau réseau (couche 3) ?

- [ ] Adresse MAC
- [x] Adresse IP

---

## Partie 4 – Observer les adresses MAC

Commande : `ipconfig /all` → ligne **Physical Address**.

### Question 6 – Tableau IP / MAC

| Machine | Adresse IP    | Adresse MAC    |
| ------- | ------------- | -------------- |
| PC-A    | 192.168.10.10 | 0040.0BAB.C95E |
| PC-B    | 192.168.10.20 | 0006.2A28.4186 |
| PC-C    | 192.168.10.30 | 0001.64D4.0256 |

### Question 7 – Sur combien de bits une adresse MAC est-elle codée ? Que représente la première moitié ?

> [!success] Une adresse MAC est codée sur **48 bits** (6 octets, notés en 12 chiffres hexadécimaux). La **première moitié (24 bits / 3 octets)** est l'**OUI** (_Organizationally Unique Identifier_) : elle identifie le **constructeur** de la carte réseau. La seconde moitié est le numéro propre à l'interface, attribué par le constructeur.

### Question 8 – À quelle couche OSI l'adresse MAC est-elle utilisée ?

> [!success]  À la **couche 2 – Liaison de données** (Data Link).

---

## Partie 5 – Observer le fonctionnement d'ARP

```
arp -a
```

### Question 9 – Contenu de la table ARP de PC-A et pourquoi ?


> [!success] La table ARP contient des **associations adresse IP ↔ adresse MAC** (avec le type, dynamique) :
> 
> - `192.168.10.20` ↔ MAC de PC-B
> - `192.168.10.30` ↔ MAC de PC-C
> 
> Ces entrées sont présentes car PC-A a **pingué** PC-B et PC-C : pour envoyer une trame Ethernet, il a dû découvrir leur adresse MAC via ARP, puis a mémorisé le résultat dans son cache.

```
arp -d
arp -a
Puis ping vers PC-B, et de nouveau arp -a.
```

![[Pasted image 20260930122857.png]]
### Question 10 – Qu'observez-vous ? Rôle du protocole ARP ?

> [!success] Réponse
> 
> - Après `arp -d`, la table est **vide** (« No ARP Entries Found »).
> - Après le ping vers PC-B, une entrée **`192.168.10.20` ↔ MAC de PC-B`** est **réapparue** (seulement celle-ci, pas celle de PC-C).
> 
> **Rôle d'ARP** (_Address Resolution Protocol_) : **résoudre une adresse IP (couche 3) en adresse MAC (couche 2)** sur le réseau local, afin de pouvoir construire la trame Ethernet. Le résultat est mis en cache dans la table ARP.

---

## Partie 6 – La table d'adresses MAC du switch

```
enable
show mac address-table
```

### Question 11 – Table MAC du switch

| VLAN | Adresse MAC    | Type    | Port  |
| ---- | -------------- | ------- | ----- |
| 1    | 0040.0BAB.C95E | DYNAMIC | Fa0/1 |
| 1    | 0006.2A28.4186 | DYNAMIC | Fa0/2 |
| 1    | 0001.64D4.0256 | DYNAMIC | Fa0/3 |

```
clear mac address-table dynamic
show mac address-table
```

![[Pasted image 20260930123553.png]]

Puis ping PC-A → PC-C et nouvel affichage : les MAC de PC-A (Fa0/1) et de PC-C (Fa0/3) réapparaissent.

![[Pasted image 20260930123656.png]]
