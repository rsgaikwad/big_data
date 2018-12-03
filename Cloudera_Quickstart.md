# Getting Started with the Cloudera Quickstart VM
## Purpose
The purpose of this post is to provide instructions on how to get started with the Cloudera Quickstart VM and what are some of the main things to know about the VM. This includes where to find certain configuration files, how to setup certain things that will make your life easier and more.

## About the Cloudera Quickstart VM
Overview
The Cloudera Quickstart VM is a Virtual Machine that comes with a pseudo distributed version of Hadoop preinstalled on it along with the main services that are offered by Cloudera. This includes the Cloudera Manager and Impala as the most notable.

## Some Requirements
Make sure your computer is setup to allow virtualization. This can be set in your bios on startup.
To use the Cloudera Manager, you will need to allocate 10GB to your VM and 2 Virtual CPU Cores.
The Cloudera Manager comes disabled by default, and all the Hadoop daemons are started up on startup and run just fine without it. so you don’t absolutely need the Cloudera Manager.

## Getting Started
Downloads
General Downloads
http://www.cloudera.com/downloads.html

Latest Quickstart VM
http://www.cloudera.com/downloads/quickstart_vms.html

Official Documentation
https://www.cloudera.com/documentation/enterprise/5-5-x/topics/cloudera_quickstart_vm.html

## Importing into VirtualBox
  Download the Quickstart VM with the above links
  Open VirtualBox
  Click on File -> Import Appliance
  Select the Quickstart VM you just download
  Click Continue
  Optional: Double click on the name, and change it to whatever you want.
    Click Import
    Wait for the machine to import and when it is done, it will be list in the window to startup
 
## Recommended VirtualBox Configurations
  Right click on the VirtualMachine and click Settings

  Setup the VM to allow you to copy and paste from that machine to your local and vice-versa

  Click on General -> Advanced

  Set Shared Clipboard to Bidirectional

  Setup port forwarding from port 2222 to port 22 to allow SSH to the machine

  Click on Network -> Advanced -> Port Forwarding

  Add a new entry

    Name: 2222

    Host Port: 2222

    Guest Port: 22

  Accessing the VM

  SSH’ing to the Machine

  Default SSH Credentials: cloudera/cloudera

  Host to connect to: localhost

Because of the Recommended VirtualBox Configuration above, we’re forwarding connections from port 2222 to 22. So you would want to use port 2222 to connect.

__Linux/Mac

  Open a command line terminal

  Use the ssh command to login

  ssh -p 2222 cloudera@localhost

  Enter the password

__Windows

  Open putty

  Set localhost as the Host Name

  Set 2222 as the port

  Connection Type: SSH

  Click open

  Enter the password

__Setup password-less SSH (Optional)

  Generate a public and private key locally

  You can follow these instructions:
  https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/
 
  Login to the machine with the instructions above
 
  create the ~/.ssh directory
    mkdir ~/.ssh
  
  Create the file ~/.ssh/authorized_keys
  Open file
    nano ~/.ssh/authorized_keys
  
  Add your public key to the authorized_keys file
 
  Save the authorized_keys file

  Change permissions of .ssh

    chmod 700 ~/.ssh

  Change permissions of the ~/.ssh/authorized_keys

    chmod 600 ~/.ssh/authorized_keys

 	  Change permissions of: chmod 740 /home/cloudera/

    chmod 740 ~/

  Now if you try SSH’ing to the machine, you shouldn’t have to provide the password


__Copying Files to the VM

  SCP

  Open a command line terminal

  Use the following command:

    scp -P 2222 {PATH_TO_FILE_ON_LOCAL} cloudera@localhost:{DESTINATION_PATH_ON_VM}

  FileZilla or anther FTP App

  Open your desired FTP Application

  Create a new connection

  Host: localhost

  Username: cloudera

  Password: cloudera

  Port: 22

  Connect
 
__Optional Setup Tasks

  Configure Apache Spark to Connect to Hive
  
  If you’re intending to use Apache Spark, you will also probably want to connect to Hive using SparkSQL so you can interact with that relational store. To do this you need to include the hive-site.xml file in the spark configurations so Spark knows how to interact with Hive. If you don’t do this, the app will still run, but you wont be able to view the same tables you have in Hive and you wont be able to store data in tables.

__SSH into the Machine

  Login as root
  
  Create a symlink to Link the hive-site.xml in the spark conf directory

  ln -s /usr/lib/hive/conf/hive-site.xml /usr/lib/spark/conf/hive-site.xml

Configure Apache Spark History Server to allow you to view previously ran Spark jobs
If you’re intending to use Apache Spark, you may end up trying to view past runs via the Apache Spark History Server. There is a small issue right off the bat with the Quickstart VM where you can’t view past runs, because of a permissions issue with the applicationHistory directory in HDFS (/user/spark/applicationHistory). The spark user, is not able to read the contents of the directory. You can follow these steps to fix this:

__SSH into the Machine

  Login as hdfs user
  Run “$ sudo su” to login as root, then “$ su hdfs”
  Change the permissions of the applicationHistory directory under the spark home directory in hdfs
  hadoop fs -mkdir -p /user/spark/applicationHistory
  hadoop fs -chown spark:spark /user/spark/applicationHistory
  hadoop fs -chgrp spark /user/spark/applicationHistory/*

  Now when you visit the Apache Spark History server you will see any past jobs that have ran
  Using Services
  Using Beeline to connect to Hive
  Beeline is a new command line shell that is supported by HiveServer2. It is recommended to use this over the normal hive shell since it supports better security and functionality.

__Credentials
cloudera/cloudera

Starting Shell with beeline Command
beeline
This will start the beeline shell.

Note: If you were to run a command such as “show tables” to list the hive tables in the currently selected database at this time you will get the following error:
No current connection

This is because you haven’t technically connected to the HiveServer2 to be able to run hive commands.

To connect you can run the following command. This will prompt you for credentials.

beeline> !connect jdbc:hive2://localhost:10000
To avoid having to enter credentials each time, you can include the username and password in the connect statement like so:

beeline> !connect jdbc:hive2://localhost:10000 cloudera cloudera
Starting Shell with beeline Command and arguments
Instead of having to use the connect command upon starting the beeline shell, you can automatically connect to the HiveServer2 using command line arguments.

beeline -u jdbc:hive2://localhost:10000/default -n cloudera -p cloudera
Shutting down the Shell

beeline> !quit
URLs and Credentials
Cloudera Manager
URL: http://quickstart.cloudera:7180/cmf/home

Credentials: cloudera/cloudera

Hue
URL: http://quickstart.cloudera:8888/accounts/login/

Credentials: cloudera/cloudera

Resource Manager
URL: http://quickstart.cloudera:8088/cluster

Credentials: None

Job History
URL: http://quickstart.cloudera:19888/jobhistory

Credentials: None

HBase Master UI
URL: http://quickstart.cloudera:60010/master-status

Credentials: None

Oozie UI
URL: http://quickstart.cloudera:11000/oozie/

Credentials: None

Apache Solr
URL: http://quickstart.cloudera:8983/solr/#/

Credentials: None

Apache Spark History
URL: http://quickstart.cloudera:18088/

Credentials: None

MySQL
Host: localhost

Credentials: root/cloudera

Example Connection
$ mysql -u root -p

cloudera

Beeline
Host: localhost

Port: 10000

Credentials: cloudera/cloudera

Example Connection
$ beeline -u jdbc:hive2://localhost:10000/default -n cloudera -p cloudera

Useful File System Paths
Configuration Files:
/usr/lib/


