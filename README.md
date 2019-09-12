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
  
  APM mode list: GUIDED_NOGPS AVOID_ADSB THROW BRAKE POSHOLD AUTOTUNE FLIP SPORT DRIFT OF_LOITER LAND POSITION CIRCLE RTL LOITER GUIDED AUTO ALT_HOLD ACRO STABILIZE

  PX4 mode list: Known modes are: AUTO.PRECLAND AUTO.FOLLOW_TARGET AUTO.RTGS AUTO.LAND AUTO.RTL AUTO.MISSION RATTITUDE AUTO.LOITER STABILIZED AUTO.TAKEOFF OFFBOARD POSCTL ALTCTL AUTO.READY ACRO MANUAL

- Arming 
  service call /mavros/cmd/arming
  
- Setting throttle
  topic pub setpoint_attitude/attitude
 
## mavros param
- Circuit Breaker
  - CBRK_SUPPLY_CHK
  - CBRK_USB_CHK, disable USB and supply voltage check
 - pmw_min, stop mot arming spin

## TODO
1. OFFBOARD timeout 0.5s, need to constantly send mavlink command, command is queueing up serial line.
2. Somehow exiting offboard mode, to alt ctrl, altitude not reached? need actual flying drone?

## Changes made to note
1. timesynce
2. launch file baudrate, port
