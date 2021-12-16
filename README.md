# Projet-BigData-HUGO_D-QUENTIN_F-Emeric_L

Sur base de :
[github/big-data-europe](https://github.com/big-data-europe/docker-hadoop)

Projet HDFS dans le cadre du cours de big data

Cluster à disposition :
 - Distribuer-3-nodes.yml
 - Distribuer-5-nodes.yml
 - Non-distribuer.yml

# Installation

1) Téléchargez [Docker](https://www.docker.com/get-started)
2) clonez le git
3) cd Chemin/acces/fichier
4) Choissisez le cluster que vous voulez démarrer
5) lancer une invite de commande
6) démarrer le cluster choissis 
```bash
docker-compose --file <nomFichier>.yml up -d
```
7) Acceder à la page web -> http://localhost:9870/
8) Vous pouvez éteindre le cluster avec
```bash
docker-compose --file <nomFichier>.yml down
```

# Fichiers

Exemple de fichier à écrire dans HDFS 

lien :

[Fichiers](https://mega.nz/folder/slolVIbB#7RUxiXkNBGd1J-jYSD0i5g)

# Utilisation - Ecriture dans HDFS

Pour ouvrir le terminal bash du namenode
```bash
docker exec -it namenode bash
```

Créez un répertoire pour stocker les fichiers d'entrée
```bash
mkdir input
```

Si vous souhaitez utiliser map reduce :

Créer un autre répertoire pour stocker les fichiers jar
```bash
mkdir jars
```

-------
Dans un second terminal

Ajoutez le fichier dans le namenode
Trouvez le container ID de votre namenode en utilisant :
```bash
docker container ls
```
Cela devrait ressembler à quelque chose comme cela cb0c13085cd3
```bash
docker cp <Chemin>\<Chemin>\<nomFichier>.<Format> <NAMENODE-CONTAINER-ID>:/input/
```


Si vous souhaitez utiliser map reduce :

Ajoutez le fichier .jar nommer wordcount dans le namenode

Dans le dossier "submit" contiens déja un fichier .jar nommé "WordCount.jar"
```bash
docker cp submit/WordCount.jar <NAMENODE-CONTAINER-ID>:/jars/
```

----------------
Dans le premier terminal

Pour distribuer il faut faire ces deux commandes
```bash
hadoop fs -mkdir -p input
hdfs dfs -put ./input/* input
```

Pour map Reduce : 

démarrer l'exécutable
```bash
hadoop jar jars/WordCount.jar org.apache.hadoop.examples.WordCount input output
```

Pour voir la sortie 
```bash
hdfs dfs -ls output/
hdfs dfs -cat output/part-r-00000
```

# Commandes importantes :

Dans la commande bash du namenode :

Voir la liste des fichiers/dossiers dans le répertoire hdfs
```bash
hdfs dfs -ls / 
```

Crée un dossier
```bash
hdfs dfs -mkdir /<nomDossier> 
```

Permet de transférer un fichier local a hadoop
```bash
hdfs dfs -put <chemin local> <chemin hadoop> 
```

Permet de lire le fichier dans hadoop
```bash
hdfs dfs -cat <nomFichier> 
```

Vérifier si le fichier existe dans HDFS 
```bash
hadoop fs -ls
hadoop fs -ls input/
```

Supprimer les fichiers
```bash
hadoop fs -rm ./input/* input
```

Vérifier la division des fichiers
```bash
hdfs fsck input/*
```

Augmenter le nombre de blocs diviser lors de l'écriture 
```bash
hdfs dfs -Ddfs.blocksize=1048576 -put ./input/* input
```

Vérifier les nœuds en vie 
```bash
hdfs dfsadmin -report
```

Dans l'invite Windows :

Pour voir les images lancer + port
```bash
Docker ps  
```

Pour voir les networks en cours
```bash
Docker network ls
```


