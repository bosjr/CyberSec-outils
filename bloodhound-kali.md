# installer **BloodHound CE sur Kali via Docker** en évitant le port 8080 et en utilisant **8181** à la place

## Procédure

1. Vérifie que Docker fonctionne sur Kali.
2. Crée un dossier de travail, par exemple `~/bloodhoundce`.
3. Télécharge le `docker-compose.yml` officiel de BloodHound CE  
4. Modifie le mapping de port pour remplacer `8080:8080` par `8181:8080`  
5. Lance la stack avec `docker compose up -dè

nano `docker-compose.yml`

6. Ouvre ensuite l’interface sur `http://localhost:8181/ui/login`  

## Commandes utiles

```bash
mkdir -p ~/bloodhoundce
cd ~/bloodhoundce
curl -L https://ghst.ly/getbhce -o docker-compose.yml
nano docker-compose.yml
docker compose up -d
```

```txt
Cette logique suit le déploiement Docker officiel de BloodHound CE, avec un simple remappage du port côté hôte  
```

## Vérification

Si tu veux confirmer qu’aucun autre service n’occupe déjà le port, utilise :

```bash
ss -ltnp | grep 8181
```

```text
L’intérêt de passer sur `8181` est de contourner le conflit avec un service déjà lié à `8080`  
```

## Accès et suite

Une fois la pile démarrée, connecte-toi avec le compte admin généré à l’installation, puis change le mot de passe au premier accès

Si tu as déjà un BloodHound local ou un autre service web sur Kali, cette méthode Docker reste la plus propre pour isoler l’application

## Notes

BloodHound CE utilise un couple email + mot de passe.

spam@example.com est l’adresse admin par défaut.

Le mot de passe est soit affiché à l’installation, soit récupérable via resetpwd ou logs
(kali㉿kali)-[~]
└─$ docker logs b29ea803cf18 2>&1 | grep "Initial Password Set To:"                                                                                      1 ⨯
Initial Password Set To:    j9JOnK35BDlH3_lBDfe4zgfWUi5jttO0    #


## ressources, liens

[bloodhound.specterops](https://bloodhound.specterops.io/get-started/quickstart/community-edition-quickstart)
[bloodhound.specterops](https://bloodhound.specterops.io/get-started/custom-installation)
[it-connect](https://www.it-connect.fr/chapitres/bloodhound-gestion-et-configuration-des-conteneurs-docker/)