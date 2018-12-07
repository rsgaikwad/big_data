WHAT IS PIG?
Pig is a high level scripting language that is used with Apache Hadoop. Pig enables data workers to write complex data transformations without knowing Java. Pig’s simple SQL-like scripting language is called Pig Latin, and appeals to developers already familiar with scripting languages and SQL.

Pig is complete, so you can do all required data manipulations in Apache Hadoop with Pig. Through the User Defined Functions(UDF) facility in Pig, Pig can invoke code in many languages like JRuby, Jython and Java. You can also embed Pig scripts in other languages. The result is that you can use Pig as a component to build larger and more complex applications that tackle real business problems.

Pig works with data from many sources, including structured and unstructured data, and store the results into the Hadoop Data File System.

Pig scripts are translated into a series of MapReduce jobs that are run on the Apache Hadoop cluster.

DOWNLOAD THE DATA

Download the driver data file from here.
Once you have the file you will need to unzip the file into a directory. We will be uploading two csv files – truck_event_text_partition.csv and drivers.csv.

UPLOAD THE DATA FILES
Select the HDFS Files view from the Off-canvas menu at the top. That is the views menu. The HDFS Files view allows you to view the Hortonworks Data Platform(HDP) file store. The HDP file system is separate from the local file system.

select_files_view

Navigate to /user/maria_dev or a path of your choice, click Upload and Browse, which brings up a dialog box where you can select the drivers.csv file from your computer. Upload the truck_event_text_partition.csv file in the same way. When finished, notice that both files are now in HDFS.

uploaded_files

CREATE YOUR SCRIPT
Note: In this tutorial Vi is used; however, any text editor will work as long as the files we create are stored on the Sandbox.


vi Truck-Events
Note: Press i to enter insert mode in Vi.

DEFINE A RELATION
In this step, you will create a script to load the data and define a relation.

On line 1 define a relation named truck_events that represents all the truck events
On line 2 use the DESCRIBE command to view the truck_events relation
The completed code will look like:

truck_events = LOAD '/user/maria_dev/truck_event_text_partition.csv' USING PigStorage(',');
DESCRIBE truck_events;
vi-script1

Note: In the LOAD script, you can choose any directory path. Verify the folders have been created in HDFS Files View.

SAVE AND EXECUTE THE SCRIPT
To save your changes while on Vi press esc and type:x then enter.

To execute the script return to ~/ and submit the file we just created to pig:

pig -f Truck-Events
submit-script1

This action creates one or more MapReduce jobs. After a moment, the script starts and the page changes.

When the job completes, result output. Notice truck_events does not have a schema because we did not define one when loading the data into relation truck_events.

save_execute_script_truck_events

DEFINE A RELATION WITH A SCHEMA
Let’s use the above code but this time with a schema. Modify line 1 of your script and add the following AS clause to define a schema for the truck events data. Open Vi and enter the following script:

truck_events = LOAD '/user/maria_dev/truck_event_text_partition.csv' USING PigStorage(',')
AS (driverId:int, truckId:int, eventTime:chararray,
eventType:chararray, longitude:double, latitude:double,
eventKey:chararray, correlationId:long, driverName:chararray,
routeId:long,routeName:chararray,eventDate:chararray);
DESCRIBE truck_events;
load_truck_events_data_schema

Save and execute the script again.

Note: Recall that we used :x to save the script and pig -f Truck-Events to run the job.

This time you should see the schema for the truck_events relation:

save_execute_script_truck_events_schema

DEFINE A NEW RELATION FROM AN EXISTING RELATION
You can define a new relation based on an existing one. For example, define the following truck_events_subset relation, which is a collection of 100 entries (arbitrarily selected) from the truck_events relation.
Add the following line to the end of your code:

truck_events_subset = LIMIT truck_events 100;
DESCRIBE truck_events_subset;
load_truck_events_data_subset

Save and execute the code. Notice truck_events_subset has the same schema as truck_events, because truck_events_subset is a subset of truck_events relation.

save_execute_script_truck_events_subset

VIEW THE DATA
To view the data of a relation, use the DUMP command.
Add the following DUMP command to your Pig script, then save and execute it again:

DUMP truck_events_subset;
dump_truck_events_subset

The command requires a MapReduce job to execute, so you will need to wait a minute or two for the job to complete. The output should be 100 entries from the contents of truck_events_text_partition.csv (and not necessarily the ones shown below, because again, entries are arbitrarily chosen):

result_truck_events_subset

SELECT SPECIFIC COLUMNS FROM A RELATION
Delete the DESCRIBE truck_events, DESCRIBE truck_events_subset and DUMP truck_events_subset commands from your Pig script; you will no longer need those.
One of the key uses of Pig is data transformation. You can define a new relation based on the fields of an existing relation using the FOREACH command. Define a new relation specific_columns, which will contain only the driverId, eventTime and eventType from relation truck_events_subset.
Now the completed code is:

