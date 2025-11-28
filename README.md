# 🌐 Infrastructure Cloud Hybride AWS + VPN Site-to-Site

![AWS](https://img.shields.io/badge/AWS-VPC%20%2B%20EC2-orange?style=for-the-badge&logo=amazon-aws)
![VPN](https://img.shields.io/badge/VPN-IPsec%20Site--to--Site-blue?style=for-the-badge)
![Monitoring](https://img.shields.io/badge/Monitoring-Grafana%20%2B%20Prometheus-green?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-En%20développement-yellow?style=for-the-badge)

## 📋 Table des matières

- [Présentation](#-présentation)
- [Architecture](#-architecture)
- [Technologies utilisées](#-technologies-utilisées)
- [Prérequis](#-prérequis)
- [Installation](#-installation)
- [Configuration](#-configuration)
- [Tests](#-tests)
- [Documentation](#-documentation)
- [Auteur](#-auteur)

---

## 🎯 Présentation

Ce projet consiste à déployer une **infrastructure Cloud hybride** interconnectant :

- Un réseau local virtualisé (VirtualBox) avec **pfSense** et **Ubuntu Server**
- Un environnement AWS (VPC, EC2, VPN Gateway)
- Un tunnel **VPN IPsec Site-to-Site** sécurisé
- Un système de supervision complet (**Grafana**, **Prometheus**, **Loki**)

### Objectifs pédagogiques

✅ Maîtriser les réseaux Cloud (VPC, subnets, routing)  
✅ Configurer un VPN professionnel IPsec  
✅ Déployer une stack de monitoring moderne  
✅ Appliquer les best practices de sécurité  
✅ Documenter une infrastructure complexe  

---

## 🏗️ Architecture

### Schéma global

![Architecture Globale](architecture/diagrams/architecture_globale.png)

### Composants

#### 🖥️ Infrastructure locale (VirtualBox)

| Composant | IP | Rôle |
|-----------|-----|------|
| **pfSense** | 192.168.10.1 | Firewall + VPN Client |
| **Ubuntu Server** | 192.168.10.10 | Reverse Proxy + Monitoring |

#### ☁️ Infrastructure AWS

| Composant | Réseau | Rôle |
|-----------|--------|------|
| **VPC** | 10.0.0.0/16 | Réseau privé AWS |
| **Subnet Public** | 10.0.1.0/24 | Ressources accessibles Internet |
| **Subnet Privé** | 10.0.2.0/24 | Ressources internes |
| **EC2 Reverse Proxy** | 10.0.1.10 | Nginx public |
| **EC2 Web Server** | 10.0.2.10 | Apache privé |
| **VPN Gateway** | - | Endpoint VPN AWS |

#### 🔐 Tunnel VPN

- **Type** : IPsec Site-to-Site
- **Chiffrement** : AES-256
- **Authentification** : SHA-256
- **PFS** : DH Group 2

---

## 🛠️ Technologies utilisées

### Infrastructure

- **Hyperviseur** : VirtualBox 7.0
- **Firewall** : pfSense 2.7.2
- **OS Serveur** : Ubuntu Server 22.04 LTS
- **Cloud Provider** : AWS (VPC, EC2, VPN)

### Services

- **Reverse Proxy** : Nginx
- **Containerisation** : Docker + Docker Compose
- **Supervision** :
  - Grafana (dashboards)
  - Prometheus (métriques)
  - Loki (logs)
  - Promtail (agent)
  - Node Exporter (collecteur)

### Sécurité

- IPsec VPN (AES-256)
- Security Groups AWS
- Firewall pfSense
- UFW (Ubuntu)
- Fail2ban

---

## 📋 Prérequis

### Matériel

- **CPU** : 4 cœurs minimum (6 recommandé)
- **RAM** : 8 Go minimum (16 Go recommandé)
- **Disque** : 50 Go d'espace libre
- **Connexion Internet** : Stable (ADSL minimum)

### Logiciels

- VirtualBox 7.0+
- Git
- Éditeur de texte (VS Code recommandé)
- Compte AWS (Free Tier)

### Connaissances

- Bases Linux (ligne de commande)
- Notions réseaux (IP, routage, NAT)
- Notions AWS (EC2, VPC)

---

## 🚀 Installation

### Phase 1 : Environnement local

1. **Installation VirtualBox**
```bash
   # Voir docs/01-installation-guide.md
```

2. **Déploiement pfSense**
   - Créer VM (512 MB RAM, 1 vCPU)
   - Configuration interfaces WAN/LAN
   - Activation DHCP

3. **Déploiement Ubuntu Server**
   - Créer VM (2 GB RAM, 2 vCPU)
   - Configuration IP statique
   - Installation Docker

### Phase 2 : Infrastructure AWS

1. **Création VPC**
```bash
   # Voir docs/03-aws-setup.md
```

2. **Déploiement EC2**
   - Instance Reverse Proxy (subnet public)
   - Instance Web Server (subnet privé)

### Phase 3 : VPN Site-to-Site

1. **Configuration AWS VPN**
   - Virtual Private Gateway
   - Customer Gateway
   - VPN Connection

2. **Configuration pfSense**
   - IPsec Phase 1 & 2
   - Firewall rules
   - NAT configuration

📖 **Documentation détaillée** : [docs/02-vpn-configuration.md](docs/02-vpn-configuration.md)

---

## ⚙️ Configuration

### Monitoring

Déploiement de la stack de supervision :
```bash
cd monitoring
docker-compose up -d
```

Accès Grafana : `http://192.168.10.10:3000`  
Credentials : `admin` / `admin123`

📖 **Documentation** : [docs/03-monitoring-setup.md](docs/03-monitoring-setup.md)

---

## 🧪 Tests

### Vérification connectivité VPN
```bash
# Depuis Ubuntu local
ping 10.0.1.10  # EC2 public
ping 10.0.2.10  # EC2 privé

# Depuis EC2 AWS
ping 192.168.10.10  # Ubuntu local
```

### Tests monitoring

- [ ] Prometheus collecte les métriques
- [ ] Grafana affiche les dashboards
- [ ] Loki reçoit les logs

📖 **Procédures de test** : [tests/test-connectivity.sh](tests/test-connectivity.sh)

---

## 📚 Documentation

- [01 - Guide d'installation](docs/01-installation-guide.md)
- [02 - Configuration VPN](docs/02-vpn-configuration.md)
- [03 - Setup Monitoring](docs/03-monitoring-setup.md)
- [04 - Sécurité](docs/04-security-hardening.md)
- [05 - Troubleshooting](docs/05-troubleshooting.md)

---

## 👤 Auteur

**[Votre Nom]**

- GitHub : [@votre-username](https://github.com/votre-username)
- LinkedIn : [Votre Profil](https://linkedin.com/in/votre-profil)
- Email : votre.email@example.com

---

## 📄 Licence

Ce projet est sous licence MIT. Voir le fichier [LICENSE](LICENSE) pour plus de détails.

---

## 🙏 Remerciements

- Documentation AWS
- Communauté pfSense
- Projet Prometheus/Grafana

---

**⭐ Si ce projet vous a été utile, n'hésitez pas à mettre une étoile !**