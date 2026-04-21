# Write-up – FFUF (Version TryHackMe)

## 1. Introduction

FFUF (Fuzz Faster U Fool) est un outil de fuzzing web rapide utilisé pour découvrir des répertoires, fichiers, paramètres et virtual hosts.

Très utilisé en CTF pour remplacer ou compléter dirsearch/gobuster.

---

## 2. Objectif en Pentest

FFUF permet de :

* Découvrir des endpoints cachés
* Identifier des paramètres vulnérables
* Tester des virtual hosts

Workflow :

```text
Nmap → Port 80 → FFUF → Enum web → Exploitation
```

---

## 3. Installation

```bash
sudo apt install ffuf
```

---

## 4. Utilisation de base

```bash
ffuf -u http://<IP>/FUZZ -w wordlist.txt
```

### Explication

* `-u` : URL cible
* `FUZZ` : emplacement du fuzzing
* `-w` : wordlist

---

## 5. Commandes avancées (IMPORTANT TryHackMe)

### 5.1 Extensions

```bash
ffuf -u http://<IP>/FUZZ -w wordlist.txt -e .php,.txt,.html
```

---

### 5.2 Filtrer par code HTTP

```bash
ffuf -u http://<IP>/FUZZ -w wordlist.txt -mc 200,301,302
```

---

### 5.3 Exclure codes

```bash
ffuf -u http://<IP>/FUZZ -w wordlist.txt -fc 404
```

---

### 5.4 Filtrer par taille

```bash
ffuf -u http://<IP>/FUZZ -w wordlist.txt -fs 4242
```

---

### 5.5 Threads

```bash
ffuf -u http://<IP>/FUZZ -w wordlist.txt -t 50
```

---

### 5.6 Suivre redirections

```bash
ffuf -u http://<IP>/FUZZ -w wordlist.txt -r
```

---

### 5.7 Virtual Host fuzzing

```bash
ffuf -u http://<IP> -H "Host: FUZZ.example.com" -w wordlist.txt
```

---

### 5.8 Paramètre GET

```bash
ffuf -u "http://<IP>/index.php?user=FUZZ" -w wordlist.txt
```

---

### 5.9 POST fuzzing

```bash
ffuf -u http://<IP>/login -X POST -d "user=FUZZ&pass=test" -w wordlist.txt
```

---

### 5.10 Export résultats

```bash
ffuf -u http://<IP>/FUZZ -w wordlist.txt -o out.json
```

---

## 6. Cas pratique TryHackMe

### Étape 1 – Scan répertoires

```bash
ffuf -u http://10.10.10.10/FUZZ -w common.txt
```

---

### Étape 2 – Résultat

```text
/admin (301)
/login.php (200)
/backup.txt (200)
```

---

### Étape 3 – Enum avancée

```bash
ffuf -u http://10.10.10.10/FUZZ -w big.txt -e .php,.bak
```

---

## 7. Bonnes pratiques

### Filtrer les faux positifs

* taille (`-fs`)
* code (`-fc`)

---

### Tester plusieurs modes

* répertoires
* paramètres
* vhosts

---

### Utiliser bonnes wordlists

---

## 8. Erreurs fréquentes

### ❌ Pas de filtre

→ trop de bruit

### ❌ Mauvaise wordlist

→ résultats incomplets

---

## 9. Avantages / Inconvénients

### Avantages

* Très rapide
* Flexible
* Puissant

### Inconvénients

* Bruyant
* Nécessite réglages

---

## 10. Conclusion

FFUF est un outil incontournable pour l’énumération web en CTF.

Il permet un fuzzing précis et rapide des applications web.

---

## 11. Fuzzing avancé

### 11.1 Fuzzing avec headers personnalisés

```bash
ffuf -u http://<IP>/FUZZ -w wordlist.txt -H "Authorization: Bearer token"
```

---

### 11.2 Fuzzing avec cookies

```bash
ffuf -u http://<IP>/FUZZ -w wordlist.txt -b "PHPSESSID=123456"
```

---

### 11.3 Fuzzing avec authentification basique

```bash
ffuf -u http://<IP>/FUZZ -w wordlist.txt -u admin:password
```

---

### 11.4 Fuzzing récursif

```bash
ffuf -u http://<IP>/FUZZ -w wordlist.txt -recursion
```

Option utile :

```bash
-recursion-depth 2
```

---

### 11.5 Fuzzing multiple paramètres

```bash
ffuf -u "http://<IP>/page.php?user=FUZZ&role=FUZZ2" -w users.txt:FUZZ -w roles.txt:FUZZ2
```

---

### 11.6 Filtrer par mot clé (match)

```bash
ffuf -u http://<IP>/FUZZ -w wordlist.txt -mr "admin"
```

---

### 11.7 Exclure par mot clé

```bash
ffuf -u http://<IP>/FUZZ -w wordlist.txt -fr "error"
```

---

### 11.8 Mode silencieux (utile scripting)

```bash
ffuf -u http://<IP>/FUZZ -w wordlist.txt -s
```

---

### 11.9 Sortie CSV/JSON avancée

```bash
ffuf -u http://<IP>/FUZZ -w wordlist.txt -o result.json -of json
```

---

## 12. Mémo rapide

```bash
# Répertoires
ffuf -u http://<IP>/FUZZ -w wordlist.txt

# Extensions
ffuf -u http://<IP>/FUZZ -w wordlist.txt -e .php,.txt

# Filtrer codes
ffuf -mc 200,301,302

# Vhost
ffuf -H "Host: FUZZ.example.com"

# POST
ffuf -X POST -d "user=FUZZ"
```
