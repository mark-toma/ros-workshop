---
title: "ROS Overview"
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

The Robotics Operating System is not an operating system in the way that Windows and Linux are. It is a structured communication layer built above a host operating system - usually Linux - thatprovides the tools to enable integrated networking between different computingclusters. 

## Minimal example using TurtleSim

1. **Installing TurtleSim**

   Use a command like this:

   ```
   sudo apt-get install ros-indigo-turtlesim
   ```
   {: .bash}

2. **Starting TurtleSim** 

   In three separate terminals, execute these three commands:

   ```
   roscore
   rosrun turtlesim turtlesim_node
   rosrun turtlesim turtle_teleop_key
   ```
   {: .bash}

   This window shows a simulated, turtle-shaped robot that lives in a square world. If you give your third terminal (the one executing the `turtle_teleop_key` command) the input focus and press the Up, Down, Left, or Right keys, the turtle will move in response to your commands, leaving a trail behind it.

   ![]()

3. **Packages**

   All ROS software is organized into packages. A ROS package is a coherent collection of files,
   generally including both executables and supporting files, that serves a specific purpose.
   In the example, we used two executables called `turtlesim_node` and `turtle_teleop_key`,
   both of which are members of the turtlesim package.

   List all installed ROS packages using this command in a new terminal window:

   ```
   rospack list
   ```
   {: .bash}

   Each package is defined by a **manifest**, which is a file called `package.xml`. This file
   defines some details about the package, including its name, version, maintainer, and dependencies.
   The directory containing `package.xml` is called the **package directory**. 

   To find a package:

   ```
   rospack find <package-name>
   ```
   {: .bash}

4. **The ROS Master**

   Let’s shift gears and talk now about how to actually execute some ROS software. One of the basic goals of ROS is to enable roboticists to design software as a collection of small, mostly independent programs called **nodes** that all run at the same time. For this to work, those nodes must be able to communicate with one another. The part of ROS that facilitates this communication is called **the ROS master**. To start the master, use this command:

   ```
   roscore
   ```
   {: .bash}

    You should allow the master to continue running for the entire time that you’re using
   ROS. One reasonable workflow is to start `roscore` in one terminal, then open other terminals
   for your “real” work. There are not many reasons to stop `roscore`, except when you’ve
   finished working with ROS. When you reach that point, you can stop the master by typing
   Ctrl-C in its terminal.

5. **Nodes**

   Once you’ve started `roscore`, you can run programs that use ROS. A running instance of a
   ROS program is called a **node**.

   *Starting nodes*: The basic command to create a node (also known as “running a ROS program”)
   is `rosrun`

   ```
   rosrun <package-name> <executable-name>
   ```
   {: .bash}

   ROS provides a few ways to get information about the nodes that are running at any particular time. To get a list of running nodes, try this command:

   ```
   rosnode list
   ```
   {: .bash}

   You’ll see a list of three nodes:

   ```
   /rosout
   /teleop_turtle
   /turtlesim
   ```
   {: .output}

   The /rosout node is a special node that is started automatically by `roscore`. Its purpose is somewhat similar to the standard output (i.e. `std::cout`) that you might use in a console program.

   *Inspecting a node*: You can get some information about a particular node using this command:

   ```
   rosnode info <node-name>
   ```
   {: .bash}

   The output includes a list of topics for which that node is a **publisher** or **subscriber** -- the services offered by that node, its Linux process identifier (PID), and a summary of the connections it has made to other nodes.

   *Killing a node*: To kill a node you can use this command:

   ```
   rosnode kill <node-name>
   ```
   {: .bash}

   (Do not implement it now)

6. **ROS Topics**

7. **Console Commands**

   ​

## Create a ROS Workspace (Catkin Workspace)

A [catkin workspace](http://wiki.ros.org/catkin/workspaces#Catkin_Workspaces) is a folder where you modify, build, and install catkin packages.The following is the recommended and typical catkin workspace layout:

```
workspace_folder/         -- WORKSPACE
  src/                    -- SOURCE SPACE
    CMakeLists.txt        -- The 'toplevel' CMake file
    package_1/
      CMakeLists.txt
      package.xml
      ...
    package_n/
      CATKIN_IGNORE       -- Optional empty file to exclude package_n from being processed
      CMakeLists.txt
      package.xml
      ...
  build/                  -- BUILD SPACE
    CATKIN_IGNORE         -- Keeps catkin from walking this directory
  devel/                  -- DEVELOPMENT SPACE (set by CATKIN_DEVEL_PREFIX)
    bin/
    etc/
    include/
    lib/
    share/
    .catkin
    env.bash
    setup.bash
    setup.sh
    ...
  install/                -- INSTALL SPACE (set by CMAKE_INSTALL_PREFIX)
    bin/
    etc/
    include/
    lib/
    share/
    .catkin 
    env.bash
    setup.bash
    setup.sh
    ...
```

A catkin workspace can contain up to four different *spaces* which each serve a different role in the software development process.

1. *Source Space:* `workspace_folder/src/`: contains the source code of catkin packages.  This is where you can clone source code for the packages you want to build. Each folder within `src/` contains one or more catkin packages.
2. *Build Space:*  `workspace_folder/src/build/`: The build space is where CMake is invoked to build the catkin packages in the source space. CMake and catkin keep their cache information and other intermediate files here.
3. *Development (Devel) Space:* `workspace_folder/src/devel`: The development space (or `/devel` space) is where built targets are placed prior to being installed. The way targets are organized in the `/devel` space is the same as their layout when they are installed. This provides a useful testing and development environment which does not require invoking the installation step. 
4. *Install Space:* `workspace_folder/src/install`: Once targets are built, they can be installed into the install space by invoking the install target, usually with `make install`

Let's create and build a catkin workspace:

```
mkdir -p ~/catkin_ws/src
cd ~/catkin_ws/
catkin_make
```
{: .bash}

Running it the first time in your workspace, it will create a `CMakeLists.txt` link in your 'src' folder. Additionally, if you look in your current directory you should now have a 'build' and 'devel' folder. Inside the 'devel' folder you can see that there are now several setup.*sh files. Sourcing any of these files will overlay this workspace on top of your environment. 

Now source your new setup.*sh file:

```
source devel/setup.bash
```
{: .bash}

To make sure your workspace is properly overlaid by the setup script, make sure `ROS_PACKAGE_PATH` environment variable includes the directory you're in.

```
echo $ROS_PACKAGE_PATH
```
{: .bash}

```
/home/<youruser>/catkin_ws/src:/opt/ros/kinetic/share
```
{: .output}

