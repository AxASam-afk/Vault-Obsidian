---
titre: Wi-Fi
formateur: Elimane NDIAYE
niveau: 1re-2e année
tags: [cours, wifi, 802.11, wpa]
---

# 21 — Wi-Fi (IEEE 802.11)

Précédent : [20 - NAT et PAT](20%20-%20NAT%20et%20PAT.md) · Suivant : [[22 - Sécurité réseau]]

## 1. Principe
Le Wi-Fi est un **LAN sans fil (WLAN)** basé sur la norme **IEEE 802.11** : les données sont transmises par **ondes radio**. Le support est **partagé et half-duplex** : un seul équipement émet à la fois par canal, géré par **CSMA/CA** (évitement de collisions, car on ne peut pas écouter en émettant).

## 2. Vocabulaire
| Terme | Définition |
|---|---|
| **AP** (point d'accès) | relie les clients Wi-Fi au réseau filaire |
| **SSID** | **nom** du réseau Wi-Fi (ex. « Entreprise-WiFi ») |
| **BSSID** | **MAC de la radio** de l'AP (identifie un AP précis) |
| **BSS / ESS** | cellule d'un AP / ensemble de plusieurs AP partageant un SSID |
| **Canal** | portion de fréquence utilisée |
| **RSSI** | puissance du signal reçu (dBm, ex. −60 dBm bon, −80 faible) |
| **Contrôleur (WLC)** | gère centralement plusieurs AP |

## 3. Normes
| Norme | Appellation | Bandes | Débit théorique max |
|---|---|---|---|
| 802.11b | — | 2,4 GHz | 11 Mbit/s |
| 802.11a | — | 5 GHz | 54 Mbit/s |
| 802.11g | — | 2,4 GHz | 54 Mbit/s |
| 802.11n | **Wi-Fi 4** | 2,4 / 5 GHz | 600 Mbit/s |
| 802.11ac | **Wi-Fi 5** | 5 GHz | ~3,5–6,9 Gbit/s |
| 802.11ax | **Wi-Fi 6 / 6E** | 2,4 / 5 / (6 GHz en 6E) | ~9,6 Gbit/s |
| 802.11be | **Wi-Fi 7** | 2,4 / 5 / 6 GHz | ~46 Gbit/s |

## 4. Bandes et canaux
| Bande | Portée | Débit | Interférences | Remarque |
|---|---|---|---|---|
| **2,4 GHz** | grande, traverse mieux les murs | plus faible | **nombreuses** (Bluetooth, micro-ondes, voisins) | seulement **3 canaux non chevauchants : 1, 6, 11** (20 MHz) |
| **5 GHz** | moyenne | élevé | moindres | beaucoup de canaux (certains DFS/radar) |
| **6 GHz** | plus courte | très élevé | très faibles | Wi-Fi 6E/7, **WPA3 obligatoire** |

Largeur de canal : 20/40/80/160 MHz (plus large = plus de débit, moins de canaux disponibles).

## 5. Débit réel
Le débit annoncé est **théorique**. Le débit réel représente typiquement **40–60 %** de la valeur théorique, car : support partagé entre tous les clients, half-duplex, distance et obstacles, interférences, surcharge protocolaire, capacités du client (nombre d'antennes, MIMO).

## 6. Roaming
Un client passe d'un AP à un autre **sans perdre la connexion** (même SSID, même VLAN). Amélioré par **802.11k/v/r**. La qualité dépend du **recouvrement** des cellules (~15–20 %) et de la configuration du seuil de bascule.

## 7. Sécurité et authentification
| Protocole | Statut | Remarque |
|---|---|---|
| WEP | ❌ cassé | ne jamais utiliser |
| WPA (TKIP) | ❌ obsolète | |
| **WPA2** (AES-CCMP) | ✅ courant | Personal (**PSK**) ou Enterprise |
| **WPA3** | ✅ recommandé | **SAE** remplace le PSK (résiste aux attaques par dictionnaire hors-ligne), chiffrement renforcé, PMF obligatoire |

- **Personal (PSK)** : mot de passe partagé (domicile, petites structures).
- **Enterprise (802.1X)** : chaque utilisateur s'authentifie individuellement via **RADIUS** (**EAP** : PEAP, EAP-TLS) et un annuaire ([25 - Active Directory et LDAP](25%20-%20Active%20Directory%20et%20LDAP.md)). Permet de révoquer un utilisateur, d'affecter un **VLAN** dynamique.

```text
Client (supplicant) ── AP (authenticator) ── Serveur RADIUS (authentication server) ── AD
```
Bonnes pratiques : WPA3/WPA2-AES, mot de passe long, réseau **invités isolé** (VLAN dédié), désactiver WPS, mises à jour firmware.

## 8. Problèmes courants
| Symptôme | Causes possibles |
|---|---|
| Signal faible / coupures | distance, obstacles, AP mal placé |
| Lenteur | trop de clients, canal saturé, bande 2,4 GHz encombrée |
| Pas d'IP | DHCP indisponible, VLAN incorrect |
| Déconnexions en déplacement | roaming mal réglé |
| Connexion refusée | mauvaise clé, RADIUS injoignable, certificat |

Outils : analyseur Wi-Fi (RSSI, canaux), `iw dev wlan0 link`, `netsh wlan show interfaces`, `nmcli dev wifi`.

## 📌 À retenir
- Wi-Fi = 802.11, CSMA/CA, partagé. Canaux 2,4 GHz : 1-6-11.
- WPA3 > WPA2 ; Enterprise = 802.1X/RADIUS.
- Débit réel ≪ débit théorique.

## 🧪 Exercices
1. Pourquoi les canaux 1, 6 et 11 en 2,4 GHz ?
2. Différence entre SSID et BSSID ?
<details><summary>Réponses</summary>1. Ce sont les seuls à ne pas se chevaucher en 20 MHz. 2. Nom du réseau / MAC de l'AP.</details>

## 🔗 Liens
[[03 - Types de réseaux (LAN, WLAN, WAN, MAN, Internet)]] · [[22 - Sécurité réseau]] · [00 - Index](00%20-%20Index.md)
