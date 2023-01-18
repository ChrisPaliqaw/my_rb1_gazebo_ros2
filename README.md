# my_rb1_gazebo_ros2

for necessary setup instructions, first see [my_rb1_description_ros2](https://github.com/ChrisPaliqaw/my_rb1_description_ros2)

for compatibility with project instructions, clone into your ros2 workspace using:
```
git clone https://github.com/ChrisPaliqaw/my_rb1_gazebo_ros2 my_rb1_gazebo
```


# Step 3   Sensor plugins
`gz` shell: launch `gz`, `rviz`, robot state publisher
```
cd ~/ros2_ws
colcon build
. install/setup.bash
ros2 launch my_rb1_gazebo gazebo.launch.py
```
spawn shell
```
cd ~/ros2_ws
colcon build
. install/setup.bash
ros2 launch my_rb1_gazebo spawn_robot_description.launch.py
```
Now you can use teleop
```
ros2 run teleop_twist_keyboard teleop_twist_keyboard
```

# Step 4   ROS2 Control
`gz` shell: script which starts gazebo with an empty world
```
cd ~/ros2_ws
colcon build
. install/setup.bash
ros2 launch my_rb1_gazebo gazebo_empty.launch.py
```

publish the `robot_description` and `control`
```
colcon build
. install/setup.bash
ros2 launch my_rb1_description rb1_ros2_control.launch.py
```
To delete the robot before respawning
```
ros2 service call /delete_entity 'gazebo_msgs/DeleteEntity' '{name: "robot"}'
```
Ensure that your controllers are active
```
user:~$ ros2 control list_controllers
joint_state_broadcaster[joint_state_broadcaster/JointStateBroadcaster] active
diffbot_base_controller[diff_drive_controller/DiffDriveController] active
```
Send a Twist command - the robot should move in gz
```
ros2 topic pub --rate 10 /diffbot_base_controller/cmd_vel_unstamped geometry_msgs/msg/Twist "{linear{x: 0.1, y: 0, z: 0.0}, angular: {x: 0.0,y: 0.0, z: 0.2}}"
```
