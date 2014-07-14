Spark Risk Monte Carlo
==============

A simple Spark application that calculates Value at Risk using the Monte Carlo method.

To make a jar:

    mvn package

To run from a gateway node in a CDH5 cluster:

    source /etc/spark/conf/spark-env.sh

    export JAVA_HOME=/usr/java/jdk1.7.0_45-cloudera

    # system jars:
    CLASSPATH=/etc/hadoop/conf
    CLASSPATH=$CLASSPATH:$HADOOP_HOME/*:$HADOOP_HOME/lib/*
    CLASSPATH=$CLASSPATH:$HADOOP_HOME/../hadoop-mapreduce/*:$HADOOP_HOME/../hadoop-mapreduce/lib/*
    CLASSPATH=$CLASSPATH:$HADOOP_HOME/../hadoop-yarn/*:$HADOOP_HOME/../hadoop-yarn/lib/*
    CLASSPATH=$CLASSPATH:$HADOOP_HOME/../hadoop-hdfs/*:$HADOOP_HOME/../hadoop-hdfs/lib/*
    CLASSPATH=$CLASSPATH:$SPARK_HOME/assembly/lib/*

    # app jar:
    CLASSPATH=$CLASSPATH:target/montecarlo-risk-0.0.1-SNAPSHOT.jar

    CONFIG_OPTS="-Dspark.master=local -Dspark.jars=target/montecarlo-risk-0.0.1-SNAPSHOT.jar"
    
    $JAVA_HOME/bin/java -cp $CLASSPATH $CONFIG_OPTS com.cloudera.datascience.montecarlorisk.MonteCarloRisk <instruments file> <num trials> <parallellism> <factor means file> <factor covariances file>

This will run the application in a single local process.  If the cluster is running a Spark standalone
cluster manager, you can replace "-Dspark.master=local" with
"-Dspark.master=spark://`<master host>`:`<master port>`".

If the cluster is running YARN, you can replace "-Dspark.master=local" with "-Dspark.master=yarn-client".

Future releases will include a "spark-submit" script that abstracts away the pain of building the
classpath and specifying the cluster manager.
