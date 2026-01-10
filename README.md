# Atelier – Sécurité des Endpoints & Supervision SIEM (Wazuh)

## Présentation
Ce laboratoire a pour objectif de déployer et d’utiliser une plateforme **Wazuh** afin de superviser des systèmes **Linux et Windows**.  
Wazuh est utilisé comme solution **SIEM** (collecte et corrélation de logs) et **EDR** (détection d’activités suspectes sur les endpoints).

L’environnement est déployé sur **AWS Learner Lab** et reproduit une architecture SOC simplifiée.

---

## Objectifs
À l’issue de ce TP, l’étudiant sera capable de :
- Déployer un serveur **Wazuh All-in-One**
- Enrôler des agents Linux et Windows
- Observer des événements de sécurité en temps réel
- Comprendre les différences entre **SIEM** et **EDR**
- Analyser des alertes liées à l’IAM et aux actions utilisateurs

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
| Source | Destination | Port | Description |
|------|------------|------|-------------|
| Agents | Wazuh Server | 1514/TCP | Communication des logs |
| Agents | Wazuh Server | 1515/TCP | Enrôlement automatique |
| Client | Wazuh Server | 443/TCP | Accès Dashboard |
| Admin | Linux Client | 22/TCP | SSH |
| Admin | Windows Client | 3389/TCP | RDP |

---

## Prérequis
- Compte **AWS Learner Lab** actif
- 3 instances EC2 dans le même VPC
- Accès administrateur sur les machines
---

## Déploiement du serveur Wazuh

Sur l’instance **Ubuntu (Wazuh Server)** :

```bash
sudo apt update && sudo apt -y upgrade
curl -sO https://packages.wazuh.com/4.7/wazuh-install.sh
sudo bash wazuh-install.sh -a
