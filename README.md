# docker-pyspark-custom

This docker projet boostrap standalone and featurefull spark environment to develop ML.

## Quickstart

When you create your machine, be sure you got right permission on your ssh private key.

```
chmod 0400 <Key_name>
```

Note : It's not necessary on Windows since Posix file permissions is emulated.

### To test hadoop:

```
# Get dataset first :
./prepare.sh`

# Download Docker image.  Be sure you actually use the latest version.
docker pull agileops/fastds-tutorial:latest

# To get yarn running
docker run --rm  -d -p9000:9000 -p 8088:8088 -v $PWD/dataset:/work-dir/data -ti agileops/fastds-tutorial bootstrap.sh

# List your active Docker containers. And, find the container id of your latest one.
docker ps

# Enter in your docker image
docker exec -ti <docker_container_id> bash

# Before uploading our dataset to hbase, create parent directory
hadoop fs -mkdir -p hdfs://localhost:9000/user/root

# Provision hdfs using local data. To learn more commands
hadoop fs -copyFromLocal data/ hdfs://localhost:9000/user/root/data

# Start map/reduce job
yarn jar $HADOOP_HOME/hadoop-streaming.jar -input data/tpsgc-pwgsc_co-ch_tous-all.csv -output out -mapper /bin/cat -reducer /bin/wc

# Show files in the output folder
hadoop fs -ls hdfs://localhost:9000/user/root/out

# show results
#1) reference (computation with a commandline)
wc data/tpsgc-pwgsc_co-ch_tous-all.csv 
361318 22527194 285375701
#2) same computation with mapreduce (when the file is too big usually)
hadoop fs -cat out/part-00000
361318 22527194 285375701
```

Note : For compatibilities/accessibilities/simplicites against hardware and env. requirements, tensorflow and pytorch are configured without AVX and Cuda.

## Suggested datasets

To download theses datasets use the following command after cloning this repos.

```
./prepare.sh
```

Canada - Contrats octroyés 2009-ajd. 270 mo
https://ouvert.canada.ca/data/fr/dataset/53753f06-8b28-42d7-89f7-04cd014323b0

Montréal - Comptage sur les pistes cyclables
http://donnees.ville.montreal.qc.ca/dataset/velos-comptage
