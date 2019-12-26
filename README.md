# LeGO-LOAM for kitti dataset

This repository contains modified code of LeGO-LOAM to run and evaluate with kitti-data set. If you run the code, you'll get the trajectory results in KITTI groundtruth format and directly evalutate with EVO-eval kit. 

Modified code

1. utility.h
```
extern const string pointCloudTopic = "/kitti/velo/pointcloud"; <- you should check your own bag file topic

//param for vel-64
extern const int N_SCAN = 64;
extern const int Horizon_SCAN = 1800;
extern const float ang_res_x = 0.2;
extern const float ang_res_y = 0.427;
extern const float ang_bottom = 24.9;
extern const int groundScanInd = 50;
```
2. featureAssociation.cpp
```
float s 10 * (pi->intensity - int(pi->intensity)); -> float s = 1;

// to delete all the code that corrects point cloud distortion
TransformToEnd(&cornerPointsLessSharp->points[i], &cornerPointsLessSharp->points[i]); -> removed
TransformToEnd(&surfPointsLessFlat->points[i], &surfPointsLessFlat->points[i]); -> removed

*Notes: The parameter "loopClosureEnableFlag" is set to "true" for SLAM. 
```
Reference : https://github.com/RobustFieldAutonomyLab/LeGO-LOAM/issues/12

## Dependency

- [ROS](http://wiki.ros.org/ROS/Installation) (tested with indigo and kinetic)
- [gtsam](https://github.com/borglab/gtsam/releases) (Georgia Tech Smoothing and Mapping library, 4.0.0-alpha2)

 ```
  wget -O ~/Downloads/gtsam.zip https://github.com/borglab/gtsam/archive/4.0.0-alpha2.zip
  cd ~/Downloads/ && unzip gtsam.zip -d ~/Downloads/
  cd ~/Downloads/gtsam-4.0.0-alpha2/
  mkdir build && cd build
  cmake ..
  sudo make install
  ```

## Compile

You can use the following commands to download and compile the package.

```
cd ~/catkin_ws/src
git clone https://github.com/Mitchell-Lee-93/kitti-lego-loam.git
cd ..
rosdep install --from-paths src --ignore-src -r -y
catkin_make
```

## Making new bagfile from kitti dataset 


## Run the package

1. Run the launch file:
```
roslaunch lego_loam run.launch
```
Notes: The parameter "/use_sim_time" is set to "true" for simulation, "false" to real robot usage.


2. Play existing bag files:
```
rosbag play *.bag --clock 
```

## Original code from
https://github.com/RobustFieldAutonomyLab/LeGO-LOAM/blob/master/README.md
