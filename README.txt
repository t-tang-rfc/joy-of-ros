# Have fun with ROS using Joystick

## Warm up

Assuming you have ROS1 *Noetic* installed on Ubuntu 20.04, refer to "Setup Development Environment" in [2] for how to setup a ROS workspace run a simple ROS node.

``` shell
# joy-of-ros is your catkin workspace
mkdir joy-of-ros
cd joy-of-ros
# IMPORTANT for creating a catkin workspace
mkdir src
source /opt/ros/noetic/setup.bash
# yes, compile the blank workspace
catkin_make
source devel/setup.bash
# create a new tmux session
tmux new -s joy-of-ros
# in that new tmux panel
roscore
# in another new tmux panel
rosrun turtlesim turtlesim_node
```

## Robot simulation with joystick

### Preparation

First, clone *this* repo. and all the submodules:
``` shell
git clone --recurse-submodules <url-to-this-repo>
```

Then, build the workspace:
``` shell
cd joy-of-ros
source /opt/ros/noetic/setup.bash
catkin_make
```

### Run the simulation

Since working with ROS usually requires multiple terminals, you can use `tmux` to make life easier.

``` shell
cd joy-of-ros
# source ROS in the parent session
source devel/setup.bash
# create a new tmux session (which will inherit the environment)
tmux new -s joy-sim
# in that new tmux panel
roscore
# in a second tmux panel, launch the joystick node
rosparam set joy_node/dev "/dev/input/js0"
rosrun joy joy_node
# in a third tmux panel, launch the simulation
rosrun joystick-robot-sim simulation_node
```
The last command above will open a GUI window for the simulation.

## Appendix

You can use `jstest` which comes with the `joy` ROS package to check if the joystick is working properly, and find out the mapping of the joystick *axes* and *buttons*, e.g.
``` shell
jstest /dev/input/js0
```

Besides, after you launched the joystick node by `rosrun joy joy_node`, you can use
``` shell
rostopic echo /joy
```
to see the processed joystick data in ROS.

You will find that the output of `jstest` is different from the output of `rostopic echo`, as `jstest` shows the raw input from the joystick, while `rostopic echo` shows the processed data---which is scaled to [-1, 1] for axes and {0, 1} for buttons.
The raw data from the joystick is usually in 16-bit signed integer format.
While the `sensor_msgs/Joy` message in ROS uses 32-bit float for axes and 8-bit integer for buttons, so the data is scaled to fit into these types.
Also, the sign of the axes is inverted, so the positive direction of the axes is opposite to the raw data from the joystick.

## Reference

1. [Beginner's Guide to ROS — Part 1 “The Working Principle” Tutorial](https://deepshiftlabs.medium.com/beginners-guide-to-ros-part-1-the-working-principle-8940e24ea5d)
2. [Beginner’s Guide to ROS — Part 2 “Installation and First Run” Tutorial](https://deepshiftlabs.medium.com/beginners-guide-to-ros-part-2-installation-and-first-run-9f8c9a1a8118)
3. [ROS lecture @GitHub](https://github.com/project-srs/ros_lecture)
