# 🛡️ Write-up – Scan de vulnérabilités avec OpenVAS (Greenbone)

---

## 1. Objectif

Réaliser un scan de vulnérabilités complet à l’aide de **OpenVAS / Greenbone Vulnerability Manager (GVM)** afin d’identifier :

* Services exposés
* Vulnérabilités connues (CVE)
* Mauvaises configurations
* Risques exploitables

---

## 2. Contexte

* Machine attaquante : Kali Linux
* Outil : **OpenVAS (GVM)**
* Cible : Machine vulnérable (ex : DVWA, Metasploitable, VM interne)
* Type de scan : authentifié ou non authentifié

---

## 3. Installation et initialisation (Kali)

### Sur la machine **attaquante**

```bash
sudo apt update
sudo apt install gvm -y
```

### Initialisation

```bash
sudo gvm-setup
```

> Cette étape :

* Télécharge les bases de vulnérabilités (NVT, CVE)
* Configure PostgreSQL
* Crée un utilisateur admin

### Vérification

```bash
sudo gvm-check-setup
```

---

## 4. Démarrage du service

```bash
sudo gvm-start
```

Accès interface web :

```url
https://127.0.0.1:9392
```

---

## 5. Configuration du scan

### 5.1 Création d’une cible

Menu :

```menu
Configuration → Targets
```

Paramètres :

* Nom : `Cible-THM`
* Hosts : `IP cible`
* Port list : `All TCP and Nmap top 100 UDP`

---

### 5.2 Création d’une tâche

Menu :

```menu
Scans → Tasks
```

Paramètres :

* Nom : `Scan complet`
* Scan Config : `Full and fast`
* Target : `Cible-THM`

---

## 6. Lancement du scan

Cliquer sur ▶️

⏳ Durée :

* 10 min à plusieurs heures selon :

  * Nombre de ports
  * Latence réseau
  * Type de scan

---

## 7. Analyse des résultats

### 7.1 Vue globale

* **High / Medium / Low**
* Score CVSS
* Nombre total de vulnérabilités

---

### 7.2 Exemple de vulnérabilités

#### 🔴 High

* **OpenSSH outdated**

  * Risque : exécution de code
  * Solution : mise à jour

* **Apache version vulnérable**

  * CVE associée
  * Exploit possible via Metasploit

---

#### 🟠 Medium

* TLS faible (TLS 1.0 activé)
* Mauvaise configuration HTTP headers

---

#### 🟢 Low

* Bannières visibles
* Informations système exposées

---

## 8. Analyse détaillée d’une vulnérabilité

Exemple :

```text
Vulnerability: Apache 2.4.x < 2.4.49 Path Traversal
```

* CVE : CVE-2021-41773
* Impact : accès fichiers système
* Exploit :

```bash
curl http://target/cgi-bin/.%2e/.%2e/.%2e/etc/passwd
```

---

## 9. Bonnes pratiques (Blue Team)

* Mettre à jour régulièrement les services
* Désactiver services inutiles
* Filtrer via firewall
* Implémenter IDS/IPS (ex : Suricata)
* Scanner régulièrement (hebdomadaire / mensuel)

---

## 10. Limites d’OpenVAS

* Faux positifs possibles
* Scan lent
* Détection ≠ exploitation
* Nécessite validation manuelle

---

## 11. Conclusion

OpenVAS est un outil essentiel pour :

* Audit de sécurité
* Gestion des vulnérabilités
* Priorisation des correctifs

Il s’intègre parfaitement dans une démarche SOC ou pentest en phase de **reconnaissance active et analyse de surface d’attaque**.

---

## 12. Améliorations possibles du lab

* Scan authentifié (SSH / SMB)
* Corrélation avec Splunk
* Exploitation via Metasploit
* Automatisation via CLI (`gvm-cli`)

---
