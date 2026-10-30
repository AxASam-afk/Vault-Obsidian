---
title: TP - Calcul d'adressage IPv4
tags: [reseau, ipv4, cidr, sous-reseaux, TP]
cssclasses: []
---

# TP : Calcul d'adressage IPv4

> [!info] Rappels utilisés
> - Poids des bits : `128 64 32 16 8 4 2 1`
> - Masque : 0, 128, 192, 224, 240, 248, 252, 254, 255
> - **Réseau** = IP ET masque (bits hôte à 0)
> - **Broadcast** = bits hôte à 1
> - **Hôtes** = 2ⁿ − 2
> - **Nombre magique** = 256 − octet de masque intéressant = taille d'un bloc

---

## Exercice 1 : Conversions décimal ⇔ binaire

### 1.1 Décimal → binaire (somme des poids)

| Décimal | Somme des poids | Binaire |
|---|---|---|
| 200 | 128 + 64 + 8 | `11001000` |
| 172 | 128 + 32 + 8 + 4 | `10101100` |
| 45 | 32 + 8 + 4 + 1 | `00101101` |
| 99 | 64 + 32 + 2 + 1 | `01100011` |
| 255 | 128 + 64 + 32 + 16 + 8 + 4 + 2 + 1 | `11111111` |
| 129 | 128 + 1 | `10000001` |
| 63 | 32 + 16 + 8 + 4 + 2 + 1 | `00111111` |
| 240 | 128 + 64 + 32 + 16 | `11110000` |

### 1.2 Binaire → décimal

| Binaire | Calcul | Décimal |
|---|---|---|
| `10110000` | 128 + 32 + 16 | **176** |
| `01111111` | 64 + 32 + 16 + 8 + 4 + 2 + 1 | **127** |
| `11100000` | 128 + 64 + 32 | **224** |
| `00010110` | 16 + 4 + 2 | **22** |
| `10000010` | 128 + 2 | **130** |
| `11111100` | 128 + 64 + 32 + 16 + 8 + 4 | **252** |

### 1.3 172.16.45.200 en binaire

```
172      16       45       200
10101100.00010000.00101101.11001000
```

### 1.4 Adresse derrière `11000000.10101000.00000001.01100100`

- `11000000` = 192
- `10101000` = 128 + 32 + 8 = 168
- `00000001` = 1
- `01100100` = 64 + 32 + 4 = 100

➡️ **192.168.1.100**

### 1.5 Pourquoi 192.168.1.300 est impossible ?

Parce qu'un octet est codé sur 8 bits et ne peut donc valoir que de 0 à 255 ; 300 dépasse cette valeur maximale.

---

## Exercice 2 : Masques et notation CIDR

### 2.1 Tableau complété

| Notation CIDR | Masque décimal | Bits hôte | Hôtes possibles |
|---|---|---|---|
| /24 | 255.255.255.0 | 8 | 2⁸ − 2 = 254 |
| /16 | 255.255.0.0 | 16 | 2¹⁶ − 2 = 65 534 |
| /27 | 255.255.255.224 | 5 | 2⁵ − 2 = 30 |
| /20 | 255.255.240.0 | 12 | 2¹² − 2 = 4 094 |
| /30 | 255.255.255.252 | 2 | 2² − 2 = 2 |
| **/25** | 255.255.255.128 | 7 | 2⁷ − 2 = 126 |
| **/21** | 255.255.248.0 | 11 | 2¹¹ − 2 = 2 046 |
| **/8** | 255.0.0.0 | 24 | 2²⁴ − 2 = 16 777 214 |
| **/28** | 255.255.255.240 | 4 | 2⁴ − 2 = 14 |

### 2.2 Masques valides ?

1. **255.255.255.100 : invalide.** 100 = `01100100` : les 1 ne sont pas contigus (un masque doit être une suite continue de 1 puis de 0).
2. **255.255.0.255 : invalide.** Après un octet à 0, on retrouve des 1 (`...00000000.11111111`), donc les 1 ne sont pas contigus.
3. **255.255.254.0 : valide.** 254 = `11111110` ; 1 contigus puis 0, c'est un /23.

