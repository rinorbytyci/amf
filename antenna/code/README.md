# Autonomous Movement Framework - Server

This repository contains the updated code for the AMF Server framework. This update should enable the support for dynamically switching the NetID of the 3DR Antenna before a drone is sent.

The NetID is updated by sending a custom command to the antenna via the serial port. This is highly based on Sikset (a command-line tool developed to program the 3DR antennas), found at: https://github.com/mr337/sikset

The `database` folder contains the sqlite database which is populated with the current drones, their names and their respective NetID. The database should be viewed or modified with an sqlite editor tool (such as DB Browser in Ubuntu App Store). 

Using the SQLite browser, we can edit and add/remove drones from the database as required. The Figure below displays the structure of the `drones` table.

![drones table](http://i.imgur.com/qdNyqxK.png)

The fields in the table are:

`id` - A unique id for each row entry in the table. It is set as auto-increment so there should be no need to specify this when adding a new drone to the database.

 `drone` - A text field, used to label the drone so it can be identified easier at a later time from the operator.
 
 `netid` - The NetID the drone is configured to use. When the application selects a random drone from the table, it will switch the Antenna connected to the PC to this NetID so it can enable communications with the drone.
 
 `available` - A field indicating whether a drone is available or not. If this is set to `1`, it means that the drone is ready to fly and can be selected from the application at random, otherwise if this is set to `0`, the drone will be excluded from the application. Additionally, once a drone has been picked by the server to perform its operation, the `available` field of that drone will be set to `0` so that in the next mission, the drone is excluded.
 
 ### Server

The Server runs on port 8080 and accepts commands directly from the Android app. Once a request is accepted from the application, a drone is chosen randomly from the databse, the NetID of the sending antenna is switched to match that of the drone and then the flight plan is sent to the drone. Upon completion, the drone is marked as not available in the database.

To start the server, please install all the prerequisite applications from the main README and use the following command:

`python server.py`


 ### To-do:

1. Figure out a better timing for the serial commands to be sent as sometimes it misses an update.
2. Make the NetID change request asynchronously so that the application does not hang.
