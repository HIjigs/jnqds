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

Vérification des services
# Vérifier l’état des services Wazuh
sudo systemctl status wazuh-manager
sudo systemctl status wazuh-indexer
sudo systemctl status wazuh-dashboard

# Les services doivent être en état : active (running)

Accès au Dashboard
https://<IP_DU_SERVEUR_WAZUH>


Le dashboard permet :

La gestion des agents

La visualisation des alertes

L’analyse des événements de sécurité

Le threat hunting

Enrôlement des agents
Client Linux
Wazuh Dashboard
→ Agents Management
→ Deploy new agent
→ Sélectionner Linux
→ Copier et exécuter les commandes proposées

Client Windows
# Exécuter dans PowerShell (Administrateur)
# Les commandes sont fournies par le Dashboard


Vérifier que le service est actif :

Services → Wazuh Agent → Running

Scénarios de démonstration
SIEM – Linux
# Tentatives SSH échouées
ssh fakeuser@IP_LINUX_CLIENT

# Élévation de privilèges
sudo su

# Modification de fichier sensible (FIM)
echo "test" | sudo tee -a /etc/passwd

EDR – Windows
# Création d’un utilisateur local
net user labuser P@ssw0rd! /add
net localgroup administrators labuser /add


Autres actions :

Tentatives RDP avec mauvais mot de passe

(Optionnel) Installation de Sysmon pour enrichir l’EDR

Visualisation des alertes
Wazuh Dashboard
→ Security Events
→ Threat Hunting


Filtres utiles :

Agent (Linux / Windows)

SSH / authentication_failed

Windows Security

User added / Group changed

Sysmon (process creation, network connection)

Livrables attendus
1. Capture : Agents Linux & Windows actifs
2. Alerte SSH (Linux)
3. Alerte Failed Logon (Windows)
4. Alerte User / Group change (Windows)
5. Schéma VPC + instances + Security Groups
6. Rapport :
   - SIEM vs EDR
   - IAM / PAM
   - Threat hunting (3 requêtes)

Conclusion

Ce TP fournit une approche pratique d’un SOC moderne, combinant SIEM et EDR
dans un environnement Cloud réaliste.


---

Si tu veux, je peux maintenant :
- 🔹 transformer ce README en **script TP étudiant**
- 🔹 fournir un **README formateur**
- 🔹 ajouter un **schéma réseau en ASCII ou Mermaid**
- 🔹 faire une **version très courte (1 page)**
