# DOCKER


## Introduction

### Qu'est-ce que Docker ?

un projet open-source qui automatise le déploiement d'applications logicielles dans des conteneurs en fournissant une couche supplémentaire d'abstraction et d'automatisation de la virtualisation au niveau du système d'exploitation sous Linux.

Docker est un outil qui permet aux développeurs, administrateurs système, etc., de déployer facilement leurs applications dans un environnement isolé (appelé conteneur ) pour s'exécuter sur le système d'exploitation hôte, par exemple Linux. L'avantage principal de Docker est qu'il permet aux utilisateurs d' empaqueter une application et toutes ses dépendances dans une unité standardisée pour le développement logiciel. Contrairement aux machines virtuelles, les conteneurs n'ont pas de surcharge importante et permettent donc une utilisation plus efficace du système et des ressources sous-jacentes.

### Que sont les conteneurs ?

Les conteneurs adoptent une approche différente : en tirant parti des mécanismes de bas niveau du système d’exploitation hôte, les conteneurs offrent la majeure partie de l’isolation des machines virtuelles avec une fraction de la puissance de calcul.


1- recuperer une image (exemple de busybox)

```bash
$ docker pull busybox
# exemple: docker pull ubuntu:18.04
```

2- executer l'image

```bash
$ docker run busybox
```

3- afficher la liste de tout les image present sur mon systeme

```bash
$ docker images
```

4 - afficher les conteneur en cours d'execution

```bash
$ docker ps
# ou
$ docker container ls
# pour voir ceux qui se sont arreté
$ docker ps -a
```

5 - supprimer des conteneurs

```bash
# suivie des identifiant des images en status Exited
$ docker rm 6565664946446 444445454154
# supprimer tout les conteneurs fermé
$ docker rm $(docker ps -a -q -f status=exited)
# ou
$ docker container prune
```

6 - supprimer les image dont je n'es plus l'utilité

```bash
$ docker rmi
```

7- rechercher des images

```bash
$ docker search
```

7 - envoyer une image sur le docker hub

```bash
$ docker login
Login in with your Docker ID to push and pull images from Docker Hub. If you do not have a Docker ID, head over to https://hub.docker.com to create one.
Username: yourusername
Password:
WARNING! Your password will be stored unencrypted in /Users/yourusername/.docker/config.json
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/credential-store

Login Succeeded


$ docker push yourusername/catnip
```

## Dockerfile

Un Dockerfile est un simple fichier texte contenant la liste des commandes exécutées par le client Docker lors de la création d'une image. C'est un moyen simple d'automatiser ce processus. L'avantage principal est que les commandes d'un Dockerfile sont quasiment identiques à leurs équivalents Linux. Vous n'avez donc pas besoin d'apprendre une nouvelle syntaxe pour créer vos propres Dockerfiles.


nous allons creer un dockerfile pour un application web ecris en Flask

```dockerfile
FROM python:3.8

# definir un repertoire de travail
WORKDIR /usr/src/app

# copier les fichier necessaire a l'app
COPY . .

# installation des dependance
RUN pip install --no-cache-dir -r requirements.txt

# preciser le port a exposer
EXPOSE 5000

# commande pour executer l'application
CMD ["python", "./app.py"]
```


1 - executer le dockerfile

```bash
$ docker build -t nresh/my_work_directory
```

2 - lancement du projet

```bash
$ docker run -p 8888:5000 nresh/my_work_directory
```

## Environnements multi-conteneurs

```
.
├── Dockerfile
├── README.md
├── aws-compose.yml
├── docker-compose.yml
├── flask-app
│   ├── app.py
│   ├── package-lock.json
│   ├── package.json
│   ├── requirements.txt
│   ├── static
│   ├── templates
│   └── webpack.config.js
├── setup-aws-ecs.sh
├── setup-docker.sh
├── shot.png
└── utils
    ├── generate_geojson.py
    └── trucks.geojson
```

