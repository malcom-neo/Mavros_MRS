# Capstone

## Introduction
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

## Commands
- Changing flight mode
  service call /mavros/set_mode with mode from apm in custom mode
  
  Mode list: GUIDED_NOGPS AVOID_ADSB THROW BRAKE POSHOLD AUTOTUNE FLIP SPORT DRIFT OF_LOITER LAND POSITION CIRCLE RTL LOITER GUIDED AUTO ALT_HOLD ACRO STABILIZE

- Arming 
  service call /mavros/cmd/arming
 
## mavros param
- Circuit Breaker
  - CBRK_SUPPLY_CHK
  - CBRK_USB_CHK, disable USB and supply voltage check
  

## TODO
1. figure out why when mavros closes, it becomes a serial killer

## Changes made to note
1. timesynce
2. launch file baudrate, port
