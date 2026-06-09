# Rapport de TP : Découverte et Prise en Main de Docker & Git

## 1. Observations sur l'installation de Docker & Git

### Versions installées :
* **Docker** : Version 29.4.3
* **Git** : Version 2.48.1.windows.1

### Premier essai (`docker run hello-world`) :
Le premier lancement a échoué avec une erreur de connexion à l'API Docker (`failed to connect to the docker API...`). Cela s'explique par le fait que l'application Docker Desktop n'était pas encore complètement démarrée.

### Deuxième essai (Succès) :
Une fois Docker Desktop bien lancé et actif, la commande a parfaitement fonctionné.

---

## 2. TP 1 : Lancer ton propre conteneur (`hello-world`)

* **Ce que j'ai réalisé** :
  * J'ai exécuté la commande `docker run hello-world` dans mon terminal Windows.
  * J'ai ensuite listé l'ensemble des conteneurs (actifs et arrêtés) à l'aide de la commande `docker ps -a`.

* **Ce que j'ai observé** :
  * N'ayant pas l'image `hello-world` installée localement au départ, le client Docker l’a automatiquement téléchargée (*Pulling from library/hello-world*).
  * Une fois le téléchargement terminé, Docker a créé et exécuté un conteneur qui a affiché le message de bienvenue `"Hello from Docker!"`.
  * En exécutant `docker ps -a`, j'ai pu constater que ce conteneur s'est immédiatement arrêté après avoir affiché son message, affichant le statut `Exited (0)`. Docker lui a attribué un nom ( `amazing_bardeen`).
  * On remarque aussi la présence d'anciens conteneurs arrêtés (comme le projet applicatif `sensante`).

---

## 3. TP 2 : Lancer un serveur web et l’afficher

* **Ce que j'ai réalisé** :
  * J'ai déployé en arrière-plan un serveur web basé sur l'image officielle Nginx, en lui attribuant le nom `monteste` avec la commande :
    ```bash
    docker run -d -p 8080:80 --name monteste nginx
    ```
  * J'ai lié le port `8080` de ma machine locale Windows au port `80` interne du conteneur.
  * J'ai inspecté les logs du serveur avec `docker logs monteste`.
  * J'ai arrêté puis supprimé définitivement le conteneur avec `docker stop` et `docker rm`.

* **Ce que j'ai observé** :
  * Dès la validation de la commande `docker run`, Docker a renvoyé l'identifiant long du conteneur et m'a immédiatement redonné la main sur l'invite de commandes .
  * L'accès dans le navigateur à `http://localhost:8080` permet d'afficher la page d'accueil de Nginx.
  * La commande `docker logs` affiche les logs.
  * Les commandes `docker stop` et `docker rm` suppriment le conteneur, libérant ainsi le port réseau et le nom `monteste`.


---

## 4. TP 3 : Entrer dans un conteneur

* **Ce que j'ai réalisé** :
  * J'ai lancé un nouveau conteneur Nginx en arrière-plan nommé `montest2`.
  * J'ai utilisé la commande `docker exec -it montest2 sh` pour ouvrir un terminal de commandes (`sh`) directement à l'intérieur du conteneur actif.
  * Une fois à l'intérieur, j'ai exploré l'arborescence en listant la racine avec `ls`, puis en naviguant dans le dossier `/usr/share/nginx/html`.
  * Je suis ressortie du conteneur en tapant `exit`, puis j'ai supprimer le conteneur avec `docker rm -f montest2`.

* **Ce que j'ai observé** :
  * Dès la validation de la commande `docker exec`, l'invite de commandes Windows habituelle (`C:\Users\HP>`) disparaît pour laisser place au symbole **`#`**. Cela confirme que le terminal a basculé à l'intérieur de l'environnement Linux .
  * En exécutant `ls` à l'intérieur du dossier `/usr/share/nginx/html`, on observe que le conteneur contient la page `index.html` .
  * Si l'on tente d'exécuter une commande comme `docker rm` alors qu'on est encore à l'intérieur du conteneur (sous l'invite `#`), le système renvoie l'erreur `sh: docker: not found`
---

## 5. TP 4 : Créer ta propre image avec un Dockerfile

* **Ce que j'ai réalisé** :
  * J'ai configuré un espace de travail en créant le dossier `montp` 
  * J'ai développé une page web d'accueil personnalisée dans un fichier `index.html`.
  * J'ai rédigé un script `Dockerfile` composé de deux instructions fondamentales :
    ```dockerfile
    FROM nginx
    COPY index.html /usr/share/nginx/html/index.html
    ```
  * J'ai compilé ce script pour construire une nouvelle image locale `mon-site` via la commande `docker build -t mon-site .`.
  * J'ai lancé un conteneur nommé `monsite` basé sur cette image personnalisée sur le port `8085` (`docker run -d -p 8085:80 --name Cake monsite mon-site`).

* **Ce que j'ai observé** :
  * Dans le terminal de VS Code, la commande `docker build` s'est exécutée étape par étape pour fabriquer mon image.
  * En tapant `http://localhost:8085` dans mon navigateur, ma page personnalisée s'est affichée directement avec le titre : *"Ma premiére image Docker !"*.
  * Le fichier `index.html` d'origine de Nginx a bien été remplacé par le mien. 



---

## 6. TP 5 : Utiliser Docker Compose

* **Ce que j'ai réalisé** :
  * Dans un nouveau dossier `montpe`, j'ai créé un fichier de configuration nommé `docker-compose.yml` pour y définir un service web basé sur l'image `nginx:alpine`.
  * J'ai démarré l'environnement en arrière-plan avec la commande `docker compose up -d`.
  * J'ai arrêté  à l'aide de `docker compose down`.

* **Ce que j'ai observé** :
  * La commande `docker compose up -d` a automatiquement géré le téléchargement de l'image et la création du conteneur sans avoir à saisir de longues lignes de commandes.
  * En ouvrant `http://localhost:8080` dans mon navigateur, la page d'accueil de Nginx s'est affichée avec succès, prouvant que la redirection du port `8080:80` a fonctionné.
  * Après avoir exécuté `docker compose down`, le conteneur a instantanément disparu.


---

## 7. Initiation Git et GitHub
*Installation, configuration et mise en ligne du dépôt de suivi.*
### Suivi de la démarche Git :
1. **Création & Liaison** : Initialisation du répertoire local et connexion au dépôt distant GitHub `onboarding-dgs`.
2. **Suivi des modifications** : Utilisation de `git add .` pour indexer le fichier `notes.md` corrigé et la capture d'écran `capture-defi.png`.
3. **Validation locale** : Enregistrement des changements avec `git commit -m "..."` pour dater et décrire la fin du TP.
4. **Synchronisation** : Exécution de `git push` pour téléverser l'intégralité des livrables sur GitHub pour correction.
lien du depot: https://github.com/diarra167/onboarding-dgs.git
### Livrable — Défi : Connexion à la base de données

* **Valeurs erronées :** DB_HOST=database, DB_PORT=1234 (L'application cherchait un mauvais nom d'hôte et un port incorrect).
* **Valeurs correctes :** DB_HOST=db, DB_PORT=5432 (Utilisation du nom du service Docker et du port standard PostgreSQL).
* **Explication :** Dans Docker Compose, les conteneurs partagent un réseau où le nom du service sert d'adresse DNS, et PostgreSQL écoute par défaut sur le port 5432.

