# lsd-slam


Based on the lsd-slam project developed by tum-vision form Munich University.
* Documentation: http://vision.in.tum.de/research/vslam/lsdslam
* Hosted on Github: https://github.com/tum-vision/lsd_slam

Some adjustments were applied to allow installation of the framework on Ubuntu 14.04, with ROS indigo and catkin.
The package was ONLY tested on Ubuntu 14.04, ROS indigo and catkin.

to install:

0) make sure ROS indigo is installed and working

1) create catkin workspace 
-----------------------------
```bash
   mkdir catkin_ws
   mkdir catkin_ws/src
   cd catkin_ws/src
   catkin_init_workspace
   ```

2) clone this repo (into src directory)
-----------------------------
```bash
   git clone https://github.com/antonays/lsd-slam.git lsd-slam
   ```
   
3) make using catking
-----------------------------
```bash
   cd ../.. (make sure to be in catkin_ws - same level of /src)
   catkin_make
   ```
   
after some time should compile correctly

### Alterations performed:
========================
* in 'catkin_ws/src/lsd_slam/lsd_slam_core/package.xml'  added:
```bash
	<build_depend>cmake_modules</build_depend>
	<run_depend>cmake_modules</run_depend> ```
	

* in catkin_ws/src/lsd_slam/lsd_slam_viewer/package.xml  add:
 ```bash
	<build_depend>cmake_modules</build_depend>
	<run_depend>cmake_modules</run_depend> ```
	
	
* in catkin_ws/src/lsd_slam/lsd_slam_viewer/CMakeFiles.txt  add:
```bash
	find_package(cmake_modules REQUIRED)
	
	add_dependencies(viewer lsd_slam_viewer_generate_messages_cpp)```
	

* in catkin_ws/src/lsd_slam/lsd_slam_core/CMakeFiles.txt  add:
```bash
	find_package(cmake_modules REQUIRED)
	change the following line:
	target_link_libraries(lsdslam ${FABMAP_LIB} ${G2O_LIBRARIES} ${catkin_LIBRARIES} csparse cxsparse X11) 
	add the X11
	
	add_dependencies(lsdslam lsd_slam_viewer_generate_messages_cpp)
	add_dependencies(live_slam lsd_slam_viewer_generate_messages_cpp)
	add_dependencies(dataset lsd_slam_viewer_generate_messages_cpp)```
	


