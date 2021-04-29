# Mapreduce-wordcountprogram-docker

##Introduction
   This goal of the assignment is to create a Word Count program using the MapReduce Programing model and the program can be implemented on any local machine or on a virtual machine created using AWS(Amazon Web Services) or GCP(Google Cloud Platform).
   In this case, the program is implemented and run using Python programming language, Docker to run Hadoop Cluster on the local machine. Here the local machine can be Linux or the MacOS.

##Prerequisites
The following softwares are required to run the program and get desired output.
+ Docker : It can be downloaded via this link
+ Docker Compose : It can be downloaded via this link 
+ Git : It can be downloaded via this link
+ Python : It can be downloaded via this link

##Steps to run the application on local machine

+ Deploy Hadoop Cluster in Docker :
  We will use the Docker image by big-data-europe(https://github.com/big-data-europe/docker-hadoop) repository to set up Hadoop. Steps to get the Docker image on the local machine:
+ Check if the Docker is installed by running the following command :
  ```docker version```
+ Check if the Docker Compose is installed by running the following command : ```docker-compose version```
+ Create a folder with a desired name where the git repository will be cloned.
+ Open the command prompt of the local system and go to the created folder and run the following command :
  ```git clone git@github.com:big-data-europe/docker-hadoop.git```
+ On running the ls -la a new folder with name docker-hadoop will appear.
+ Go to the docker-hadoop folder and change the docker-compose.yml with the following code :
  https://gist.github.com/nathan815/a938b3f7a4d06b2811cf2b1a917800e1
+ Run the following command ```docker-compose up -d```
+ Run the following command to see the up and running containers
  ```docker ps```
+ The current status of the Hadoop Cluster will be available at
  http://localhost:9870/dfshealth.html#tab-overview
+ Now, the developed python codes for mapper and reducer named ```mapper.py``` and ```reducer.py``` has to be placed in the same folder docker-hadoop
+ Now, we need to copy the python files to the namenode docker container by running the following command :
  ```docker cp LOCAL_PATH/mapper.py namenode:mapper.py```
  ```docker cp LOCAL_PATH/reducer.py namenode:reducer.py```
+ Running the following command, we need to go inside the namenode docker container 
  ```docker exec -it namenode bash```
+ Being inside of the docker container, we need to make some test inputs. Above image states that and here are the commands : 
  ```
  mkdir input
  echo "Hello World" >input/f1.txt
  echo "Hello Docker" >input/f2.txt 
  echo "Hello Hadoop" >input/f3.txt 
  echo "Hello MapReduce" >input/f4.txt
  ```
+ The MapReduce program accesses files from the Hadoop Distributed File System (HDFS). Run the following to transfer the input directory and files to HDFS:
  ```hadoop fs -mkdir -p input hdfs dfs -put ./input/* input```
+ Use find / -name 'hadoop-streaming*.jar' to locate the hadoop string library JAR file. The path should look something like PATH/hadoop-streaming-3.2.1.jar  
+ Finally, we can execute the MapReduce program:
  ```hadoop jar /opt/hadoop-3.2.1/share/hadoop/tools/lib/hadoop-streaming-3.2.1.ja r -file mapper.py -mapper mapper.py -file reducer.py -reducer reducer.py -input input -output output```
+ To see the output run the command :
  ```hadoop dfs -cat output```
  The output shall look like below :
  ```"Hello" 1 "Hadoop 1 "Sumana," 1 "test" 1 [...] ```
+ To stop the docker container once the desired output is got, run the command : 
  ```docker-compose down```