```dockerfile
# start from base
FROM ubuntu:18.04

# nom du mainteneur du projet (le dev et son email)
MAINTAINER Prakhar Srivastav <prakhar@prakhar.me>

# installation des dependances
RUN apt-get -yqq update
RUN apt-get -yqq install python3-pip python3-dev curl gnupg
RUN curl -sL https://deb.nodesource.com/setup_10.x | bash
RUN apt-get install -yq nodejs

# copier l'app dans un nouveau volume de conteneur
ADD flask-app /opt/flask-app

WORKDIR /opt/flask-app

# installation des dependance specifique a l'app
RUN npm install
RUN npm run build
RUN pip3 install -r requirements.txt

# expose port
EXPOSE 5000

# start app
CMD [ "python3", "./app.py" ]
```

## Réseau Docker

Comment faire communiquer un conteneur avec l'autre ?

1 - creer un reseau

```bash
$ docker network create new_network_for_me
```

2 - executer des conteneur dans le reseau

```bash
$ docker run -it --rm --net new_network_for_me yourusername/foodtrucks-web bash
```

3 - supprimer un reseau

```bash
$ docker network rm new_network_for_me
```

4 - inspecter le reseau

```bash
$ docker network inspect new_network_for_me
```

## Docker Compose

Jusqu'à présent, nous avons consacré tout notre temps à explorer le client Docker. Cependant, dans l'écosystème Docker, il existe de nombreux autres outils open source qui fonctionnent parfaitement avec Docker. En voici quelques exemples :

* Docker Machine - Créez des hôtes Docker sur votre ordinateur, chez des fournisseurs de cloud et au sein de votre propre centre de données.
* Docker Compose - Un outil permettant de définir et d'exécuter des applications Docker multi-conteneurs.
* Docker Swarm - Une solution de clustering native pour Docker
* Kubernetes - Kubernetes est un système open source permettant d'automatiser le déploiement, la mise à l'échelle et la gestion des applications conteneurisées.

Dans cette section, nous allons examiner l'un de ces outils, Docker Compose, et voir comment il peut faciliter la gestion des applications multi-conteneurs.


### definition 

 Compose est un outil permettant de définir et d'exécuter facilement des applications Docker multi-conteneurs. Il fournit un fichier de configuration docker-compose.ymlqui permet de lancer une application et l'ensemble des services dont elle dépend en une seule commande. Compose fonctionne dans tous les environnements : production, préproduction, développement, test, ainsi que dans les flux d'intégration continue (CI), bien qu'il soit particulièrement adapté aux environnements de développement et de test.


### fichier docker compose

```yml
# docker-compose.yml
version: "3"
services:
  es:
    image: docker.elastic.co/elasticsearch/elasticsearch:6.3.2
    container_name: es
    environment:
      - discovery.type=single-node
    ports:
      - 9200:9200
    volumes:
      - esdata1:/usr/share/elasticsearch/data
  web:
    # image ou build: . (pour ne par redemarer a chaque modif en developpement)
    image: yourusername/foodtrucks-web
    # executer avec cette commande
    command: python3 app.py
    # environment: - DEGUB: True (developpement)
    # demarer es avant
    depends_on:
      - es
    # lancer sur le ports ....
    ports:
      - 5000:5000
    volumes:
      - ./flask-app:/opt/flask-app
volumes:
  esdata1:
    driver: local
```

1 - services: definis les differents services en leur attribuant des nom (ici: es, web)

2 - les parametres

1.1 - images :  parametre requis -- fais reference a l'image docker a executer

1.2 - pour les autres parametre voir [la documentation officielle](https://docs.docker.com/compose/compose-file)


### executer le docker compose

```bash
$ docker-compose up
# en mode detaché
$ docker-compose up -d
```

### détruire le cluster et les volumes de données

```bash
$ docker-compose down -v
```



[source du document](https://docker-curriculum.com/)
