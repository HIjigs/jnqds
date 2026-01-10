# Atelier – Sécurité des Endpoints & Supervision SIEM (Wazuh)

## Présentation
Ce laboratoire vise à déployer une plateforme **Wazuh** afin de superviser la sécurité
des endpoints **Linux** et **Windows**.  
Wazuh est utilisé comme solution **SIEM** et **EDR** dans un environnement Cloud
hébergé sur **AWS Learner Lab**.

---

## Objectifs
- Déployer un serveur Wazuh All‑in‑One
- Enrôler des agents Linux et Windows
- Générer et analyser des événements de sécurité
- Comprendre les concepts SIEM, EDR et IAM
- S’initier au threat hunting

---

## Architecture

### Composants
- **Wazuh Server (Ubuntu 22.04)**
  - Manager
  - Indexer
  - Dashboard
- **Client Linux (Ubuntu 22.04)**
  - Agent Wazuh
- **Client Windows (Windows Server)**
  - Agent Wazuh
  - (Optionnel) Sysmon

### Flux & Ports
- Agents → Wazuh Server : `1514/TCP`
- Enrôlement agents : `1515/TCP`
- Accès Dashboard : `443/TCP`
- SSH Linux : `22/TCP`
- RDP Windows : `3389/TCP`

---

## Prérequis
- Compte AWS Learner Lab actif
- 3 instances EC2 dans le même VPC
- Accès administrateur sur les machines
- Accès Internet sortant autorisé

---

## Déploiement du serveur Wazuh

Sur l’instance **Ubuntu (Wazuh Server)** :

```bash
# Mise à jour du système
sudo apt update && sudo apt -y upgrade

# Téléchargement du script officiel Wazuh
curl -sO https://packages.wazuh.com/4.7/wazuh-install.sh

# Installation Wazuh All-in-One
sudo bash wazuh-install.sh -a

# --- Informations de connexion (affichées à la fin du script) ---
# URL du Wazuh Dashboard : https://<IP_DU_SERVEUR_WAZUH>
# Utilisateur par défaut : admin
# Mot de passe initial  : affiché à l'écran
# ⚠️ Noter ces informations pour la première connexion
