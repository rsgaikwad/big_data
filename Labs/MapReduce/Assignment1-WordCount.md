# Hadoop & Mapreduce WordCount Examples 

## Problem Statement: Find out Number of word counts from given inputs.


1.	Create the input directory /user/cloudera/wordcount/input in HDFS:

    $ sudo su hdfs

    $ hadoop fs -mkdir /user/cloudera 

    $ hadoop fs -chown cloudera /user/cloudera

    $ exit

    $ sudo su cloudera

    $ hadoop fs -mkdir /user/cloudera/wordcount /user/cloudera/wordcount/input

2.	Create sample text files as input and move to the input directory:

    $ echo "Hello World Bye World" > file0

    $ echo "Hello Hadoop Goodbye Hadoop" > file1

    $ hadoop fs -put file* /user/cloudera/wordcount/input

3.  Compile WordCount.java:
  
    $mkdir wordcount_classes

    $javac -cp /usr/lib/hadoop/*:/usr/lib/hadoop-mapreduce/* WordCount.java -d wordcount_classes/

  where <classpath> is:
      
      o	CD5 : /usr/lib/hadoop/*:/usr/lib/hadoop-mapreduce/*
      
      o	CDH 4
      
        Parcel installation -/opt/cloudera/parcels/CDH/lib/hadoop/*:/opt/cloudera/parcels/CDH/lib/hadoop/client-0.20/*
      
        Package installation -/usr/lib/hadoop/*:/usr/lib/hadoop/client-0.20/*
      
      o	CDH3 - /usr/lib/hadoop-0.20/hadoop-0.20.2-cdh3u6-core.jar

4.	Create a JAR:
    
      $ jar -cvf wordcount.jar -C wordcount_classes/ . 

5.	Run the application:
      $ hadoop jar wordcount.jar org.myorg.WordCount /user/cloudera/wordcount/input /user/cloudera/wordcount/output

6.	Examine the output:
      $ hadoop fs -cat /user/cloudera/wordcount/output/part-00000

      Bye 1
      Goodbye 1
      Hadoop 2
      Hello 2
      World 2

7.	Remove the output directory /user/cloudera/wordcount/output so that you can run the sample again:
      $ hadoop fs -rm -r /user/cloudera/wordcount/output



Example :


[training@localhost ~]$ hadoop fs -mkdir /home/training/cloudera
[training@localhost ~]$ hadoop fs -mkdir /home/training/cloudera/wordcount /home/training/cloudera/wordcount/input
[training@localhost ~]$ echo "Hello World Bye World" > file0
[training@localhost ~]$ echo "Hello Hadoop Goodbye Hadoop" > file1
[training@localhost ~]$ hadoop fs -put file* /home/training/cloudera/wordcount/input
[training@localhost ~]$ mkdir wordcount_classes
[training@localhost ~]$ javac -cp /usr/lib/hadoop/*:/usr/lib/hadoop/client-0.20/* -d wordcount_classes WordCount.java
javac: file not found: WordCount.java
Usage: javac <options> <source files>
use -help for a list of possible options
[training@localhost ~]$ pwd
/home/training
[training@localhost ~]$ vi WordCount.java
[training@localhost ~]$ javac -cp /usr/lib/hadoop/*:/usr/lib/hadoop/client-0.20/* -d wordcount_classes WordCount.java
[training@localhost ~]$ jar -cvf wordcount.jar -C wordcount_classes/ .
added manifest
adding: org/(in = 0) (out= 0)(stored 0%)
adding: org/myorg/(in = 0) (out= 0)(stored 0%)
adding: org/myorg/WordCount$Reduce.class(in = 1611) (out= 649)(deflated 59%)
adding: org/myorg/WordCount$Map.class(in = 1938) (out= 798)(deflated 58%)
adding: org/myorg/WordCount.class(in = 1546) (out= 749)(deflated 51%)
 [training@localhost ~]$ hadoop jar wordcount.jar org.myorg.WordCount /home/training/cloudera/wordcount/input /home/training/cloudera/wordcount/output
14/12/14 09:43:11 WARN mapred.JobClient: Use GenericOptionsParser for parsing the arguments. Applications should implement Tool for the same.
14/12/14 09:43:11 WARN snappy.LoadSnappy: Snappy native library is available
14/12/14 09:43:11 INFO util.NativeCodeLoader: Loaded the native-hadoop library
14/12/14 09:43:11 INFO snappy.LoadSnappy: Snappy native library loaded
14/12/14 09:43:11 INFO mapred.FileInputFormat: Total input paths to process : 2
14/12/14 09:43:12 INFO mapred.JobClient: Running job: job_201412140803_0002
14/12/14 09:43:13 INFO mapred.JobClient:  map 0% reduce 0%
14/12/14 09:43:44 INFO mapred.JobClient:  map 66% reduce 0%
14/12/14 09:43:58 INFO mapred.JobClient:  map 100% reduce 0%
14/12/14 09:44:16 INFO mapred.JobClient:  map 100% reduce 100%
14/12/14 09:44:22 INFO mapred.JobClient: Job complete: job_201412140803_0002
14/12/14 09:44:22 INFO mapred.JobClient: Counters: 23
14/12/14 09:44:22 INFO mapred.JobClient:   Job Counters 
14/12/14 09:44:22 INFO mapred.JobClient:     Launched reduce tasks=1
14/12/14 09:44:22 INFO mapred.JobClient:     SLOTS_MILLIS_MAPS=77687
14/12/14 09:44:22 INFO mapred.JobClient:     Total time spent by all reduces waiting after reserving slots (ms)=0
14/12/14 09:44:22 INFO mapred.JobClient:     Total time spent by all maps waiting after reserving slots (ms)=0
14/12/14 09:44:22 INFO mapred.JobClient:     Launched map tasks=3
14/12/14 09:44:22 INFO mapred.JobClient:     Data-local map tasks=3
14/12/14 09:44:22 INFO mapred.JobClient:     SLOTS_MILLIS_REDUCES=30439
14/12/14 09:44:22 INFO mapred.JobClient:   FileSystemCounters
14/12/14 09:44:22 INFO mapred.JobClient:     FILE_BYTES_READ=79
14/12/14 09:44:22 INFO mapred.JobClient:     HDFS_BYTES_READ=396
14/12/14 09:44:22 INFO mapred.JobClient:     FILE_BYTES_WRITTEN=220152
14/12/14 09:44:22 INFO mapred.JobClient:     HDFS_BYTES_WRITTEN=41
14/12/14 09:44:22 INFO mapred.JobClient:   Map-Reduce Framework
14/12/14 09:44:22 INFO mapred.JobClient:     Reduce input groups=5
14/12/14 09:44:22 INFO mapred.JobClient:     Combine output records=6
14/12/14 09:44:22 INFO mapred.JobClient:     Map input records=2
14/12/14 09:44:22 INFO mapred.JobClient:     Reduce shuffle bytes=91
14/12/14 09:44:22 INFO mapred.JobClient:     Reduce output records=5
14/12/14 09:44:22 INFO mapred.JobClient:     Spilled Records=12
14/12/14 09:44:22 INFO mapred.JobClient:     Map output bytes=82
14/12/14 09:44:22 INFO mapred.JobClient:     Map input bytes=50
14/12/14 09:44:22 INFO mapred.JobClient:     Combine input records=8
14/12/14 09:44:22 INFO mapred.JobClient:     Map output records=8
14/12/14 09:44:22 INFO mapred.JobClient:     SPLIT_RAW_BYTES=342
14/12/14 09:44:22 INFO mapred.JobClient:     Reduce input records=6
[training@localhost ~]$ 

