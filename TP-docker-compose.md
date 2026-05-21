# exemples docker compose

## Contexte SOC/pentest :

```yaml
version: '3'

services:
  dvwa:
    image: vulnerables/web-dvwa
    ports:
      - "8085:80"

  splunk:
    image: splunk/splunk:latest
    ports:
      - "8000:8000"
      - "8088:8088"
    environment:
      - SPLUNK_START_ARGS=--accept-license
      - SPLUNK_PASSWORD=Password123!

  kali:
    image: kalilinux/kali-rolling
    stdin_open: true
    tty: true
```

* **DVWA** → cible vulnérable
* **Kali** → machine d’attaque
* **Splunk** → collecte/analyse logs
* `docker compose up -d` → tout le labo démarre d’un coup

## scénario plus « architecture web » :

```yaml
version: '3'

services:
  reverse_proxy:
    image: nginx

  app:
    image: vulnerables/web-dvwa
```

Ici nginx sert de **reverse proxy** :

```text
Internet
   ↓
Nginx
   ↓
DVWA
```

Dans ce cas nginx devient utile pour :

* TLS/HTTPS
* filtrage
* logs centralisés
* tests WAF
* simulation d’infrastructure réelle

Pour un TP Docker débutant : nginx + DVWA montre simplement le multi-service.
Pour un TP cybersécurité : DVWA + Kali + SIEM est plus pertinent.
