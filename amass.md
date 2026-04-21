# Write-up – Amass (Version TryHackMe)

## 1. Introduction

Amass est un outil d’OSINT et d’énumération DNS utilisé pour cartographier les domaines, sous-domaines et infrastructures associées.

Très utilisé en reconnaissance pour les pentests web et bug bounty.

---

## 2. Objectif en Pentest

Amass permet de :

* Découvrir des sous-domaines
* Identifier des assets exposés
* Cartographier l’infrastructure

Workflow :

```text
OSINT → Amass → Sous-domaines → Scan → Exploitation
```

---

## 3. Installation

```bash
sudo apt install amass
```

---

## 4. Modes principaux

### 4.1 Enumération passive

```bash
amass enum -passive -d example.com
```

* utilise des sources publiques
* discret

---

### 4.2 Enumération active

```bash
amass enum -active -d example.com
```

* effectue des requêtes DNS actives
* plus bruyant

---

### 4.3 Bruteforce DNS

```bash
amass enum -brute -d example.com -w wordlist.txt
```

---

## 5. Options importantes

### Domaine

```bash
-d example.com
```

---

### Sortie fichier

```bash
-o result.txt
```

---

### Résolution DNS

```bash
-ip
```

---

### Threads

```bash
-max-dns-queries 50
```

---

## 6. Commandes avancées

### 6.1 Enum complète

```bash
amass enum -active -brute -d example.com -w wordlist.txt -o result.txt
```

---

### 6.2 Visualisation graphique

```bash
amass viz -d3 -d example.com
```

---

### 6.3 Utilisation d’une config

```bash
amass enum -config config.ini -d example.com
```

---

## 7. Cas pratique TryHackMe

### Étape 1 – Enum passive

```bash
amass enum -passive -d target.com
```

---

### Étape 2 – Résultat

```text
api.target.com
admin.target.com
dev.target.com
```

---

### Étape 3 – Scan ports

```bash
nmap -iL result.txt
```

---

## 8. Bonnes pratiques

* commencer en passif
* utiliser plusieurs wordlists
* corréler avec autres outils (subfinder, dnsx)

---

## 9. Erreurs fréquentes

### ❌ uniquement passif

→ résultats incomplets

### ❌ pas de bruteforce

→ sous-domaines manqués

---

## 10. Avantages / Inconvénients

### Avantages

* très complet
* OSINT puissant
* cartographie réseau

### Inconvénients

* lent
* nécessite config

---

## 11. Fuzzing / Enum avancée

### 11.1 Résolution + IP

```bash
amass enum -d example.com -ip
```

---

### 11.2 Sortie JSON

```bash
amass enum -d example.com -json out.json
```

---

### 11.3 Enum multi-domaines

```bash
amass enum -df domains.txt
```

---

### 11.4 Pipeline avec httpx

```bash
amass enum -d example.com -o subs.txt
cat subs.txt | httpx
```

---

### 11.5 Combinaison avec massdns

```bash
amass enum -brute -d example.com -rf resolvers.txt
```

---

## 12. Mémo rapide

```bash
# Passif
amass enum -passive -d example.com

# Actif
amass enum -active -d example.com

# Bruteforce
amass enum -brute -w wordlist.txt

# Full
amass enum -active -brute -d example.com
```