truck_events = LOAD '/user/maria_dev/truck_event_text_partition.csv' USING PigStorage(',')
AS (driverId:int, truckId:int, eventTime:chararray,
eventType:chararray, longitude:double, latitude:double,
eventKey:chararray, correlationId:long, driverName:chararray,
routeId:long,routeName:chararray,eventDate:chararray);
truck_events_subset = LIMIT  truck_events 100;
specific_columns = FOREACH truck_events_subset GENERATE driverId, eventTime, eventType;
DESCRIBE specific_columns;
load_truck_events_data_subset_specific

Save and execute the script and your output will look like the following:

save_execute_script_truck_events_specific

STORE RELATIONSHIP DATA INTO A HDFS FILE
In this step, you will use the STORE command to output a relation into a new file in HDFS. Enter the following command to output the specific_columns relation to a folder named output/specific_columns:

STORE specific_columns INTO 'output/specific_columns' USING PigStorage(',');
store_truck_events_subset_specific

Save and Execute the script. Again, this requires a MapReduce job (just like the DUMP command), so you will need to wait a minute for the job to complete.

Once the job is finished, go to HDFS Files view and look for a newly created folder called “output” under /user/maria_dev:

Note: If you didn’t use the default path above, then the new folder will exist in the path you created.

navigate_to_output_directory

Click on “output” folder. You will find a sub-folder named “specific_columns”.

navigate_to_specific_columns_directory

Click on “specific_columns” folder. You will see an output file called “part-r-00000”:

navigate_to_part_r_file

Click on the file “part-r-00000” and then click on Open :

file_preview

PERFORM A JOIN BETWEEN 2 RELATIONS
In this step, you will perform a join on two driver statistics data sets: truck_event_text_partition.csv and the driver.csv files. Drivers.csv has all the details for the driver like driverId, name, ssn, location, etc.

You have already defined a relation for the events named truck_events. Create a new Pig script named “Pig-Join”. Then define a new relation named drivers then join truck_events and drivers by driverId and describe the schema of the new relation join_data.
The completed code will be:

truck_events = LOAD '/user/maria_dev/truck_event_text_partition.csv' USING PigStorage(',')
AS (driverId:int, truckId:int, eventTime:chararray,
eventType:chararray, longitude:double, latitude:double,
eventKey:chararray, correlationId:long, driverName:chararray,
routeId:long,routeName:chararray,eventDate:chararray);
drivers =  LOAD '/user/maria_dev/drivers.csv' USING PigStorage(',')
AS (driverId:int, name:chararray, ssn:chararray,
location:chararray, certified:chararray, wage_plan:chararray);
join_data = JOIN  truck_events BY (driverId), drivers BY (driverId);
DESCRIBE join_data;
join_two_datasets

Save the script and execute it:

pig -f Truck-Events | tee -a joinAttributes.txt
cat joinAttributes.txt
result_join_data_download

Notice join_data contains all the fields of both truck_events and drivers.

SORT THE DATA USING “ORDER BY”
Use the ORDER BY command to sort a relation by one or more of its fields. Create a new Pig script named “Pig-Sort” from maria_dev home directory enter:

vi Pig-Sort
Next, enter the following commands to sort the drivers data by name then date in ascending order:

drivers =  LOAD '/user/maria_dev/drivers.csv' USING PigStorage(',')
AS (driverId:int, name:chararray, ssn:chararray,
location:chararray, certified:chararray, wage_plan:chararray);
ordered_data = ORDER drivers BY name asc;
DUMP ordered_data;
sort_drivers_data

Save and execute the script. Your output should be sorted as shown here:

result_sort_drivers_data

FILTER AND GROUP THE DATA USING “GROUP BY”
The GROUP command allows you to group a relation by one of its fields. Create a new Pig script named “Pig-Group”. Then, enter the following commands, which group the truck_events relation by the driverId for the eventType which are not ‘Normal’.

truck_events = LOAD '/user/maria_dev/truck_event_text_partition.csv' USING PigStorage(',')
AS (driverId:int, truckId:int, eventTime:chararray,
eventType:chararray, longitude:double, latitude:double,
eventKey:chararray, correlationId:long, driverName:chararray,
routeId:long,routeName:chararray,eventDate:chararray);
filtered_events = FILTER truck_events BY NOT (eventType MATCHES 'Normal');
grouped_events = GROUP filtered_events BY driverId;
DESCRIBE grouped_events;
DUMP grouped_events;
filter_group_datasets

Save and execute the script. Notice that the data for eventType which are not Normal is grouped together for each driverId.

result_group_data

The results of the Pig Job will show all non-Normal events grouped under each driverId.

Congratulations! You have successfully completed the tutorial and well on your way to pigging on Big Data.