### 2.3 Masque /26

```
11111111.11111111.11111111.11000000
```

Le 4ᵉ octet contient 2 bits à 1 : 128 + 64 = 192, d'où 255.255.255.192.

---

## Exercice 3 : Adresse réseau, broadcast et plage

### 3.1 Méthode binaire (ET logique)

**a. 192.168.10.77 /26**

```
IP       : 11000000.10101000.00001010.01001101   (192.168.10.77)
Masque   : 11111111.11111111.11111111.11000000   (255.255.255.192)
ET       : 11000000.10101000.00001010.01000000   (192.168.10.64)
```

Broadcast (bits hôte à 1) : `11000000.10101000.00001010.01111111` = 192.168.10.127

**b. 192.168.10.200 /27**

```
IP       : 11000000.10101000.00001010.11001000   (192.168.10.200)
Masque   : 11111111.11111111.11111111.11100000   (255.255.255.224)
ET       : 11000000.10101000.00001010.11000000   (192.168.10.192)
```

Broadcast : `11000000.10101000.00001010.11011111` = 192.168.10.223

### 3.2 Tableau complet

| | Adresse | Adresse réseau | Broadcast | 1ʳᵉ adresse hôte | Dernière adresse hôte | Nb d'hôtes |
|---|---|---|---|---|---|---|
| a | 192.168.10.77 /26 | 192.168.10.64 | 192.168.10.127 | 192.168.10.65 | 192.168.10.126 | 62 |
| b | 192.168.10.200 /27 | 192.168.10.192 | 192.168.10.223 | 192.168.10.193 | 192.168.10.222 | 30 |
| c | 192.168.5.130 /24 | 192.168.5.0 | 192.168.5.255 | 192.168.5.1 | 192.168.5.254 | 254 |
| d | 10.20.30.40 /8 | 10.0.0.0 | 10.255.255.255 | 10.0.0.1 | 10.255.255.254 | 16 777 214 |
| e | 172.16.45.200 /16 | 172.16.0.0 | 172.16.255.255 | 172.16.0.1 | 172.16.255.254 | 65 534 |
| f | 172.16.45.200 /20 | 172.16.32.0 | 172.16.47.255 | 172.16.32.1 | 172.16.47.254 | 4 094 |
| g | 192.168.1.9 /30 | 192.168.1.8 | 192.168.1.11 | 192.168.1.9 | 192.168.1.10 | 2 |
| h | 10.1.1.250 /28 | 10.1.1.240 | 10.1.1.255 | 10.1.1.241 | 10.1.1.254 | 14 |

> [!note] Détail des lignes f, g, h (nombre magique)
> - **f** : masque 255.255.240.0 → magique = 256 − 240 = 16 sur le 3ᵉ octet. 45 est dans le bloc 32–47.
> - **g** : /30 → magique = 4. 9 est dans le bloc 8–11.
> - **h** : /28 → magique = 16. 250 est dans le bloc 240–255.

### 3.3 Comparaison des lignes e et f

C'est la **même adresse IP** (172.16.45.200), mais avec un masque différent elle appartient à **deux réseaux différents** : 172.16.0.0/16 (65 534 hôtes) pour e, 172.16.32.0/20 (4 094 hôtes) pour f. Plus le masque est long (plus de bits réseau), plus le réseau est petit. L'adresse seule ne suffit pas : il faut toujours son masque.

### 3.4 Ligne f : pourquoi 0 et 255 au 4ᵉ octet ?

Avec un /20 (255.255.240.0), le 4ᵉ octet du masque vaut 0 : ses 8 bits sont **tous des bits hôte**. Le ET avec 0 donne 0 pour l'adresse réseau, et le broadcast met tous les bits hôte à 1, donc 255. La « coupure » se fait dans le 3ᵉ octet (45 → 32 pour le réseau, 47 pour le broadcast).

---

