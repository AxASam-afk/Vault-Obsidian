---
titre: Linux pour l'infrastructure
formateur: Elimane NDIAYE
niveau: 1re-2e année
tags: [cours, linux, systemd, commandes]
---

# 23 — Linux pour l'infrastructure

Précédent : [[22 - Sécurité réseau]] · Suivant : [24 - Windows Server](24%20-%20Windows%20Server.md)

## 1. Architecture
```text
Applications / Utilisateur
   Shell (bash)
   Bibliothèques
   ─────── Noyau (kernel) : processus, mémoire, pilotes, réseau, systèmes de fichiers
   Matériel
```
Le **noyau** gère le matériel ; le **shell** interprète les commandes ; le **terminal** en est l'interface. Distributions : Debian, Ubuntu, RHEL/Rocky/Alma, Alpine…

## 2. Arborescence
| Répertoire | Contenu |
|---|---|
| `/` | racine |
| `/etc` | **configuration** |
| `/home` | dossiers utilisateurs |
| `/root` | dossier de root |
| `/var/log` | **journaux** |
| `/var` | données variables |
| `/usr`, `/bin`, `/sbin` | programmes |
| `/tmp` | temporaire |
| `/dev`, `/proc`, `/sys` | périphériques, infos noyau |

Commandes de base : `pwd`, `ls -la`, `cd`, `mkdir`, `cp`, `mv`, `rm`, `cat`, `less`, `grep`, `find`, `nano`/`vim`, `man`, `sudo`, `tail -f`.

## 3. Permissions
`ls -l` → `-rwxr-xr--  1 alice dev  120 Jan 1 fichier`
```text
- rwx r-x r--
│ │   │   └─ autres (others)
│ │   └───── groupe
│ └───────── propriétaire (user)
└─────────── type (- fichier, d dossier, l lien)
```
r=4, w=2, x=1 → `chmod 754 fichier` (rwx r-x r--). `chmod u+x script.sh`, `chown alice:dev fichier`.
**Erreur fréquente** : `chmod 777` (trop permissif) ; oublier le droit `x` sur un dossier.

## 4. Utilisateurs et groupes
Fichiers : `/etc/passwd` (comptes), `/etc/shadow` (hachages de mots de passe, root uniquement), `/etc/group`.
```bash
sudo adduser alice           # créer (Debian/Ubuntu)
sudo useradd -m -s /bin/bash bob
sudo passwd alice
sudo usermod -aG sudo alice  # ajouter au groupe sudo (-a indispensable !)
sudo groupadd dev
id alice ; groups alice
sudo userdel -r bob
```

## 5. Processus et services
```bash
ps aux | grep nginx    top / htop
kill 1234 ; kill -9 1234 ; pkill nginx
systemctl status ssh
```
**systemd** gère les services :
```bash
sudo systemctl start|stop|restart|reload nginx
sudo systemctl enable --now nginx    # au démarrage + maintenant
systemctl is-active nginx ; systemctl list-units --type=service
```
Fichier d'unité : `/etc/systemd/system/mon.service` (`[Unit]`, `[Service]` avec `ExecStart=`, `[Install]`), puis `systemctl daemon-reload`.

## 6. Paquets
| Famille | Commandes |
|---|---|
| Debian/Ubuntu | `sudo apt update && sudo apt install nginx` ; `apt remove`, `apt upgrade` |
| RHEL/Rocky | `sudo dnf install httpd` ; `dnf update` |

## 7. Réseau sous Linux
- **Interfaces** : `ip a`, `ip link`. Configuration : **Netplan** (`/etc/netplan/*.yaml`, Ubuntu), NetworkManager (`nmcli`), `/etc/network/interfaces` (Debian).
- **Routes** : `ip route`. **DNS** : `/etc/resolv.conf`, `systemd-resolved` (`resolvectl status`), `/etc/hosts`.
```yaml
# /etc/netplan/01-net.yaml
network:
  version: 2
  ethernets:
    ens33:
      addresses: [192.168.1.50/24]
      routes:
        - to: default
          via: 192.168.1.1
      nameservers:
        addresses: [1.1.1.1, 8.8.8.8]
```
`sudo netplan apply`

