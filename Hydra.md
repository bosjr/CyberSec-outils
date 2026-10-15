## Documentation rapide — THC Hydra

Hydra est un outil de brute force réseau permettant de tester des identifiants sur de nombreux protocoles (HTTP, SSH, FTP, SMB, etc.).

---

# 1. Syntaxe générale

```bash
hydra [options] target service [module-options]
```

---

# 2. Options principales

## Identifiants

```text
-l LOGIN        # utilisateur unique
-L FILE         # liste utilisateurs
-p PASSWORD     # mot de passe unique
-P FILE         # wordlist mots de passe
```

---

## Cible

```text
-s PORT         # port personnalisé
```

Ex :

```bash
-s 42001
```

---

## Contrôle de l’attaque

```text
-t N            # threads (par défaut 16)
-f              # stop après premier succès
-F              # stop par cible
-v / -vv        # mode verbeux
```

---

# 3. Modules principaux

## HTTP POST

```text
http-post-form
```

Ex DVWA :

```bash
hydra -l admin -P pass.txt 127.0.0.1 http-post-form \
"/login.php:user=^USER^&pass=^PASS^:F=incorrect"
```

---

## HTTP GET

```text
http-get-form
```

Ex DVWA brute force :

```bash
hydra -l admin -P pass.txt 127.0.0.1 http-get-form \
"/vulnerabilities/brute/:username=^USER^&password=^PASS^:F=incorrect"
```

---

# 4. Format des modules HTTP

```text
URL:POST_DATA:FAIL/SUCCESS_CONDITION
```

Ex :

```text
F=incorrect
S=Welcome
```

---

# 5. Headers (cookies, auth)

Dans THC Hydra :

```text
:H=Cookie\: PHPSESSID=xxxx; security=low
```

⚠️ `:` et `;` doivent être échappés

---

# 6. Exemples utiles

## SSH

```bash
hydra -l root -P pass.txt ssh://192.168.1.10
```

---

## FTP

```bash
hydra -L users.txt -P pass.txt ftp://192.168.1.10
```

---

## HTTP form login

```bash
hydra -l admin -P pass.txt 192.168.1.10 http-post-form \
"/login.php:user=^USER^&pass=^PASS^:F=Invalid"
```

---

# 7. Options avancées

## Stop rapide

```text
-f
```

## Threads

```text
-t 4
```

## Timeout

```text
-w 30
```

---

# 8. Bonnes pratiques

* toujours capturer requête avec Burp Suite
* identifier correctement :

  * paramètres POST/GET
  * cookies
  * message d’erreur exact
* éviter `S=` si page ambiguë → préférer `F=`

---

# 9. Erreurs fréquentes

| Problème               | Cause                   |
| ---------------------- | ----------------------- |
| tous mots valides      | mauvais filtre          |
| aucun résultat         | mauvais endpoint        |
| login invalide partout | cookie absent           |
| syntaxe cassée         | `:` ou `;` non échappés |

---

# 10. Résumé

Hydra =

* ✔ rapide
* ✔ multi-protocoles
* ❌ fragile sur applications web modernes
* ❌ sensible aux tokens / delays

---

# Conclusion

Hydra est efficace pour :

* services réseau simples
* formulaires web basiques (DVWA low)

Mais devient limité avec :

* tokens dynamiques
* delays (medium)
* protections applicatives

---

## Comparatif outils.

---

# 11. Comparatif outils de brute force

## THC Hydra

### Points forts Hydra

* Très rapide (multi-thread)
* Support de nombreux protocoles (SSH, FTP, HTTP, SMB…)
* Léger et scriptable en CLI
* Bon pour services réseau simples

### Limites

* Sensible aux pages web dynamiques (tokens, CSRF)
* Détection fragile (`F=` / `S=` / `L=`)
* Faux positifs possibles si mauvaise configuration
* Peu adapté aux applications modernes

---

## Burp Suite

### Points forts Burp

* Analyse complète du trafic HTTP
* Très fiable pour applications web
* Gestion des sessions, cookies, tokens
* Intruder + Repeater + Proxy intégrés
* Visualisation claire des réponses

### Limites

* Plus lent que Hydra
* Moins automatisé en CLI
* Version gratuite limitée

---

## Autres outils utiles

### ffuf (fuzzing HTTP)

* Très rapide
* Flexible sur endpoints web
* Bon pour discovery + brute force API

---

# 12. Choix de l’outil selon le contexte

| Situation           | Outil recommandé |
| ------------------- | ---------------- |
| SSH / FTP / SMB     | Hydra            |
| DVWA low            | Hydra ou Burp    |
| DVWA medium/high    | Burp             |
| API / endpoints web | Burp / ffuf      |
| Bug bounty / réel   | Burp + ffuf      |

---

# 13. Résumé opérationnel

* Hydra → brute force réseau rapide
* Burp → analyse et exploitation web fiable
* ffuf → fuzzing et discovery HTTP

---

# Conclusion outils

Hydra reste utile, mais dans les environnements web modernes :
👉 Burp Suite devient l’outil de référence pour fiabilité et contrôle.

---
