---
titre: Cloud, conteneurs et Infrastructure as Code
formateur: Elimane NDIAYE
niveau: 2e année (introduction)
tags: [cours, cloud, docker, iac, terraform, ansible, devops]
---

# 32 — Cloud, réseaux virtuels, conteneurs et Infrastructure as Code

Précédent : [[31 - Architecture d'entreprise et haute disponibilité]] · Index : [00 - Index](00%20-%20Index.md)

Définitions de départ : [01 - Vocabulaire de l'infrastructure (Cloud, DevOps, Virtualisation)](01%20-%20Vocabulaire%20de%20l%27infrastructure%20%28Cloud%2C%20DevOps%2C%20Virtualisation%29.md).

## 1. Réseaux dans le cloud
Les notions restent les mêmes (IP, sous-réseaux, routage, firewall), **virtualisées** et pilotées par API.
| Concept | AWS | Azure | Rôle |
|---|---|---|---|
| Réseau privé virtuel | **VPC** | **VNet** | espace d'adressage isolé (ex. `10.0.0.0/16`) |
| Sous-réseau | Subnet | Subnet | public (routé vers Internet) ou privé |
| Passerelle Internet | Internet Gateway | (routes par défaut) | accès Internet |
| NAT | NAT Gateway | NAT Gateway | sortie Internet des sous-réseaux privés |
| Filtrage | Security Group / NACL | NSG | **firewall** par instance (stateful) / par sous-réseau |
| Table de routage | Route table | Route table (UDR) | routes |
| Interconnexion | Peering, Transit Gateway | Peering, vWAN | relier des VPC/VNet |
| Lien privé vers on-premise | VPN, Direct Connect | VPN, ExpressRoute | hybride |
| DNS | Route 53 | Azure DNS | |
| Répartition de charge | ELB/ALB | Load Balancer | |
```text
VPC 10.0.0.0/16
 ├─ Subnet public 10.0.1.0/24  ── Internet Gateway ── Internet    (load balancer, bastion)
 └─ Subnet privé  10.0.2.0/24  ── NAT Gateway (sortie seulement)  (serveurs applicatifs, BDD)
```
**Modèle de responsabilité partagée** : le fournisseur sécurise le cloud (datacenters, hyperviseur) ; **vous** sécurisez **dans** le cloud (configuration, accès, données).

## 2. Conteneurs et Docker
**Conteneur** : processus **isolé** (namespaces, cgroups) qui **partage le noyau de l'hôte**, contenant l'application et ses dépendances. Plus **léger et rapide** qu'une VM.
```text
VM :        App | OS invité | Hyperviseur | Matériel      (1 OS par VM)
Conteneur : App + libs | Moteur (Docker) | OS hôte | Matériel   (noyau partagé)
```
Vocabulaire : **image** (modèle en lecture seule), **conteneur** (instance en cours), **Dockerfile** (recette), **registre** (Docker Hub), **volume** (données persistantes), **réseau**.

### Commandes essentielles
```bash
docker run -d --name web -p 8080:80 nginx     # lance nginx, port hôte 8080 → conteneur 80
docker ps ; docker ps -a
docker logs -f web
docker exec -it web sh
docker stop web ; docker rm web
docker images ; docker pull debian:12
docker build -t monapp:1.0 .
docker volume create data ; docker run -v data:/var/lib/mysql …
docker network ls ; docker network create lan-app
```
### Dockerfile simple
```dockerfile
FROM python:3.12-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY . .
EXPOSE 5000
CMD ["python", "app.py"]
```
### Docker Compose (plusieurs services)
```yaml
services:
  web:
    image: nginx
    ports: ["8080:80"]
    depends_on: [db]
  db:
    image: mariadb:11
    environment:
      MARIADB_ROOT_PASSWORD: "${DB_PASSWORD}"
    volumes: [dbdata:/var/lib/mysql]
volumes:
  dbdata:
```
`docker compose up -d` · `docker compose down`
### Réseau Docker
Réseau **bridge** par défaut (NAT vers l'hôte), publication de ports `-p hôte:conteneur`, réseaux personnalisés avec **DNS interne** (les conteneurs se joignent par leur nom), modes `host`, `none`, `overlay` (multi-hôtes).
### Sécurité
Images officielles/minimales, ne pas exécuter en root, ne pas mettre de secrets dans les images, mettre à jour, limiter les ports publiés. **Orchestration** : Kubernetes (aperçu : pods, services, déploiements).

## 3. Infrastructure as Code (IaC)
**Idée** : décrire l'infrastructure dans des **fichiers** (versionnés dans Git), et non par des clics manuels → **reproductible**, relisible, testable, documentée, rapide.
| Approche | Outils | Rôle |
|---|---|---|
| **Provisionnement** (créer l'infra) | **Terraform**, OpenTofu, CloudFormation, Bicep | créer VM, réseaux, bases |
| **Configuration** (configurer les machines) | **Ansible**, Puppet, Chef | installer, configurer les services |
Notions : **déclaratif** (on décrit l'état voulu) vs impératif, **idempotence** (rejouer donne le même résultat), *drift* (dérive), *state*.

### Exemple Ansible (playbook)
```yaml
- name: Installer et démarrer nginx
  hosts: webservers
  become: true
  tasks:
    - name: Paquet nginx
      ansible.builtin.apt:
        name: nginx
        state: present
        update_cache: true
    - name: Service nginx actif
      ansible.builtin.service:
        name: nginx
        state: started
        enabled: true
```
`ansible-playbook -i inventaire.ini site.yml`

### Exemple Terraform (principe)
```hcl
resource "aws_vpc" "main" {
  cidr_block = "10.0.0.0/16"
}
resource "aws_subnet" "public" {
  vpc_id     = aws_vpc.main.id
  cidr_block = "10.0.1.0/24"
}
```
`terraform init` → `terraform plan` → `terraform apply`

## 4. DevOps de bout en bout
```text
Git (code + IaC) → CI (build, tests, scan) → Image Docker → Registre → CD (Terraform/Ansible/K8s) → Supervision
```
Pipeline typique : Git push → tests automatiques → build de l'image → déploiement → **supervision** ([29 - Supervision et logs](29%20-%20Supervision%20et%20logs.md)) → retour d'information. Bonnes pratiques : secrets dans un coffre-fort (pas dans Git), revue de code, environnements dev/test/prod, déploiements progressifs et retour arrière.

## 📌 À retenir
- Cloud : VPC/VNet, sous-réseaux, security groups, NAT gateway — mêmes principes que le réseau classique.
- Conteneur = processus isolé partageant le noyau ; image → conteneur.
- IaC = infrastructure décrite en code : Terraform (créer), Ansible (configurer).

## 🧪 Exercices
1. Différence entre une VM et un conteneur ?
2. Que fait `docker run -p 8080:80 nginx` ?
3. Pourquoi l'idempotence est-elle importante en IaC ?
<details><summary>Réponses</summary>1. La VM embarque son OS (hyperviseur), le conteneur partage le noyau de l'hôte. 2. Lance nginx et publie son port 80 sur le port 8080 de l'hôte. 3. Pour pouvoir rejouer un script sans effet de bord et converger vers l'état voulu.</details>

## 🔗 Liens
[[26 - Virtualisation avancée]] · [23 - Linux pour l'infrastructure](23%20-%20Linux%20pour%20l%27infrastructure.md) · [[31 - Architecture d'entreprise et haute disponibilité]] · [00 - Index](00%20-%20Index.md)
