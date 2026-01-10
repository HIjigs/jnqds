# Atelier – Sécurité des Endpoints & Supervision SIEM (Wazuh)

## Présentation
Ce laboratoire a pour objectif de déployer une plateforme **Wazuh** afin d’assurer la
supervision de la sécurité sur des endpoints **Linux** et **Windows**.
La solution Wazuh est utilisée à la fois comme **SIEM** (collecte, corrélation et analyse
des événements) et comme **EDR** (détection d’activités suspectes sur les hôtes).

L’environnement est déployé sur **AWS Learner Lab** et reproduit une architecture SOC
simplifiée.

---

## Objectifs
À l’issue de ce TP, l’étudiant sera capable de :
- Déployer un serveur **Wazuh All-in-One**
- Enrôler des agents Linux et Windows
- Générer et analyser des alertes de sécurité
- Comprendre les concepts **SIEM**, **EDR** et **IAM**
- Réaliser une première approche du **threat hunting**

---

## Architecture

### Composants
- **Wazuh Server (Ubuntu 22.04)**
  - Wazuh Manager
  - Wazuh Indexer
  - Wazuh Dashboard
- **Client Linux (Ubuntu 22.04)**
  - Agent Wazuh
- **Client Windows (Windows Server)**
  - Agent Wazuh
  - (Optionnel) Sysmon

### Flux & Ports

| Source | Destination | Port | Description |
|------|------------|------|-------------|
| Agents | Wazuh Server | 1514/TCP | Communication des logs |
| Agents | Wazuh Server | 1515/TCP | Enrôlement des agents |
| Administrateur | Wazuh Server | 443/TCP | Accès Dashboard |
| Administrateur | Linux Client | 22/TCP | SSH |
| Administrateur | Windows Client | 3389/TCP | RDP |

---

## Prérequis
- Compte **AWS Learner Lab** actif
- 3 instances EC2 dans le même VPC
- Accès administrateur sur les machines
- Accès Internet sortant autorisé
- Navigateur Web (Chrome / Firefox)

---

## Déploiement du serveur Wazuh

Sur l’instance **Ubuntu (Wazuh Server)** :

```bash
sudo apt update && sudo apt -y upgrade
curl -sO https://packages.wazuh.com/4.7/wazuh-install.sh
sudo bash wazuh-install.sh -a
