# Capstone

# Introduction
This is a SUTD capstone(fyp) that is about an autonomous quad built to inspect for cracks in the inner ship hull through contact based sonic waves. It is built using ROS Melodic architecture, and is build in collaboration with SUTD's club [Multi-Rotor Society(MRS)](https://github.com/multirotorsociety).

## Installing mavros

1. debian installing
```
sudo apt-get install ros-melodic-mavros
sudo apt-get install ros-melodic-mavros-extras
```

2. install GeographicLib
```
sudo apt-get install geographiclib-*
```

3. source ros and install dataset
- go to [mavros github](https://github.com/mavlink/mavros) and download it
- install dataset
```
./install_geographiclib_datasets.sh
```
4. Launch mavros according to FC
```
roslaunch mavros apm.launch 
roslaunch mavros px4.launch 
```
## TODO
1. figure out why when mavros closes, it becomes a serial killer