## 8. Logs
`/var/log/syslog` (ou `messages`), `/var/log/auth.log` (connexions), `journalctl`, `dmesg`. Rotation : `logrotate`.

## 9. Commandes réseau — fiches

**`ip`**
- Syntaxe : `ip [objet] [action]` · Ex. : `ip a`, `ip link set eth0 up`, `sudo ip addr add 192.168.1.50/24 dev eth0`
- Explication : gère interfaces, adresses, routes. Remplace `ifconfig`/`route`.
- Usage : voir/modifier la config IP. Erreur : les changements manuels sont **perdus au redémarrage**.

**`ping`**
- `ping -c 4 8.8.8.8` (Linux : sans `-c` il tourne indéfiniment).
- Teste la joignabilité (ICMP), la latence, la perte. Erreur : un ping bloqué n'implique pas que la machine est éteinte.

**`ss`** (remplace `netstat`)
- `ss -tulpn` : ports en écoute + processus ; `ss -t state established`.
- Usage : vérifier qu'un service écoute. Erreur : oublier `sudo` pour voir les processus.

**`ip route`**
- `ip route`, `ip route get 8.8.8.8`, `sudo ip route add default via 192.168.1.1`.
- Usage : diagnostic de passerelle/routage.

**`ip neigh`**
- `ip neigh show` : table ARP/NDP. Usage : vérifier la MAC d'un voisin, ARP.

**`traceroute`**
- `traceroute -n 8.8.8.8` (`tracert` sous Windows). Affiche les sauts (TTL croissants).
- Usage : localiser où le trafic s'arrête. Erreur : `* * *` ≠ forcément panne (ICMP filtré).

**`dig` / `nslookup`**
- `dig +short A exemple.fr`, `dig @8.8.8.8 MX exemple.fr` · Usage : diagnostic DNS ([16 - DNS](16%20-%20DNS.md)).

**`curl`**
- `curl -I https://exemple.fr` (en-têtes), `curl -v`, `curl -k` (ignore certificat, à éviter), `curl -o f url`.
- Usage : tester HTTP(S) et API. Erreur : `-k` cache un vrai problème de certificat.

**`wget`**
- `wget https://exemple.fr/fichier.iso` · télécharge un fichier (reprise `-c`).

**`ssh`**
- `ssh user@hôte`, `ssh -p 2222 user@hôte`, `ssh -L 8080:localhost:80 user@hôte` (tunnel).
- Erreur : mauvaises permissions de clé (`chmod 600 ~/.ssh/id_*`).

**`scp`**
- `scp fichier user@hôte:/chemin` ; `scp -r dossier user@hôte:/tmp/` · copie chiffrée via SSH.
- Erreur : inverser source et destination.

**`systemctl`** / **`journalctl`**
- `systemctl status ssh` ; `journalctl -u ssh --since "1 hour ago"` ; `journalctl -xe` ; `journalctl -f`.
- Usage : état et logs d'un service qui ne démarre pas.

**`tcpdump`**
- `sudo tcpdump -i eth0 -n port 53` ; `sudo tcpdump -i any host 192.168.1.10 -w capture.pcap`
- Usage : capturer et analyser des paquets (à ouvrir ensuite dans Wireshark). Erreur : oublier `-n` (résolution DNS lente), capturer sans filtre (fichier énorme).

## 📌 À retenir
- `/etc` = config, `/var/log` = logs ; permissions rwx = 4-2-1.
- `systemctl` + `journalctl` pour les services.
- `ip`, `ss`, `dig`, `curl`, `tcpdump` = boîte à outils réseau.

## 🧪 Exercices
1. Que signifie `chmod 640` ?
2. Comment vérifier que nginx écoute sur le port 80 ?
3. Comment consulter les logs SSH des dernières 24 h ?
<details><summary>Réponses</summary>1. rw- r-- --- . 2. `ss -tulpn | grep :80`. 3. `journalctl -u ssh --since yesterday`.</details>

## 🔗 Liens
[18 - Protocoles essentiels](18%20-%20Protocoles%20essentiels.md) · [[30 - Diagnostic réseau]] · [29 - Supervision et logs](29%20-%20Supervision%20et%20logs.md) · [00 - Index](00%20-%20Index.md)