## Exercice 4 : Même réseau ou pas ?

### 4.1 Paires

| Paire | Machine A | Machine B | Masque | Réseau A | Réseau B | Même réseau ? |
|---|---|---|---|---|---|---|
| 1 | 192.168.10.2 | 192.168.11.3 | /24 | 192.168.10.0 | 192.168.11.0 | **Non** |
| 2 | 192.168.10.20 | 192.168.10.60 | /26 | 192.168.10.0 | 192.168.10.0 | **Oui** |
| 3 | 192.168.10.60 | 192.168.10.70 | /26 | 192.168.10.0 | 192.168.10.64 | **Non** |
| 4 | 172.16.33.5 | 172.16.40.10 | /20 | 172.16.32.0 | 172.16.32.0 | **Oui** |
| 5 | 10.0.0.1 | 10.255.0.1 | /8 | 10.0.0.0 | 10.0.0.0 | **Oui** |
| 6 | 192.168.1.14 | 192.168.1.17 | /29 | 192.168.1.8 | 192.168.1.16 | **Non** |

> [!tip] Vérifications rapides
> - Paire 3 : blocs de 64 → 60 est dans 0–63, 70 dans 64–127.
> - Paire 4 : blocs de 16 sur le 3ᵉ octet → 33 et 40 sont tous deux dans 32–47.
> - Paire 6 : blocs de 8 → 14 est dans 8–15, 17 dans 16–23.

### 4.2 Trouver l'erreur

1. **192.168.10.64 /26** : c'est l'**adresse réseau** du bloc 64–127, elle ne peut pas être attribuée à un hôte. → Utiliser une adresse entre .65 et .126, par ex. 192.168.10.65.
2. **192.168.10.127 /26** : c'est l'**adresse de broadcast** du bloc 64–127. → Utiliser par ex. 192.168.10.126 (ou toute autre adresse de .65 à .126).
3. **192.168.10.70 /26, passerelle 192.168.10.1** : le PC est dans le réseau 192.168.10.64/26 (hôtes .65–.126) alors que la passerelle est dans 192.168.10.0/26. Ils ne sont pas dans le même réseau, la passerelle est donc injoignable. → Mettre une passerelle dans la plage du PC, par ex. 192.168.10.65.
4. **Masque 255.255.255.100** : masque invalide (100 = `01100100`, bits non contigus). → Utiliser un masque valide, par ex. 255.255.255.0 (/24), ou 255.255.255.192 (/26) selon le plan d'adressage voulu.

---

## Exercice 5 : Découpage en sous-réseaux

### 5.1 Découper 192.168.50.0/24 en 8 sous-réseaux

1. 8 = 2³ → on emprunte **3 bits**. Nouveau masque : **/27 = 255.255.255.224** (24 + 3).
2. Bits hôte restants : 8 − 3 = 5 → 2⁵ − 2 = **30 hôtes** par sous-réseau (bloc de 32).
3. Plan d'adressage :

| N° | Adresse réseau | Première adresse hôte | Dernière adresse hôte | Broadcast |
|---|---|---|---|---|
| 1 | 192.168.50.0 | 192.168.50.1 | 192.168.50.30 | 192.168.50.31 |
| 2 | 192.168.50.32 | 192.168.50.33 | 192.168.50.62 | 192.168.50.63 |
| 3 | 192.168.50.64 | 192.168.50.65 | 192.168.50.94 | 192.168.50.95 |
| 4 | 192.168.50.96 | 192.168.50.97 | 192.168.50.126 | 192.168.50.127 |
| 5 | 192.168.50.128 | 192.168.50.129 | 192.168.50.158 | 192.168.50.159 |
| 6 | 192.168.50.160 | 192.168.50.161 | 192.168.50.190 | 192.168.50.191 |
| 7 | 192.168.50.192 | 192.168.50.193 | 192.168.50.222 | 192.168.50.223 |
| 8 | 192.168.50.224 | 192.168.50.225 | 192.168.50.254 | 192.168.50.255 |

