# ros2-jazzy-notes
Personal ROS 2 Jazzy learning notes and commands

# ROS 2 Jazzy Setup Notes

Personal notes documenting my first steps with **ROS 2 Jazzy Jalisco** on Ubuntu, including setup, common errors, and running `turtlesim`.

---

## System
- OS: Ubuntu 24.04 (Noble)
- ROS 2: Jazzy Jalisco

---

## Problem: ROS Commands Not Found

Before sourcing ROS:


ros --version
ros2

## Locate ROS Installation
ls /opt/ros/


Output:

jazzy


## ROS is installed at:

/opt/ros/jazzy

Source ROS 2
source /opt/ros/jazzy/setup.bash


## Verify:

printenv ROS_DISTRO


Output:

jazzy


After this, ros2 commands work.

## Install and Check Turtlesim
sudo apt install ros-jazzy-turtlesim


## Check executables:

ros2 pkg executables turtlesim

## Run Turtlesim
ros2 run turtlesim turtlesim_node


## Expected:

Turtlesim window opens

Turtle spawns successfully

## Control the Turtle

# Open a new terminal and source ROS again:

source /opt/ros/jazzy/setup.bash
ros2 run turtlesim turtle_teleop_key


# Use arrow keys to move the turtle.

## Create a ROS 2 Workspace
mkdir ~/Desktop/ros2_ws
cd ~/Desktop/ros2_ws
mkdir src

## Common Mistakes

❌ Using ros instead of ros2
❌ Running ROS commands before sourcing
❌ Missing spaces in Linux commands

Example:

cd/opt/ros    # wrong
cd /opt/ros   # correct

## Key Rule

Every new terminal must source ROS:

source /opt/ros/jazzy/setup.bash


## If ros2 is not found, the environment is not sourced.


```bash
ros --version
ros2


