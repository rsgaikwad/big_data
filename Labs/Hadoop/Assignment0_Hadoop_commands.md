# Hadoop commands

## Hadoop command Usage

$ hadoop fs -help

### 1. mkdir: 
similar to Unix mkdir command, it is used for creating directories in HDFS.

Syntax:

$ hadoop fs -mkdir  [-p] <path>

-p  Do not fail if the directory already exists


$ hadoop fs -mkdir /user/hadoop/ 

$ hadoop fs -mkdir /user/data/ 

$ hadoop fs -mkdir /user/test/ 

$ hadoop fs -mkdir /user/test/in

Notes:

In order to create a sub directory /user/hadoop, its parent directory /user must already exist. Otherwise ‘No such file or directory’ error message will be returned.


### 2. ls:

similar to Unix ls command, it is used for listing directories in HDFS. The -lsr command can be used for recursive listing.

Syntax:

 
$ hadoop fs -ls [-d] [-h] [-R] <path>

List the contents that match the specified file pattern. If path is not specified, the contents of /user/<currentUser> will be listed. Directory entries are of the form:

 
permissions - userId groupId sizeOfDirectory(in bytes) modificationDate(yyyy-MM-dd HH:mm) directoryName
and file entries are of the form:

permissions numberOfReplicas userId groupId sizeOfFile(in bytes)

modificationDate(yyyy-MM-dd HH:mm) fileName

-d Directories are listed as plain files.

-h Formats the sizes of files in a human-readable fashion rather than a number of bytes.

-R Recursively list the contents of directories.


$ hadoop fs -ls / 

$ hadoop fs -lsr /


### 3. put:

Copies files from local file system to HDFS. This is similar to -copyFromLocal command.

Syntax:

	$ hadoop fs -put [-f] [-p] <localsrc> ... <dst>

Copying fails if the file already exists, unless the -f flag is given. Passing -p preserves access and modification times, ownership and the mode. Passing -f overwrites the destination if it already exists.

$ hadoop fs -put sample.txt /user/data/


### 4. get:

Copies files from HDFS to local file system. This is similar to -copyToLocal  command.


$ hadoop fs -get /user/data/sample.txt workspace/


### 5. cat:

similar to Unix cat command, it is used for displaying contents of a file.

$ hadoop fs -cat /user/data/sample.txt

### 6. cp:

similar to Unix cp command, it is used for copying files from one directory to another within HDFS.

$ hadoop fs -cp /user/data/sample.txt /user/hadoop 

$ hadoop fs -cp /user/data/sample.txt /user/test/in


### 7. mv:

similar to Unix mv command, it is used for moving a file from one directory to another within HDFS.

$ hadoop fs -mv /user/hadoop/sample.txt /user/test/

### 8. rm:

similar to Unix rm command, it is used for removing a file from HDFS. The command -rmr can be used for recursive delete.

Syntax:

$ hadoop fs -rm [-f] [-r|-R] [-skipTrash] <src>


-skipTrash   option bypasses trash, if enabled, and immediately deletes <src>

-f                          If the file does not exist, do not display a diagnostic message or  modify the exit status to reflect an error.

-[rR]                  Recursively deletes directories


$ hadoop fs -rm -r /user/test/sample.txt


Note:

Directories can’t be deleted by -rm command. We need to use -rm -r (recursive remove) command to delete directories and files inside them. Only files can be deleted by -rm command.


