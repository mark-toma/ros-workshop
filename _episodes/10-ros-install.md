---
title: "ROS Installation"
teaching: 30
exercises: 0
questions:
- "Question to be answered?"
objectives:
- "List the objectives here"
keypoints:
- "List the key points here."
keypoints:
- "List more key points?"
---

Installing ROS [fetched from ROS Wiki on xx/xx/xxxx](http://wiki.ros.org/).

1. Setup your computer to accept software from packages.ros.org.

   ```
   sudo sh -c 'echo"deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" >/etc/apt/sources.list.d/ros-latest.list'
   ```

   Mirrors: 


2. Set up your keys

   ```
   sudo apt-key adv--keyserver hkp://ha.pool.sks-keyservers.net:80 --recv-key421C365BD9FF1F717815A3895523BAEEB01FA116
   ```

   If you experienceissues connecting to the keyserver, you can try substituting `hkp://pgp.mit.edu:80` or `hkp://keyserver.ubuntu.com:80` in the previous command.

3. Installation

   First, make sure your Debian package index is up-to-date:

   ```
   sudo apt-get update
   ```

   Desktop-Full Install:

   ```
   sudo apt-get install ros-kinetic-desktop-full
   ```

4. Initialize `rosdep`

   Before you can use ROS, you will need to initialize `rosdep`. `rosdep` enables you to easily install system dependencies for source you want to compile and is required to run some core components in ROS.

   ```
   sudo rosdep init
   rosdep update
   ```

5. Environment setup

   It's convenient if the ROS environment variables are automatically added to your bash session every time a new shell is launched:

   ```
   echo "source /opt/ros/kinetic/setup.bash" >> ~/.bashrc
   source ~/.bashrc
   ```

   You can then confirm that the environment variables are set correctly using a command
   like this:

   ```
   export | grep ROS
   ```

   If everything has worked correctly, you should see a handful of values (showing values for
   environment variables like `ROS_DISTRO` and `ROS_PACKAGE_PATH`) as the output
   from this command. If setup.bash has not been run, then the output of this command
   will usually be empty

6. Dependencies for building packages

    `rosinstall` is a frequently used command-line tool that enables you to easily download many source trees for ROS packages with one command.

   To install this tool and other dependencies for building ROS packages, run:

   ```
   sudo apt-get install python-rosinstall python-rosinstall-generator python-wstool build-essential
   ```
