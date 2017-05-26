# pulson_ros

A ROS wrapper for the PulsON P440 and P410 ultra-wideband radios from Time Domain. 

Node: publishes range information to ROS framework

Beacon: will be asked for range information by the node (can be not connected to the system)

IMPORTANT: Remember to update the path at line 164 of 'src/P4xx/RangeNET/TDMA/rcmIf.c' to the corresponding serial port (usually '/dev/ttyACM0')

Steps to work with a beacon and a node:
1) Build a slotmap in the 'config' folder (use `motion_tests.yaml` as a reference).

2) Plug in both the beacon and the node and run

`roslaunch receiver_beacon_reset.launch receiver_number:=<receiver_no> receiver_path:=/path/to/receiver beacon_number:=<beacon_no> beacon_path:=/path/to/beacon` 

with the proper ID numbers (shown in the module labels) and port numbers to reset the devices.

3) Run 

`roslaunch receiver_beacon_beacon.launch beacon_number:=<beacon_no> beacon_path:=/path/to/beacon slot_map_file_name:=<slotmap name>`

to setup the beacon.

4) After you configure the beacon it is safe to unplug it.

5) Run 

`roslaunch receiver_beacon_launch.launch receiver_number:=<receiver_no> receiver_path:=/path/to/receiver slot_map_file_name:=<slotmap name> robot:=<name>`

to start the communication.

6) Leave the node running and look at range information from the corresponding topic `<robot>/ranges/ranges`.

Note: A similar procedure is followed for launching one node talking to two beacons. You need to have a single slotmap file (see motivation_dynamics.yaml as an example). Remember that the two communications should be on a different slot and on a different channel.

Interfacing with MATLAB 2016b:
1) You will need support for custom messages. In a MATLAB window, run `roboticsAddons`.
2) Select the add-on "Robotics System Toolbox Interface for ROS Custom Messages".
3) Click "Install", select "Install" and wait for the installation to complete.
4) Now you need to generate the custom message. Create a folder somewhere (e.g in the Documents).
5) Copy the whole package (with the name `pulson_ros`) in the created folder.
6) In MATLAB, type `userFolder = '/path/to/folder/you/created/'`.
7) Run `rosgenmsg(userFolder)` to create the custom messages.
8) Follow the MATLAB instructions: a) Edit `javaclasspath.txt` by adding the line `/path/to/folder/you/created/matlab_gen/jar/pulson_ros-0.0.0.jar`, b) Execute `addpath('/path/to/folder/you/created/matlab_gen/msggen')`, c) Run `savepath`.
9) Restart MATLAB and check that everything works by running `rosmsg list`.
