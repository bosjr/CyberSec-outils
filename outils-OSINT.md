# OSINT (Open Source Intelligence)

## 1. Introduction

Cette série d’exercices OSINT a pour objectif de développer des compétences en collecte et analyse de renseignement en sources ouvertes.

Public cible :

* Analyste SOC
* Threat Intelligence
* Red Team
* Enquêteur

---

## 2. Cartographie de la surface d’attaque

### 2.1 Identification des services exposés

**Objectif :** Identifier les services accessibles publiquement.

**Outil :** Shodan : https://www.shodan.io/  

* Recherche par IP
* Identification des ports ouverts
* Détection de services mal configurés

---

### 2.2 Reconnaissance automatisée

**Outil :** Amass : https://github.com/owasp-amass/amass  

**Objectif :** Découverte de domaines et sous-domaines

* Reconnaissance passive
* Reconnaissance active

---

### 2.3 Analyse des certificats TLS

**Outil :** Censys : https://search.censys.io/  

**Objectif :**

* Identifier les domaines liés
* Cartographier l’infrastructure

---

### 2.4 Détection de stockage cloud exposé

**Outil :** Grayhat Warfare : https://buckets.grayhatwarfare.com/  

**Objectif :**

* Identifier des buckets S3 publics
* Détecter des fuites de données

---

## 3. Ingénierie sociale et phishing

### 3.1 Vérification de compromission

**Outil :** Have I Been Pwned : https://haveibeenpwned.com/  

**Objectif :**

* Vérifier si un email est compromis

---

### 3.2 Investigation d’identité

**Outil :** Holehe : https://github.com/megadose/holehe  

**Objectif :**

* Associer un email à des comptes en ligne

---

### 3.3 Analyse d’URL suspectes

**Outil :** URLScan : https://urlscan.io/  

**Objectif :**

* Analyser un site web suspect
* Détecter du phishing

---

### 3.4 Détection de faux comptes

**Outil :** Botometer : https://botometer.osome.iu.edu/  

**Objectif :**

* Identifier des bots sur les réseaux sociaux

---

## 4. Veille réseau et Dark Web

### 4.1 Threat Intelligence

**Outil :** AlienVault OTX : http://otx.alienvault.com  

**Objectif :**

* Suivi des menaces
* IOC (Indicators of Compromise)

---

### 4.2 Surveillance DNS

**Outil :** SecurityTrails :  https://securitytrails.com/  

**Objectif :**

* Historique DNS
* Changements de domaine

---

### 4.3 Recherche Dark Web

**Outil :** OnionSearch : https://github.com/megadose/OnionSearch  

**Objectif :**

* Recherche sur TOR

---

## 5. Énumération de domaines

### 5.1 Sous-domaines

**Outil :** Subfinder : https://github.com/projectdiscovery/subfinder  

**Objectif :**

* Découverte rapide

---

### 5.2 Bruteforce DNS

**Outil :** MassDNS : https://github.com/blechschmidt/massdns  

**Objectif :**

* Résolution massive

---

### 5.3 Détection de typosquatting

**Outil :** DNSTwist : https://github.com/elceef/dnstwist  

**Objectif :**

* Détection de domaines frauduleux

---

### 5.4 Certificats SSL

**Outil :** CertSpotter : https://github.com/SSLMate/certspotter  

**Objectif :**

* Nouveaux certificats émis

---

## 6. Analyse de code et dépôts

### 6.1 Recherche de secrets

**Outil :** GitLeaks : https://github.com/gitleaks/gitleaks  

**Objectif :**

* Détection de clés API

---

### 6.2 GitHub Dorking

**Objectif :**
**Outil :** GirDorker : https://github.com/obheda12/GitDorker  

* Recherche de données sensibles

---

### 6.3 Surveillance de dépôts

**Outil :** Repo Hunt : 

**Objectif :**

* Détection d’activités suspectes

---

### 6.4 Analyse des développeurs

**Outil :** GHunt : https://github.com/mxrch/GHunt  

**Objectif :**

* Profiling développeur

---

## 7. Bonnes pratiques
* ♻️ Share with someone who may need it
Follow Rajneesh Gupta for more such resources! : https://www.linkedin.com/in/rajneeshgupta01/
* Croiser les sources
* Automatiser les recherches
* Documenter les résultats
* Vérifier la légalité des actions

---

## 8. Conclusion

L’OSINT est une compétence clé en cybersécurité permettant d’identifier des informations critiques sans interaction directe avec la cible.  

Ces outils permettent de couvrir l’ensemble du cycle de renseignement : de la collecte à l’analyse avancée.  

https://www.linkedin.com/in/bosjr  