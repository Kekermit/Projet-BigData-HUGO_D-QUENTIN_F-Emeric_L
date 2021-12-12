# Projet-BigData-HUGO_D-QUENTIN_F-Emeric_L

Sur base :
https://github.com/big-data-europe/docker-hadoop


# Installation

- clonez le git
- cd Chemin/acces/fichier
- Choissisez le cluster que vous voulez lancé
- Ensuite une fois dans le dossier, lancez avec " docker-compose up -d "
- Accès a la page web -> http://localhost:9870/



# Ecriture dans HDFS


Get a bash terminal on the namenode
docker exec -it namenode bash

Make a directory for storing input files
mkdir input

Make another directory for storing jar files
mkdir jars

-------

Add the file into the namenode's file system
Find the container ID of your namenode container using docker container ls.
It should be something like cb0c13085cd3.

docker cp relative/path/to/local/file/you/want/to/copy/into/hadoop.txt <NAMENODE-CONTAINER-ID>:/input/

Exemple template :
docker cp D:\BigData\<nomFichier>.<Format> <NAMENODE-CONTAINER-ID>:/input/


Add the wordcount jar into the namenode's file system

This repository's submit folder already contains a compiled WordCount.jar file
docker cp submit/WordCount.jar <NAMENODE-CONTAINER-ID>:/jars/

----------------

Pour distribuer il faut faire ces deux commandes

hadoop fs -mkdir -p input
hdfs dfs -put ./input/* input

Now run the executable

hadoop jar jars/WordCount.jar org.apache.hadoop.examples.WordCount input output

View the output

hdfs dfs -ls output/
hdfs dfs -cat output/part-r-00000


# Commandes importantes :

Voir la liste des fichiers/dossier dans le répértoire hdfs
hdfs dfs -ls / 

Crée une dossier 
hdfs dfs -mkdir /"nomFichier" 

Permet de transferer un fichier local a hadoop
hdfs dfs -put "chemin local" "endroitHDFS" 

Permet de lire le fichier dans hadoop
hdfs dfs -cat "nomFichier" 

Vérifier si le fichier existe dans HDFS 
hadoop fs -ls
hadoop fs -ls input/

Supprimer les fichiers 
hadoop fs -rm ./input/* input

Vérifier la division des fichiers
hdfs fsck input/*

Augmenter le nombre de blocs diviser
hdfs dfs -Ddfs.blocksize=1048576 -put ./input/* input

Verifier les noeuds en vie
hdfs dfsadmin -report

Pour voir les images lancer + port
Docker ps  

Pour voir les networks en cours
Docker network ls 