### 5.2 100 machines par sous-réseau dans 192.168.60.0/24

1. 2⁶ − 2 = 62 < 100 et 2⁷ − 2 = 126 ≥ 100 → on garde **7 bits hôte**. Masque : **/25 = 255.255.255.128**.
2. On a emprunté 1 bit → 2¹ = **2 sous-réseaux** :

| N° | Adresse réseau | Broadcast |
|---|---|---|
| 1 | 192.168.60.0/25 | 192.168.60.127 |
| 2 | 192.168.60.128/25 | 192.168.60.255 |

### 5.3 10 machines par sous-réseau dans 192.168.70.0/24

1. 2³ − 2 = 6 < 10 et 2⁴ − 2 = 14 ≥ 10 → **4 bits hôte**, donc masque **/28 = 255.255.255.240**. On a emprunté 4 bits → 2⁴ = **16 sous-réseaux** (bloc de 16).
2. Les 3 premiers et le dernier :

| N° | Adresse réseau | Plage des hôtes | Broadcast |
|---|---|---|---|
| 1 | 192.168.70.0/28 | 192.168.70.1 → 192.168.70.14 | 192.168.70.15 |
| 2 | 192.168.70.16/28 | 192.168.70.17 → 192.168.70.30 | 192.168.70.31 |
| 3 | 192.168.70.32/28 | 192.168.70.33 → 192.168.70.46 | 192.168.70.47 |
| … | … | … | … |
| 16 | 192.168.70.240/28 | 192.168.70.241 → 192.168.70.254 | 192.168.70.255 |

---

## Exercice 6 : Cas pratique, réseau d'une PME (192.168.100.0/24)

| Service | Nombre de postes |
|---|---|
| Production | 50 |
| Wi-Fi invités | 25 |
| Comptabilité | 20 |
| Direction | 12 |

### 6.1 Quel service impose la taille ?

C'est **Production** (50 postes, le plus gros besoin). Il faut 50 postes + 1 passerelle = 51 adresses : 2⁵ − 2 = 30 est insuffisant, 2⁶ − 2 = 62 convient. → **6 bits hôte**, masque **/26 = 255.255.255.192** (bloc de 64).

### 6.2 Combien de sous-réseaux ?

Il y a 2 bits empruntés (de /24 à /26) → 2² = **4 sous-réseaux**. C'est **juste suffisant** pour les 4 services (aucune marge pour un 5ᵉ service).

### 6.3 Plan d'adressage

| Service | Adresse réseau | Masque | Passerelle | Plage des postes | Broadcast |
|---|---|---|---|---|---|
| Production | 192.168.100.0 | /26 (255.255.255.192) | 192.168.100.1 | 192.168.100.2 → 192.168.100.62 | 192.168.100.63 |
| Wi-Fi invités | 192.168.100.64 | /26 (255.255.255.192) | 192.168.100.65 | 192.168.100.66 → 192.168.100.126 | 192.168.100.127 |
| Comptabilité | 192.168.100.128 | /26 (255.255.255.192) | 192.168.100.129 | 192.168.100.130 → 192.168.100.190 | 192.168.100.191 |
| Direction | 192.168.100.192 | /26 (255.255.255.192) | 192.168.100.193 | 192.168.100.194 → 192.168.100.254 | 192.168.100.255 |

### 6.4 Adresses inutilisées dans le sous-réseau Direction

Le sous-réseau offre 62 adresses hôte. Il en utilise 12 pour les postes + 1 pour la passerelle = 13. Il reste **62 − 13 = 49 adresses inutilisées** (50 si on ne compte pas la passerelle).

**Ce découpage n'est pas économe** : tous les services reçoivent 62 adresses alors qu'ils n'en ont besoin que de 12 à 50, ce qui gaspille beaucoup d'adresses. Une solution est le **VLSM** (masques de tailles différentes), par exemple : Production /26 (62), Wi-Fi invités /27 (30), Comptabilité /27 (30), Direction /28 (14). On libérerait ainsi de la place pour de futurs services.
