# Projet-BigData-HUGO_D-QUENTIN_F-Emeric_L

Sur base de :
https://github.com/big-data-europe/docker-hadoop


# Installation

- clonez le git
- cd Chemin/acces/fichier
- Choissisez le cluster que vous voulez lancé
- Ensuite une fois dans le dossier, lancez avec " docker-compose up -d "
- Accès a la page web -> http://localhost:9870/



# Ecriture dans HDFS


Get a bash terminal on the namenode <br>
docker exec -it namenode bash

Make a directory for storing input files <br>
mkdir input

Make another directory for storing jar files <br>
mkdir jars

-------

Add the file into the namenode's file system <br>
Find the container ID of your namenode container using docker container ls. <br>
It should be something like cb0c13085cd3.<br>

docker cp relative/path/to/local/file/you/want/to/copy/into/hadoop.txt <NAMENODE-CONTAINER-ID>:/input/

Exemple template :<br>
docker cp D:\BigData\<nomFichier>.<Format> <NAMENODE-CONTAINER-ID>:/input/


Add the wordcount jar into the namenode's file system<br>

This repository's submit folder already contains a compiled WordCount.jar file<br>
docker cp submit/WordCount.jar <NAMENODE-CONTAINER-ID>:/jars/

----------------

Pour distribuer il faut faire ces deux commandes<br>

hadoop fs -mkdir -p input
hdfs dfs -put ./input/* input

Now run the executable<br>

hadoop jar jars/WordCount.jar org.apache.hadoop.examples.WordCount input output

View the output<br>

hdfs dfs -ls output/
hdfs dfs -cat output/part-r-00000


# Commandes importantes :

Voir la liste des fichiers/dossier dans le répértoire hdfs<br>
hdfs dfs -ls / 

Crée une dossier <br>
hdfs dfs -mkdir /"nomFichier" 

Permet de transferer un fichier local a hadoop<br>
hdfs dfs -put "chemin local" "endroitHDFS" 

Permet de lire le fichier dans hadoop<br>
hdfs dfs -cat "nomFichier" 

Vérifier si le fichier existe dans HDFS <br>
hadoop fs -ls
hadoop fs -ls input/

Supprimer les fichiers <br>
hadoop fs -rm ./input/* input

Vérifier la division des fichiers <br>
hdfs fsck input/*

Augmenter le nombre de blocs diviser <br>
hdfs dfs -Ddfs.blocksize=1048576 -put ./input/* input

Verifier les noeuds en vie <br>
hdfs dfsadmin -report

Pour voir les images lancer + port <br>
Docker ps  

Pour voir les networks en cours <br>
Docker network ls 