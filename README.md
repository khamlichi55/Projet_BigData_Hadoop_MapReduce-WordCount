# Projet Hadoop MapReduce - WordCount

## Description

Ce projet illustre l’utilisation de Hadoop MapReduce pour effectuer un calcul simple de type WordCount. Le projet est déployé dans un cluster Hadoop composé d’un noeud maître (Namenode) et de deux noeuds workers (Datanodes) sous forme de conteneurs Docker.

L’objectif est de démontrer la configuration d’un cluster Hadoop dans des conteneurs Docker, la manipulation de fichiers dans HDFS, ainsi que le développement et le déploiement d’une application MapReduce simple.

## Structure du projet

- Conteneurs Docker :
  - **hadoop-master** : Noeud maître (Namenode + Resource Manager)
  - **hadoop-worker1** et **hadoop-worker2** : Noeuds workers (Datanodes + Node Managers)

- Application MapReduce WordCount développée en Java avec Maven :
  - **TokenizerMapper** : Mapper qui découpe le texte en mots et émet (mot, 1)
  - **IntSumReducer** : Reducer qui somme les occurrences des mots
  - **WordCount** : Classe principale qui configure et lance le job MapReduce

## Fonctionnalités principales

- Création d’un réseau Docker et lancement des conteneurs Hadoop
- Démarrage du cluster Hadoop via un script dans le conteneur master
- Chargement des fichiers dans HDFS et manipulation des fichiers (mkdir, put, ls, tail, etc.)
- Compilation et packaging du projet Java avec Maven, incluant toutes les dépendances
- Exécution du job MapReduce sur le cluster Hadoop
- Monitoring des jobs et du cluster via les interfaces web (ports exposés)

## Prérequis

- Docker installé et configuré
- Maven et JDK 8 installés pour compiler le projet Java
- VSCode recommandé pour le développement Java avec les extensions Maven for Java et Extension Pack for Java

## Utilisation

1. Télécharger l’image Docker Hadoop cluster :
   ```bash
   docker pull liliasfaxi/hadoop-cluster:latest
   ```
2. Créer un réseau Docker :
   ```bash
   docker network create hadoop-net
   ```
3. Lancer les conteneurs Docker :
   ```bash
   docker run -d --net hadoop-net --name hadoop-master liliasfaxi/hadoop-cluster:latest /bin/bash /start-dfs.sh
   docker run -d --net hadoop-net --name hadoop-worker1 liliasfaxi/hadoop-cluster:latest
   docker run -d --net hadoop-net --name hadoop-worker2 liliasfaxi/hadoop-cluster:latest
   ```
4. Accéder au conteneur master et démarrer les services Hadoop :
   ```bash
   docker exec -it hadoop-master /bin/bash
   /start-dfs.sh
   /start-yarn.sh
   ```
5. Créer les répertoires HDFS et copier les fichiers d’entrée :
   ```bash
   hdfs dfs -mkdir /input
   hdfs dfs -put input.txt /input/
   ```
6. Compiler et packager le projet Java (depuis votre machine locale) :
   ```bash
   mvn clean package
   ```
7. Copier le fichier jar dans le conteneur master :
   ```bash
   docker cp target/wordcount-1.0-SNAPSHOT.jar hadoop-master:/root/
   ```
8. Lancer le job MapReduce :
   ```bash
   yarn jar /root/wordcount-1.0-SNAPSHOT.jar com.example.WordCount /input /output
   ```
9. Vérifier le résultat :
   ```bash
   hdfs dfs -cat /output/part-r-00000
   ```
## Aperçu des images du projet

### Cluster Hadoop - Master et Slaves

<img src="https://github.com/user-attachments/assets/42a3f3be-f3ed-4121-8695-81ac059d0207" alt="Conteneurs Hadoop" width="600" />

### Démarrage des services Hadoop et YARN sur le master

<img src="https://github.com/user-attachments/assets/5bf5870d-53e7-4e0b-92c6-1643d12a8ad2" alt="Démarrage Hadoop" width="600" />

### Chargement du fichier purchases dans HDFS

<img src="https://github.com/user-attachments/assets/5d0f490a-97fd-4ceb-8ec8-70882ba6c109" alt="Chargement fichier" width="600" />

### Interface Web du Namenode (port 50070)

![image](https://github.com/user-attachments/assets/97edcf2c-189f-46c2-8411-a3ce925255a0)


### Interface Web du Resource Manager (port 8088)

![image](https://github.com/user-attachments/assets/0daa109d-61d4-4cae-b7a6-98930a7670a9)

## Programme Java MapReduce : WordCount
<img src="https://github.com/user-attachments/assets/1d5f454c-c1d2-4863-b642-7fffc12226b1" alt="Capture finale" width="600"/>
---

## Auteur

Projet développé par - **Idrissi Khamlichi Abdelhadi** - Ingenieur Informatique et Reseaux
-   [Mon CvWeb](https://ik-abdou.vercel.app/)
