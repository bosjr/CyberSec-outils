# Configuration de FoxyProxy avec Burp Suite et interception de `127.0.0.1`

## 1. Objectif

L'objectif est de configurer **FoxyProxy** dans Firefox afin de faire passer les requêtes HTTP de **DVWA**, accessible localement sur :

```text
http://127.0.0.1:4280
```

par **Burp Suite**, afin de pouvoir les intercepter et les modifier.

Architecture :

```text
Firefox
   │
   │ HTTP
   ▼
FoxyProxy
   │
   │ 127.0.0.1:8080
   ▼
Burp Suite
   │
   │
   ▼
DVWA
127.0.0.1:4280
```

---

## 2. Configurer le proxy dans Burp Suite

Dans Burp Suite :

### Proxy → Proxy settings → Proxy listeners

Vérifier qu'un listener est actif sur :

```text
127.0.0.1:8080
```

Configuration typique :

```text
Interface : 127.0.0.1
Port      : 8080
```

Le listener doit être en état **Running**.

---

## 3. Installer et épingler FoxyProxy

Dans Firefox, installer l'extension **FoxyProxy**.

Une fois installée :

1. Cliquer sur l'icône **Extensions**.
2. Repérer FoxyProxy.
3. Choisir **Épingler dans la barre d'outils**.

L'icône FoxyProxy sera alors directement accessible.

---

## 4. Créer le proxy Burp

Dans FoxyProxy, créer un nouveau proxy.

Paramètres :

```text
Type de proxy : HTTP
Adresse        : 127.0.0.1
Port           : 8080
```

Donner par exemple au profil le nom :

```text
Burp
```

Enregistrer la configuration.

---

## 5. Le problème de `127.0.0.1`

Un problème fréquent apparaît lorsque l'on veut intercepter :

```text
http://127.0.0.1:4280
```

Le navigateur peut considérer `localhost` et `127.0.0.1` comme des adresses qui ne doivent pas passer par le proxy.

Dans ce cas, Firefox accède directement à DVWA :

```text
Firefox ───────────────► 127.0.0.1:4280
```

et Burp ne voit aucune requête.

Le résultat est alors :

```text
Burp Suite
   │
   └── aucune requête
```

---

## 6. Solution simple : utiliser le proxy pour toutes les URL

Pour un environnement de laboratoire, la solution la plus simple est de sélectionner dans FoxyProxy le mode permettant d'utiliser le proxy pour **toutes les URL**.

Selon la version de FoxyProxy, le libellé peut être légèrement différent, par exemple :

```text
Proxy Burp for all URLs
```

ou une option équivalente permettant d'utiliser le profil Burp globalement.

Ainsi :

```text
Firefox
   │
   ▼
127.0.0.1:8080
   │
   ▼
Burp
   │
   ▼
127.0.0.1:4280
```

---

## 7. Vérifier l'interception

Dans Burp :

### Proxy → Intercept

Activer :

```text
Intercept ON
```

Puis dans Firefox ouvrir :

```text
http://127.0.0.1:4280
```

Burp doit intercepter une requête similaire à :

```http
GET / HTTP/1.1
Host: 127.0.0.1:4280
User-Agent: Mozilla/5.0 ...
Accept: text/html,...
```

La requête peut alors être modifiée avant d'être envoyée à DVWA.

---

## 8. Vérification dans HTTP history

Même si **Intercept** est désactivé, les requêtes peuvent être consultées dans :

### Proxy → HTTP history

Rechercher notamment :

```text
Host: 127.0.0.1:4280
```

Si les requêtes apparaissent dans HTTP history, le trafic passe bien par Burp.

---

## 9. Méthode avec un pattern FoxyProxy

Si l'on ne souhaite pas faire passer tout le trafic Firefox par Burp, FoxyProxy peut fonctionner avec des règles.

Créer une règle correspondant à :

```text
127.0.0.1
```

ou, selon la syntaxe proposée par la version utilisée :

```text
*127.0.0.1*
```

L'objectif est d'obtenir :

```text
127.0.0.1:4280
       │
       ▼
    FoxyProxy
       │
       ▼
127.0.0.1:8080
       │
       ▼
     Burp
```

Cette méthode est préférable lorsque l'on souhaite intercepter uniquement DVWA.

---

## 10. Attention à `localhost`

Il existe une différence entre :

```text
http://127.0.0.1:4280
```

et :

```text
http://localhost:4280
```

Si l'application est accessible avec les deux noms, prévoir éventuellement deux règles :

```text
*127.0.0.1*
*localhost*
```

Cela évite qu'une requête utilisant `localhost` contourne la configuration destinée à `127.0.0.1`.

---

## 11. Diagnostic si Burp n'intercepte rien

### Vérifier DVWA

Tester directement :

```bash
curl http://127.0.0.1:4280
```

Si DVWA répond, le service est disponible.

### Vérifier Burp

Le listener doit être actif :

```text
127.0.0.1:8080
```

### Vérifier FoxyProxy

Le profil **Burp** doit être actif.

### Vérifier le contournement

Si Firefox accède à DVWA mais que Burp ne reçoit rien, le trafic contourne probablement le proxy.

Dans ce cas, utiliser temporairement le mode :

```text
Proxy Burp for all URLs
```

pour confirmer le diagnostic.

---

## 12. Schéma de fonctionnement

### Sans FoxyProxy

```text
Firefox
   │
   └──────────────────► DVWA :4280
                         │
                         └── aucune interception
```

### Avec FoxyProxy correctement configuré

```text
Firefox
   │
   ▼
FoxyProxy
   │
   ▼
Burp :8080
   │
   ▼
DVWA :4280
```

Burp devient alors un proxy intermédiaire :

```text
Client ──► Burp ──► Serveur
          ▲
          │
       modification
       / inspection
```

---

## 13. Points à retenir

| Élément      | Valeur                      |
| ------------ | --------------------------- |
| Application  | DVWA                        |
| Adresse DVWA | `127.0.0.1:4280`            |
| Proxy        | Burp Suite                  |
| Adresse Burp | `127.0.0.1:8080`            |
| Extension    | FoxyProxy                   |
| Interception | Burp → Proxy → Intercept    |
| Historique   | Burp → Proxy → HTTP history |

### Problème principal

```text
127.0.0.1 peut être exclu du proxy
```

### Solution

Configurer FoxyProxy pour que les requêtes destinées à :

```text
127.0.0.1:4280
```

passent explicitement par :

```text
127.0.0.1:8080
```

### Résultat

```text
Firefox
   │
   │ http://127.0.0.1:4280
   ▼
FoxyProxy
   │
   │ 127.0.0.1:8080
   ▼
Burp Suite
   │
   ▼
DVWA
```

Cette configuration permet ensuite d'utiliser Burp pour analyser les requêtes DVWA et réaliser les exercices de sécurité web du laboratoire.
