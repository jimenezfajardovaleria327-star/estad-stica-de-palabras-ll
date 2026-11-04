# Tema 02: Estadística de Palabras II (Hadoop MapReduce)

Este repositorio contiene la documentación y comandos para ejecutar las pruebas de `wordstandarddeviation` y `multifilewc` en un entorno **Hadoop 3.3.6 Standalone** sobre un contenedor Debian 12.

## 🚀 Requisitos Previos

Tener en ejecución el contenedor Docker de Hadoop:
```bash
docker start hadoop
docker exec -it hadoop bash

export JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64
export HADOOP_HOME=/opt/hadoop-3.3.6
export PATH=$PATH:$HADOOP_HOME/bin:$HADOOP_HOME/sbin:$JAVA_HOME/bin

mkdir -p /tmp/practica2 && cd /tmp/practica2
echo "Hadoop MapReduce procesa datos masivos en paralelo" > archivo1.txt
echo "La desviacion estandar mide la dispersion de palabras" > archivo2.txt

hdfs dfs -mkdir -p /tema2/std_input
hdfs dfs -mkdir -p /tema2/multi_input

hdfs dfs -put archivo1.txt /tema2/std_input/
hdfs dfs -put archivo1.txt archivo2.txt /tema2/multi_input/

# Limpiar salida previa si existe
hdfs dfs -rm -r /tema2/std_output 2>/dev/null

hadoop jar $HADOOP_HOME/share/hadoop/mapreduce/hadoop-mapreduce-examples-3.3.6.jar \
  wordstandarddeviation /tema2/std_input /tema2/std_output

# Consultar resultado
hdfs dfs -cat /tema2/std_output/part-r-00000
# Limpiar salida previa si existe
hdfs dfs -rm -r /tema2/multi_output 2>/dev/null

hadoop jar $HADOOP_HOME/share/hadoop/mapreduce/hadoop-mapreduce-examples-3.3.6.jar \
  multifilewc /tema2/multi_input /tema2/multi_output

# Consultar resultado
hdfs dfs -cat /tema2/multi_output/part-r-00000
