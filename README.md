#Introduction
This set of utilities is created to support the smashbox analytical setup consisting of smashbox-sim, InfluxDB and Grafana web interface  to display the results in real-time. 

``https://github.com/mrow4a/smashbox-sim``

``https://hub.docker.com/r/mrow4a/smashbox/``

#Smashbox-tools
<pre>

   PROJECT TREE
   │
   ├── admin-tools
   │   ├── backup                                 : utility to backup the contents of the InfluxDB database localy.
   │   ├── backup.config                          : configuration script for pull/push of the InfluxDB database
   │   └── docker-smash-restart                   : Utility which will restart docker only if it is not running smashbox-deamon. If it is running, it will execute loop until smashbox-deamon finishes 
   └── sim-tools
       ├── analyse                                : script to aggregate and summarize data collection stored in the InfluxDB database 
       └── analyse.config                         : configuration script to analyse script   
   
</pre>

#Analyse

Running the analyse script with default config file analyse.config

``./analyse``

Running the analyse script with config file located in different place of with different name

``./analyse examples/demo.config``

Config JSON has a structure:

<pre>

   YOUR_NAME_FOR_FILE.config
   │
   ├── config                              
   │   ├── remote_storage_server           : ip address of the remote server, default port is 8086
   │   ├── remote_database                 : name of the remote database 
   │   ├── remote_storage_user             : name of the user with read permision on the database 
   │   └── remote_storage_password         : password of the user with read permision on the database
   │
   ├── summary    
   |  ...
   │   └── nth summary_dict                : dictionary responsible for one of the tests configurations
   |      ├── show                         : True/False
   |      ├── alias                        : Alias for the test
   |      ├── server                       : Address of the server under the test, as specified in smashbox-sim configuration
   |      ├── runid                        : [RUNID1,RUNID2,..] runid of the test machine, as specified in smashbox-sim configuration under RUNID
   |      ├── start_time                   : Start time in days, e.g. "30d" means it will aggregate from minus 30 days
   |      ├── stop_time                    : Stop time in days, e.g. "1d" means it will aggregate till minus 1 day
   │      └── graph                        : [GRAPH_DICT1,GRAPH_DICT2,..] array responsible for one of the graph configurations
   |
   └── compare                             
      ...
       └── nth compare_dict                : dictionary responsible for one of the tests configurations
          ├── show                         : True/False
          ├── test_name                    : Test name
          ├── testdirstruct                : Test directory structure
          ├── alias                        : Alias for the test
          ├── server                       : [SERVER1,SERVER2,..] Address of the server under the test, as specified in smashbox-sim configuration
          ├── runid                        : Runid of the test machine, as specified in smashbox-sim configuration under RUNID
          ├── start_time                   : Start time in days, e.g. "30d" means it will aggregate from minus 30 days
          ├── stop_time                    : Stop time in days, e.g. "1d" means it will aggregate till minus 1 day
          └── graph                        : [GRAPH_DICT1,GRAPH_DICT2,..] array responsible for one of the graph

</pre>

#GRAPH TYPES

Graph types are described as below and you can browse an examples of them under ``examples`` directory.

<pre>

   GRAPH TYPES STRUCTURE
   │
   ├── summary dict tests                              
   │   ├── compare-time-test               : used to compare client to server/server to client sync times during different hours of the day, for the specified server
   │   ├── compare-files-test              : used to compare number of synced files as a cumulative distribution of all the measurements and to display single sync failure ratio under different tests and test machines, for the specified server
   │   ├── cm-histogram-sync-test          : Cumulative distribution of synchronisation times for different test types, for the specified server 
   │   └── cm-histogram-transf-test        : Detailed description of the synchronisation runs in terms of the transfered data comparing different types of the tests and test machines for the specified server
   |
   └── compare dict tests                  
       ├── compare-files-server            : Used to compare number of synced files as a cumulative distribution of all the measurements and to display single sync failure ratio for differents servers; specified test machine and test  
       ├── cm-histogram-sync-test          : Cumulative distribution of synchronisation times for different servers; specified test machine and test 
       └── cm-histogram-transf-test        : Detailed description of the synchronisation runs in terms of the transfered data comparing different servers; specified type of the test and test machine
       
</pre>
