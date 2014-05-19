##Instalar shark en el cluster.   

Se realizó una prueba de instalación de shark en una máquina virtual separada con la finalidad de no afectar la operación de master.    

Las  referencias se encuentran en las siguientes ligas.   
http://www.abcn.net/2014/04/install-shark-on-cdh5-hadoop2-spark.html   
https://github.com/amplab/shark/wiki/Running-Shark-on-a-Cluster   
http://spark.apache.org/docs/0.9.1/quick-start.html  
http://spark.apache.org/   

La ruta de instalación (usuario hduser) es la misma que el conjunto de aplicaciones 


```{bash}
cd /home/hduser/hadoop-src  
```


###Scala  (todos los equipos)
   
**Descarga e instalación**  
  
```{bash}   
cd /home/hduser/hadoop-src   
wget http://www.scala-lang.org/files/archive/scala-2.10.3.tgz   
tar zxf scala-2.10.3.tgz
```  
Ruta de instalación  */home/hduser/hadoop-src/scala-2.10.3*   


###Spark.  (Master)
  
**Descarga e instalación**  
  
```{bash}
wget http://d3kbcqa49mib13.cloudfront.net/spark-0.9.1-bin-hadoop2.tgz   
tar xvfz spark-0.9.1-bin-hadoop2.tgz   
mv spark-0.9.1-bin-hadoop2/ spark-0.9.1   
```
**Ajuste de parámetros**    

Archivo:	spark-env.sh   
ruta:	/home/hduser/hadoop-src/spark-0.9.1/conf  

```{bash}
cp spark-env.sh.template spark-env.sh
nano spark-env.sh
```
Agregar las siguientes líneas:  

export SCALA_HOME=/path/to/scala-2.10.3
export SPARK_WORKER_MEMORY=4g
 
Archivo: 	slaves.
ruta:	/home/hduser/hadoop-src/spark-0.9.1/conf  

```{bash}
cp spark-env.sh.template spark-env.sh
nano slaves   
```
Agregar hostname de los esclavos:  
  
192.168.15.99	master  
192.168.15.98	master-secundario  
192.168.15.97	hadoop01  
192.168.15.96	hadoop02  
192.168.15.95	hadoop03  
192.168.15.94	hadoop04  
192.168.15.93	hadoop05  
192.168.15.92	hadoop06  
192.168.15.91	hadoop07  
192.168.15.90	hadoop08  
   

###Shark (Master)

**Descarga e instalación**  
  
```{bash}  
cd /home/hduser/hadoop-src  
wget https://github.com/amplab/shark/archive/v0.9.1.tar.gz  
tar zxf v0.9.1.tar.gz   
``` 

**Ajuste de parámetros**    
 
Archivo:	shark-env.sh  
Ruta:		

```{bash}  
home/hduser/hadoop-src/shark-0.9.1/conf   
cp spark-env.sh.template shark-env.sh
nano spark-env.sh
```
  
Modificar el archivo shark-env.sh:
  
export SPARK_USER_HOME=/var/lib/spark  
export SPARK_MEM=2g  
export SHARK_MASTER_MEM=1g  
export SCALA_HOME="/home/hduser/hadoop-src/scala-2.10.3"    
export HIVE_HOME="/home/hduser/hadoop-src/hive-0.12.0-bin"   
export HIVE_CONF_DIR="$HIVE_HOME/conf"   
export HADOOP_HOME="/home/hduser/hadoop-src/hadoop-2.2.0"    
export SPARK_HOME="/home/hduser/hadoop-src"   
export MASTER="spark://HostName:7077"   

 IMPORTANTE   La variable **HostName** se reemplaza por le valor del master

SPARK_JAVA_OPTS=" -Dspark.local.dir=/tmp "
SPARK_JAVA_OPTS+="-Dspark.kryoserializer.buffer.mb=10 "
SPARK_JAVA_OPTS+="-verbose:gc -XX:-PrintGCDetails -XX:+PrintGCTimeStamps "
export SPARK_JAVA_OPTS

**Otros ajustes**

en el host se debe cambiar el host name:
MASTER="spark://HostName:7077" 
  
 Agregar el directorio de spark al path:  
export PATH=$PATH:/home/hduser/hadoop-src/spark-0.9.1/bin  


###Pruebas
    
####SHARK  

./shark-withinfo  

-hiveconf hive.root.logger=INFO,console  
Starting the Shark Command Line Client   
  
Logging initialized using configuration in   jar:file:/home/hduser/hadoop-src/shark-0.9.1/lib_managed/jars/edu.berkeley.cs.shark/hive-common/hive-common-0.11.0-shark-0.9.1.jar!/hive-log4j.properties  
14/05/17 23:11:33 INFO SessionState:   
Logging initialized using configuration in jar:file:/home/hduser/hadoop-src/shark-0.9.1/lib_managed/jars/edu.berkeley.cs.shark/hive-common/hive-common-0.11.0-shark-0.9.1.jar!/hive-log4j.properties   
Hive history    file=/tmp/hduser/hive_job_log_hduser_7761@vagrant-ubuntu-saucy-64_201405172311_893478216.txt   
14/05/17 23:11:34 INFO exec.HiveHistory: Hive history   file=/tmp/hduser/hive_job_log_hduser_7761@vagrant-ubuntu-saucy-64_201405172311_893478216.txt   
14/05/17 23:11:36 INFO slf4j.Slf4jLogger: Slf4jLogger started   
14/05/17 23:11:36 INFO Remoting: Starting remoting  

SHARK iniciar server   

./shark --service sharkserver2   

####SPARK  

/home/hduser/hadoop-src/spark-0.9.1/bin
 ./spark-shell
14/05/17 23:06:03 INFO spark.HttpServer: Starting HTTP Server
14/05/17 23:06:03 INFO server.Server: jetty-7.x.y-SNAPSHOT
14/05/17 23:06:03 INFO server.AbstractConnector: Started SocketConnector@0.0.0.0:39745
Welcome to   

```
      ____              __  
     / __/__  ___ _____/ /__  
    _\ \/ _ \/ _ `/ __/  '_/  
   /___/ .__/\_,_/_/ /_/\_\   version 0.9.1  
      /_/  

```

Using Scala version 2.10.3 (OpenJDK 64-Bit Server VM, Java 1.7.0_51)
Type in expressions to have them evaluated.
Type :help for more information.
14/05/17 23:06:07 INFO slf4j.Slf4jLogger: Slf4jLogger started
14/05/17 23:06:07 INFO Remoting: Starting remoting

./scala
hduser@vagrant-ubuntu-saucy-64:~/hadoop-src/scala-2.10.3/bin$ ./scala
Welcome to Scala version 2.10.3 (OpenJDK 64-Bit Server VM, Java 1.7.0_51).
Type in expressions to have them evaluated.
Type :help for more information.

scala> 


