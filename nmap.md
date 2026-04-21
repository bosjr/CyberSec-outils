# Write-up – Nmap (Version TryHackMe)

## 1. Introduction

Nmap (Network Mapper) est l’outil de référence pour le scan réseau. Il permet de découvrir des hôtes, identifier les ports ouverts, détecter les services et analyser les systèmes.

Dans un contexte TryHackMe / CTF, Nmap est utilisé après un scan rapide (RustScan) ou directement pour une reconnaissance complète.

---

## 2. Objectif en Pentest

Nmap permet de :

* Identifier les ports ouverts
* Détecter les services et versions
* Identifier le système d’exploitation
* Lancer des scripts d’énumération

Workflow :

```text
Recon → Nmap → Enumération → Exploitation
```

---

## 3. Installation

### Kali Linux

```bash
sudo apt install nmap
```

### Vérification

```bash
nmap --version
```

---

## 4. Utilisation de base

```bash
nmap <IP>
```

Exemple :

```bash
nmap 10.10.10.10
```

Résultat :

* Ports principaux scannés
* État (open/closed/filtered)

---

## 5. Types de scans essentiels

### 5.1 Scan TCP SYN (rapide)

```bash
nmap -sS <IP>
```

* Scan furtif (semi-ouvert)
* Nécessite privilèges root

---

### 5.2 Scan TCP Connect

```bash
nmap -sT <IP>
```

* Scan complet
* Utilisé sans privilèges root

---

### 5.3 Scan UDP

```bash
nmap -sU <IP>
```

* Lent mais important
* Services critiques (DNS, SNMP)

---

## 6. Détection avancée

### 6.1 Détection de version

```bash
nmap -sV <IP>
```

---

### 6.2 Détection OS

```bash
nmap -O <IP>
```

---

### 6.3 Scan agressif

```bash
nmap -A <IP>
```

Inclut :

* OS detection
* Version detection
* Scripts
* Traceroute

---

## 7. Gestion des ports

### 7.1 Scanner ports spécifiques

```bash
nmap -p 22,80,443 <IP>
```

---

### 7.2 Scanner tous les ports

```bash
nmap -p- <IP>
```

---

### 7.3 Ports rapides

```bash
nmap -F <IP>
```

---

## 8. Scripts NSE (Nmap Scripting Engine)

Les scripts NSE permettent d’automatiser l’énumération et la détection de vulnérabilités.

---

### 8.1 Scripts par défaut

```bash
nmap -sC <IP>
```

---

### 8.2 Scripts par catégorie

```bash
nmap --script vuln <IP>
```

Catégories utiles :

* vuln : détection de vulnérabilités
* auth : authentification
* brute : brute force
* discovery : découverte
* safe : non intrusif

---

### 8.3 Scripts ciblés par service (IMPORTANT CTF)

#### SMB

```bash
nmap -p445 --script smb-enum-shares,smb-enum-users <IP>
```

Objectif :

* Lister les partages
* Identifier les utilisateurs

---

#### FTP

```bash
nmap -p21 --script ftp-anon,ftp-bounce <IP>
```

Objectif :

* Vérifier accès anonyme
* Tester mauvaises configurations

---

#### HTTP

```bash
nmap -p80 --script http-enum,http-title <IP>
```

Objectif :

* Découverte de pages
* Identification du serveur web

---

#### SSH

```bash
nmap -p22 --script ssh-auth-methods,ssh-hostkey <IP>
```

Objectif :

* Méthodes d’authentification
* Clés serveur

---

#### DNS

```bash
nmap -p53 --script dns-brute <IP>
```

Objectif :

* Découverte de sous-domaines

---

### 8.4 Combinaison de scripts

```bash
nmap -p80 --script http-* <IP>
```

---

### 8.5 Exemple complet

```bash
nmap -p21,22,80,445 --script vuln,default <IP>
```

---

### 8.6 Localisation des scripts

```bash
/usr/share/nmap/scripts/
```

---

### 8.7 Mise à jour des scripts

```bash
nmap --script-updatedb
```

---

### 8.2 Scripts spécifiques

```bash
nmap --script vuln <IP>
```

---

### 8.3 Exemple web

```bash
nmap --script http-enum <IP>
```

---

## 9. Optimisation des scans

### 9.1 Vitesse

```bash
nmap -T4 <IP>
```

* T0 → lent
* T5 → très rapide (bruyant)

---

### 9.2 Verbose

```bash
nmap -v <IP>
```

---

### 9.3 Scan sans ping

```bash
nmap -Pn <IP>
```

---

## 10. Export des résultats

```bash
nmap -oN scan.txt <IP>
```

Formats :

```bash
-oN  (normal)
-oX  (XML)
-oG  (grepable)
```

---

## 11. Cas pratique TryHackMe

### Étape 1 – Scan complet

```bash
nmap -p- 10.10.10.10
```

---

### Étape 2 – Scan ciblé

```bash
nmap -p22,80 -sV -sC 10.10.10.10
```

---

### Étape 3 – Enumération

* Port 80 → web
* Port 22 → SSH

---

## 12. Bonnes pratiques

### Toujours scanner tous les ports

```bash
-p-
```

---

### Séparer les phases

1. Découverte
2. Analyse détaillée

---

### Adapter la vitesse

* CTF : rapide
* Réel : discret

---

## 13. Erreurs fréquentes

### ❌ Scanner uniquement ports standards

→ perte d’informations

### ❌ Aller trop vite

→ faux négatifs

### ❌ Ignorer UDP

→ services critiques manqués

---

## 14. Avantages / Inconvénients

### Avantages

* Très complet
* Flexible
* Puissant

### Inconvénients

* Peut être lent
* Bruyant selon config

---

## 15. Conclusion

Nmap est un outil incontournable en pentest et CTF.

Une bonne maîtrise permet de gagner un temps important dans la phase de reconnaissance.

---

## 16. Mémo rapide

```bash
# Scan simple
nmap <IP>

# Tous les ports
nmap -p- <IP>

# Version + scripts
nmap -sV -sC <IP>

# Scan agressif
nmap -A <IP>

# Sans ping
nmap -Pn <IP>

# Export
nmap -oN scan.txt <IP>
```
