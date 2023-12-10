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
