# lsd-slam


Based on the lsd-slam project developed by tum-vision form Munich University.
* Documentation: http://vision.in.tum.de/research/vslam/lsdslam
* Hosted on Github: https://github.com/tum-vision/lsd_slam

Adjustments were made to allow easy compilation of the framework on Ubuntu 16.04, with ROS kinetic and catkin.
The package was ONLY (partially) tested on Ubuntu 16.04, ROS kinetic and catkin.

to install:

0) make sure ROS indigo is installed and working
-----------------------------

1) install dependancies
-----------------------------
```
   sudo apt-get install ros-kinetic-libg2o liblapack-dev libblas-dev freeglut3-dev libqglviewer-dev-qt4 libsuitesparse-dev libx11-dev
```

2) create catkin workspace 
-----------------------------
```bash
   mkdir catkin_ws
   mkdir catkin_ws/src
   cd catkin_ws/src
   catkin_init_workspace
   ```

3) clone this repo (into src directory)
-----------------------------
```bash
   git clone https://github.com/Sinnlosvoll/lsd-slam.git lsd-slam
   ```
   
4) make using catkin
-----------------------------
```bash
   cd ..
   catkin_make
   ```
   

### Motivation of this Fork

This package has been adapted to the current LTS Version of ubuntu and the current supported ros version to make it easier to use the lsd_slam package.
