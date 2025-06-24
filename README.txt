# Have fun with ROS

=== Warm up

For how to setup a ROS workspace and run a simple ROS node, consult [2] (the part "Setup Development Environment"), assume you have ROS Noetic installed on Ubuntu 20.04.

# JOY-OF-ROS is your catkin workspace
mkdir JOY-OF-ROS
cd JOY-OF-ROS
mkdir src # <- IMPORTANT for creating a catkin workspace
source /opt/ros/noetic/setup.bash
catkin_make <- # yes, compile the blank workspace
source devel/setup.bash
tmux new -s JOY-OF-ROS
---
roscore # in a new tmux panel
rosrun turtlesim turtlesim_node # in a new tmux panel

=== Create a ROS package

cd JOY-OF-ROS/src
catkin_create_pkg qt-ros-sim std_msgs roscpp joy
# then edit CMakeLists.txt and package.xml

=== Clone the pre-configured package

cd JOY-OF-ROS/src
git clone git@github.com:t-tang-rfc/qt-ros-sim.git
# If you clone the pre-configured package, you can skip the previous step.

=== Build the package

cd JOY-OF-ROS
catkin_make
source devel/setup.bash

=== Run the package
rosrun qt-ros-sim simulation_node

=== Put it all together

Open a new tmux window and start the master node:
+++ cmd
roscore
+++
Open another tmux window, set the ros parameter and launch the joy node:
+++ cmd
rosparam set joy_node/dev "/dev/input/js0"
rosrun joy joy_node
+++
Open yet another tmux window and echo the topic:
+++ cmd
rosrun qt-ros-sim simulation_node
+++
When you play with the joystick, you should see the following messages in the terminal:
+++ output
[INFO] [1749533254.763727679]: JoystickController initialized!
[INFO] [1749533256.630911878]: Joy callback called!
[INFO] [1749533256.650830497]: Joy callback called!
[INFO] [1749533256.670615996]: Joy callback called!
...
+++

=== Reference

1. [Beginner's Guide to ROS — Part 1 “The Working Principle” Tutorial](https://deepshiftlabs.medium.com/beginners-guide-to-ros-part-1-the-working-principle-8940e24ea5d)
2. [Beginner’s Guide to ROS — Part 2 “Installation and First Run” Tutorial](https://deepshiftlabs.medium.com/beginners-guide-to-ros-part-2-installation-and-first-run-9f8c9a1a8118)
3. [ROS lecture @GitHub](https://github.com/project-srs/ros_lecture)

