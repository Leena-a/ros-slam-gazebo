# ros-slam-gazebo
Steps for virtually constructing and updating a map for ROS robots using simultaneous localization and mapping (SLAM) in the Gazebo environment.

The steps below uses ROS melodic 1.14.11 version, which runs on ubuntu 18.04.

## TurtleBot3 

### Installation
Install TurtleBot3 via Debian Packages:
```
$ sudo apt-get install ros-melodic-dynamixel-sdk
$ sudo apt-get install ros-melodic-turtlebot3-msgs
$ sudo apt-get install ros-melodic-turtlebot3
```

Set TurtleBot3 Model Name:
Setting the default `TURTLEBOT3_MODEL` name to the model.

In case of TurtleBot3 Burger:
```
$ echo "export TURTLEBOT3_MODEL=burger" >> ~/.bashrc
```
In case of TurtleBot3 Waffle Pi:
```
$ echo "export TURTLEBOT3_MODEL=waffle_pi" >> ~/.bashrc
```

>In the following steps, TurtleBot3 Burger will be the model to work on.

### Gazebo Simulation 

>The following steps assumes that the user have proper Gazebo version for ROS melodic installed.

The TurtleBot3 Simulation Package requires `turtlebot3` and `turtlebot3_msgs` packages as prerequisite.
```
$ cd ~/catkin_ws/src/
$ git clone -b melodic-devel https://github.com/ROBOTIS-GIT/turtlebot3_simulations.git
$ cd ~/catkin_ws && catkin_make
```

After building the workspace with catkin_make, source it by running:
```
source ~/catkin_ws/devel/setup.bash
```
Three simulation environments are prepared for TurtleBot3: Empty World, TurtleBot3 World, TurtleBot3 House.

>In the following steps, TurtleBot3 World will be the environment to work on.


In order to teleoperate the TurtleBot3 with the keyboard, launch the teleoperation node with below command in a new terminal window:
```
$ roslaunch turtlebot3_teleop turtlebot3_teleop_key.launch
```

Launching the Gazebo environment:
```
$ export TURTLEBOT3_MODEL=burger
$ source ~/catkin_ws/devel/setup.bash
$ roslaunch turtlebot3_gazebo turtlebot3_world.launch
```

Operate TurtleBot3 in a new terminal tab:
```
$ roslaunch turtlebot3_teleop turtlebot3_teleop_key.launch
```

### SLAM Simulation

Launch Simulation World:
```
$ export TURTLEBOT3_MODEL=burger
$ source ~/catkin_ws/devel/setup.bash
$ roslaunch turtlebot3_gazebo turtlebot3_world.launch
```

In a new terminal tab, run the SLAM node. Gmapping SLAM method is used by default:
```
$ export TURTLEBOT3_MODEL=burger
$ roslaunch turtlebot3_slam turtlebot3_slam.launch slam_methods:=gmapping
```

Open a new terminal and run the teleoperation node from the Remote PC:
```
$ export TURTLEBOT3_MODEL=burger
$ roslaunch turtlebot3_teleop turtlebot3_teleop_key.launch
```

As can be seen, the map is created successfully:

![TurtleBot3Mapping](https://user-images.githubusercontent.com/52850659/123856396-b8aa7f00-d929-11eb-97b4-285dae518367.png)

Using the following command, the map is saved in .pgm extension:
```
rosrun map_server map_saver -f ~/map
```

![SLAM map](https://user-images.githubusercontent.com/52850659/123856558-e98ab400-d929-11eb-9d8e-d7b36e3f3f8e.png)



