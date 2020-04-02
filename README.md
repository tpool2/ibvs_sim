IBVS Sim ROS Package
====================

This package contains source files needed to simulate multirotor precision landings using image-based visual servoing. Simulations for landing on stationary ground targets, and moving targets exhibiting ship-like motion are included. The simulation environment is ROS/Gazebo. Also included in this repository are launch files for performing precision landings in hardware using a PX4-enabled multirotor. Use of this package assumes you have installed Ubuntu 18.04 LTS, ROS Melodic, and Gazebo 9.

## Workspace Setup ##

To run simulations, you will need to clone this repository into the `/src` folder of your catkin workspace along with the following additional packages:

* [ROScopter](https://github.com/byu-magicc/roscopter) branch master
* [ROSflight](https://github.com/rosflight/rosflight) branch master
* [ROSflight Plugins](https://github.com/byu-magicc/rosflight_plugins) branch master
* [MAGICC SIM](https://github.com/byu-magicc/magicc_sim) branch master
* [ArUco Localization](https://github.com/wynn4/aruco_localization) branch master

You also need to install MAVROS: `sudo apt install ros-kinetic-mavros`

Before compiling, make sure to update the rosflight submodules:
```bash
cd rosflight
git submodule update --init --recursive
cd ../..
```
Then run `catkin_make`

Don't forget to source your workspace.

Also do `pip install pyqtgraph --user`
Scipy is a python package needed for python2 and python3 scripts found in this repository and roscopter. To ensure that your installation of scipy is compatible with both, do `pip install scipy=1.2 --user` 

Also edit `~/.ignition/fuel/config.yaml` using your favorite text editor so that the website is not `https://api.ignitionfuel.org` but rather `https://api.ignitionrobotics.org`


## Simulating Landing on a Stationary Ground Target ##

To simulate a multirotor landing on a stationary ArUco marker:
`roslaunch ibvs_sim ibvs_sim_nested.launch`

## Simulating Landing on a Moving Barge ##

To simulate landing a multirotor on a barge that is underway:
`roslaunch ibvs_sim ibvs_sim_under_way.launch`

## Testing IBVS Precision Landings in Hardware ##

The file `ibvs_sim/launch/truck_landing.launch` is a ROS launch file for landing on a moving ground target in hardware. It assumes you have a PX4-enabled multirotor with a downward-facing camera. It also assumes you have a ground target that shares its GPS data (lat, lon, etc.) on the ROS network. After launching `ibvs_sim/launch/truck_landing.launch` on the multirotor's on-board computer, launch `ibvs_sim/launch/odroid_target.launch` to launch the ground target GPS publishers (in this launch file we assume that we get GPS data via the Inertial Sense INS ROS node).
