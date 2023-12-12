NavigationStack_ROS2
------------------

1. Install navigation stack `sudo apt install ros-humble-navigation2 ros-humble-nav2-bringup ros-humble-turtlebot3*`
2. `gedit ~/.bashrc` addin the follwoing line `export TURTLEBOT3_MODEL=waffle`. save and exit
3. `source .bashrc`
4. `printenv | grep TURTLE`
5. `gazebo` run gazebo. verify FPS at 60 and close the gazebo
6. `ros2 launch turtlebot3_gazebo turtlebot3_world.launch.py`
7. it may take some time to open the new world in gazebo
8. `ros2 run turtlebot3_teleop teleop_keyboard` use this to operate the robot inside the world
9. use the following commands to analyze the topic and connections respectively `ros2 topic list` and `rqt_graph`

Generate a map with SLAM
------------------------
1. `ros2 launch turtlebot3_gazebo turtlebot3_world.launch.py`
2. `ros2 launch turtlebot3_cartographer cartographer.launch.py use_sim_time:=True` here you can see partially generated map
3. `ros2 run turtlebot3_teleop teleop_keyboard` to operate the robot to navigate and generate the full map. consider moving slow and not to crash
wall or other obstacles
4. In the home directory `mkdir maps`
5. `ros2 run nav2_map_server map_saver_cli -f maps/my_map` to save the map in to maps folder
6. stop the all running commands

Map
----
1. we have tow files in the maps folder. pgm is the image of the map. another is yaml file. which has the details of the map
2. `nano my_map.pgm` this is used to store the pixel in x direction and y direction. if you multiply the pixels with resolution you can get the
   real dimension of the map

Quick fix with Nav2
-------------------
1. map not loading correctly or taking too much of time is one of the problem.
2. change the fast DDS to cyclone DDS. `sudo apt update` `sudo apt install ros-humble-rmw-cyclonedds-cpp`
3. tell the ros2 to use the dds `gedit ~/.bashrc`
4. input the following `export RMW_IMPLEMENTATION=rmw_cyclonedds_cpp` in the bashrc file and save it
5. `cd /opt/ros/humble` `cd share/` `ls` `cd turtlebot3_navigation2/`
6. `cd param/` `sudo gedit waffle.yaml` use ctrl + f to find the robot model type. comment it
7. change the above param to `nav2_amcl::DifferentialMotionModel` modify and save it
8. reboot the computer

Navigate using map
------------------
1. `ros2 launch turtlebot3_gazebo turtlebot3_world.launch.py`
2. `ros2 launch turtlebot3_navigation2 navigation2.launch.py use_sim_time:=True map:=maps/my_map.yaml`
3. select `2D pose estimate` and select the direction of the robot indicating
4. select `Nav2 goal` and select the point in the map to where the robot need to go and the oreintation of the robot. once you select the point the robot will start to move
5. right hand ruled used in map coordinate systems. red - X, green - Y
