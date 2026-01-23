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



