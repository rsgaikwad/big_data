Hadoop & Mapreduce Examples: Create your First Program

Problem Statement: Find out Number of Products Sold in Each Country.

Steps: 1
Create a new directory with name MapReduceTutorial

sudo mkdir MapReduceTutorial


Give permissions

sudo chmod -R 777 MapReduceTutorial

Check the file permissions of all these files and if 'read' permissions are missing then grant the same-


Steps: 2

Compile Java files. Its class files will be put in the package directory

javac  -cp /usr/lib/hadoop/*:/usr/lib/hadoop-mapreduce/* SalesMapper.java SalesCountryReducer.java SalesCountryDriver.java -d .


This compilation will create a directory in a current directory named with package name specified in the java source file (i.e. SalesCountry in our case) and put all compiled class files in it.


cp SalesJan2009.csv ~/inputMapReduce


Steps: 3

Create a new file Manifest.txt

vi Manifest.txt

add following lines to it,

Main-Class: SalesCountry.SalesCountryDriver

---SalesCountry.SalesCountryDriver is name of main class. Please note that you have to hit enter key at end of this line.

Steps: 4
Create a Jar file

jar cfm ProductSalePerCountry.jar Manifest.txt SalesCountry/*.class

hadoop dfs -copyFromLocal ~/inputMapReduce /user/cloudera

Check that the jar file is created

hadoop fs -ls /user/cloudera/inputMapReduce

Steps: 5

Copy the File SalesJan2009.csv into ~/inputMapReduce

Now Use below command to copy ~/inputMapReduce to HDFS.


hadoop dfs -copyFromLocal ~/inputMapReduce /user/cloudera

hdfs dfs -ls /user/cloudera/inputMapReduce


Step: 6

Run MapReduce job

hadoop jar ProductSalePerCountry.jar /user/cloudera/inputMapReduce /user/cloudera/mapreduce_output_sales

This will create an output directory named mapreduce_output_sales on HDFS. Contents of this directory will be a file containing product sales per country.

Step: 7
Result can be seen through command interface as,

hdfs dfs -cat /user/cloudera/mapreduce_output_sales/part-00000


Argentina	1
Australia	38
Austria	7
Bahrain	1
Belgium	8
Bermuda	1
Brazil	5
Bulgaria	1
CO	1
Canada	76
Cayman Isls	1
China	1
Costa Rica	1
Country	1
Czech Republic	3
Denmark	15
Dominican Republic	1
Finland	2
France	27
Germany	25
Greece	1
Guatemala	1
Hong Kong	1
Hungary	3
Iceland	1
India	2
Ireland	49
Israel	1
Italy	15
Japan	2
Jersey	1
Kuwait	1
Latvia	1
Luxembourg	1
Malaysia	1
Malta	2
Mauritius	1
Moldova	1
Monaco	2
Netherlands	22
New Zealand	6
Norway	16
Philippines	2
Poland	2
Romania	1
Russia	1
South Africa	5
South Korea	1
Spain	12
Sweden	13
Switzerland	36
Thailand	2
The Bahamas	2
Turkey	6
Ukraine	1
United Arab Emirates	6
United Kingdom	100
United States	462