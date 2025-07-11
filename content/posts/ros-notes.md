---
author: amrithmmh
category:
  - uncategorized
date: "2022-06-21T03:09:17+00:00"
guid: https://armphibian.wordpress.com/?p=102
title: ROS 1 notes
url: /2022/06/21/ros-2-notes/

---
![](/wp-content/uploads/2022/06/screenshot-from-2022-06-21-08-45-12.png?w=1024)[https://www.ros.org/](https://www.ros.org/)

personal notes while learning ROS2

**ROS1 installation**:

>Using Ubuntu as a base operating system, follow the installation guide on [ROS wiki](http://wiki.ros.org/ROS/Installation)

**ROS1 Workspaces**:

>Catkin is a build system used to make workspaces, packages and libraries for ROS

_To create a new workspace:_

```
$ mkdir -p ~/catkin_ws/src
$ cd ~/catkin_ws/
$ catkin_init_workspace
$ catkin_make
or
$ catkin build
```

> use [this](https://catkin-tools.readthedocs.io/en/latest/cheat_sheet.html) cheatsheet to refer catkin commands
>
> another interesting [gist](https://gist.github.com/sytrus-in-github/5fae81ead75ad27c1220bd4ec499e36b) that has a summary of commands

_source devel/setup.bash_ after making a new workspace, also after creating a new package

**Create a package**:

```
$ cd ~/catkin_ws/src
$ catkin_create_pkg beginner_tutorials std_msgs rospy roscpp
```

`catkin_create_pkg <pkg_name> [<depend_pkg_1> <depend_pkg_2> ...]`

**Build a package**:

source setup.bash and from your workspace run

```
# In a catkin workspace
$ catkin_make
$ catkin_make install  # (optionally)
```

helpful websites:

[http://wiki.ros.org/ROS/Tutorials](http://wiki.ros.org/ROS/Tutorials)

video tutorials:

{{< youtube 0BxVPCInS3M >}}&list=PLE-BQwvVGf8HOvwXPgtDfWoxd4Cc6ghiP&ab\_channel=RoboticSystemsLab%3ALeggedRoboticsatETHZ%C3%BCrich

youtube playlist: [https://www.youtube.com/watch?v=0BxVPCInS3M&list=PLE-BQwvVGf8HOvwXPgtDfWoxd4Cc6ghiP&ab\_channel=RoboticSystemsLab%3ALeggedRoboticsatETHZ%C3%BCrich](https://www.youtube.com/watch?v=0BxVPCInS3M&list=PLE-BQwvVGf8HOvwXPgtDfWoxd4Cc6ghiP&ab_channel=RoboticSystemsLab%3ALeggedRoboticsatETHZ%C3%BCrich)
