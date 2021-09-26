# Hilti-challenge_details

## Introduction

In the framework of the International Conference on Intelligent Robots and Intelligent Systems 2021, a call for participation in the HILTI-challenge was launched by Prof. Davide Scaramuzza and Giovanni Cioffi from the University of Zürich with the collaboration of Danwei Wang, Christian Laugier, Philippe Martinet and Yufeng Yue. In response, the research group of the University of Ibagué joins this event by means of this document. In response, the research group of the University of Ibagué joins this event by means of this document.

<img src = "https://github.com/cristianrubioa/Hilti-challenge_details/blob/main/images/methodology_for_hilti.png" width="1000">

## Estimated Trajectories

The 12 trajectories obtained were generated according to the requested formats. Their files can be obtained through the following link: [https://www.hilti-challenge.com/dataset.html](https://hilti-challenge.com/dataset.html).

## Overview
The challenge was addressed based on the LOAM  method and the [A-LOAM](https://github.com/HKUST-Aerial-Robotics/A-LOAM) github repository, in which the trajectory is recovered using a batch optimisation method that processes segmented data sets with added constraints. By decomposing the original problem, a simpler problem such as online motion estimation is solved first. Then, the mapping is performed as a batch optimization (similar to iterative closest point (ICP) methods) to produce high-precision motion estimates and maps.
The method used does not report the use of future or predicted information, packet adjustment or loop closure. A-LOAM is an advanced implementation of LOAM (J. Zhang and S. Singh. LOAM: Lidar Odometry and Mapping in Real-time), which uses Eigen and Ceres Solver to simplify the code structure. The code has been modified from the original version to a cleaner and simpler version, without complicated mathematical derivations and redundant operations. Referred to by the authors as a good learning material for SLAM beginners.

##  Used Sensors 
In this implementation only the LiDAR sensor data was used by subscribing to the topic ```/os_cloud_node/points```.

## Execution Environment 

#### Used Hardware

| Command | Description |
| --- | --- |
| OS | **Versión**: Ubuntu 18.04.5 LTS - **Kernel**: Linux 4.15.0-137-generic |
| CPU | Intel(R) Xeon(R) Gold 6152 CPU @ 2.10GHz 4 Dual-core |
| RAM | **kB**: 65947352 kB - **GB**: 65,95 GB |
| ROS | **Distro**: Melodic - **Versión**: 1.14.10 |

## Evaluation Metric
The integrity and precision of the estimated trajectories were calculated from the evaluation system of the same challenge [hiltislamchallenge/evaluation-evo](https://github.com/hemi86/hiltislamchallenge/tree/master/evaluation-evo). Where all trajectories have to be provided in the TUM format [cristianrubioa/extract_trajectory](https://github.com/cristianrubioa/extract_trajectory). 

```
#timestamp tx ty tz qx qy qz qw
1.403636580013555527e+09 0.0 0.0 0.0 0.0 0.0 0.0 0.0
```

* **timestamp** (float) gives the number of seconds.
* **tx ty tz** (3 floats) give the position, where t is the translation vector.
* **qx qy qz qw** (4 floats) give the orientation, where q is a quaternion describing the rotation.

**Note:** with each value separated by a space. Names of the provided reference files should not be changed as they indicate which frame the reference is in.



## Simulation Results 

|         Sequences           |        Name_trayectory     | Poses | Path_length [m] | Time [s] |  ATE  |
|:---------------------------:|:--------------------------:|:-----:|:---------------:|:--------:|:-----:|
| RPG Drone Testing Arena     | uzh_tracking_area_run2.txt |  893  | 78.151          | 89.299   | 0.492 |
| IC Office                   | IC_Office_1.txt            |  2004 | 171.163         | 200.301  | -     |
| Office Mitte                | Office_Mitte_1.txt         |  2640 | 214.265         | 263.913  | -     |
| Parking Deck                | Parking_1.txt              |  5823 | 556.871         | 582.212  | -     |
| Basement                    | Basement_1.txt             |  1129 | 74.981          | 112.803  | 0.297 |
| Basement 3                  | Basement_3.txt             |  3306 | 118.793         | 330.616  | -     |
| Basement 4                  | Basement_4.txt             |  3503 | 113.898         | 350.224  | 0.237 |
| Lab                         | LAB_Survey_2.txt           |  1356 | 48.089          | 135.513  | 0.116 |
| Construction Site Outdoor 1 | Construction_Site_1.txt    |  1995 | 106.880         | 199.420  |       |
| Construction Site Outdoor 2 | Construction_Site_2.txt    |  3991 | 218.970         | 399.117  | 1.596 |
| Campus 1                    | Campus_1.txt               |  4298 | 225.502         | 429.730  | -     |
| Campus 2                    | Campus_2.txt               |  3747 | 194.664         | 374.725  | 0.625 |


## Used Parameters

1. Modify the scanRegistration.cpp file.
```cpp
scanID = int((angle + 45) / 1.428 + 0.2);

// angle: angle of vertical elevation.  
// 45: The vertical filter (lower vertical angle).
// 1.428: the interval between each scan. [vFOV/(num_chanel-1)]
// 0.2: for rounding (keep the integer part).
```

2. In [cristianrubioa/extract_trajectory](https://github.com/cristianrubioa/extract_trajectory) rename the topic to ```'/aft_mapped_to_init'``` for A-LOAM method. 
3. In Launch file, rename the name of the topic to the one corresponding to the name of the topic that publishes the LiDAR data (in this case ```/os_cloud_node/points```).
