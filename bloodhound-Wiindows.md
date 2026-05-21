# installer **BloodHound Community Edition** sur Windows 11 : passer par **Docker Desktop** puis lancer l’installation avec le **BloodHound CLI**. SpecterOps indique que cette méthode installe BloodHound CE via un `docker-compose.yml`, avec une interface accessible ensuite en local sur `http://localhost:8080/ui/login`  

## Prérequis

- Windows 11 avec **Docker Desktop** installé et démarré. SpecterOps recommande Docker Desktop pour exécuter les conteneurs BloodHound CE en local
- Au moins **8 Go de RAM**, 4 cœurs CPU et 10 Go d’espace disque pour une installation standard

## Installation

1. Installe Docker Desktop sur Windows 11 et vérifie qu’il fonctionne.
2. Télécharge la version Windows du **BloodHound CLI** depuis la page officielle SpecterOps
3. Décompresse l’archive dans un dossier de ton choix.
4. Ouvre PowerShell dans ce dossier et lance la commande d’installation adaptée à Windows :

   ```powershell
   .\bloodhound-cli.exe install
   ```

   La documentation officielle décrit le même principe d’installation via le CLI, qui télécharge les images Docker et génère la configuration nécessaire  

5. Attends la fin du déploiement et note le mot de passe admin affiché dans la console

## Connexion

- Ouvre ensuite [http://localhost:8080/ui/login](http://localhost:8080/ui/login) dans ton navigateur  
- Connecte-toi avec l’utilisateur `admin` et le mot de passe généré à l’installation  
- Au premier accès, changez immédiatement le mot de passe

## Alternative Docker Compose

Pour éviter le CLI, la documentation et des guides techniques indiquent aussi une installation directement avec Docker Compose sur Windows après récupération du `docker-compose.yml`  

## Pour l’usage AD

BloodHound sert ensuite à importer des données collectées depuis Active Directory, par exemple avec **SharpHound**, puis à analyser les chemins d’attaque dans l’interface  

## Liens

[bloodhound.specterops](https://bloodhound.specterops.io/get-started/quickstart/community-edition-quickstart)
[it-connect](https://www.it-connect.fr/chapitres/bloodhound-installation-avec-docker/)
[it-connect](https://www.it-connect.fr/chapitres/bloodhound-gestion-et-configuration-des-conteneurs-docker/)

## pas-à-pas pour Windows 11

Oui. Sur **Windows 11**, la procédure la plus simple pour BloodHound CE avec ton port libre `8181` est la suivante : lance la stack Docker, récupère le mot de passe initial affiché dans la console, puis connecte-toi avec l’email admin demandé à l’écran.

1. Installe **Docker Desktop** sur Windows 11 et vérifie qu’il est démarré.
2. Crée un dossier de travail, par exemple `C:\bloodhoundce`.
3. Télécharge le fichier officiel `docker-compose.yml` de BloodHound CE.

```powershell
curl -L https://ghst.ly/getbhce -o docker-compose.yml
```

4. Remplace le port exposé `8080:8080` par `8181:8080` dans docker-compose.yml.
5. Ouvre PowerShell dans ce dossier et lance :

   ```powershell
   docker compose up
   ```

   Le mot de passe initial s’affiche pendant le démarrage du conteneur

## Accès

- Ouvre ensuite [http://127.0.0.1:8181](http://127.0.0.1:8181) ou l’URL indiquée par la stack BloodHound CE.
- Utilise l’**email** demandé par l’interface BloodHound CE, puis le **mot de passe initial** affiché dans la console au premier démarrage sinon dans docker compose logs.
- Au premier accès, BloodHound te demandera de définir ou de réinitialiser le mot de passe.

## Vérification du port

Si `8181` est déjà pris, vérifie avec :

```powershell
netstat -ano | findstr :8181
```

Sinon, garde `8181` comme port hôte pour éviter le conflit avec `8080`.

La logique est :

- Docker Desktop.
- `docker compose up`.
- récupère le mot de passe dans les logs.
- connexion sur le port `8181`.
