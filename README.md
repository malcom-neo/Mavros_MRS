# MRS Rox My Sox

## Introduction
This documents the efforts of the SUTD [Multi-Rotor Society(MRS)](https://github.com/multirotorsociety) to get MAVROS to work with drones in a variety of use cases including but not limited to:
- GPS-enabled with `OFFBOARD` control using Global Pose
- GPS-denied with `OFFBOARD` contol using Local Pose
The test hardware is a PixHawk Cube running PX4.
More detailed instructions will be outlined as progress is made.

## Installing mavros

1. Debian installation:
```
sudo apt-get install ros-melodic-mavros
sudo apt-get install ros-melodic-mavros-extras
```
Debian installation is the most convenient though it is also possible to build from source: instructions [here](https://dev.px4.io/v1.9.0/en/ros/mavros_installation.html#source-installation).

2. GeographicLib installation:
```
sudo apt-get install geographiclib-*
```

3. Source ros and install dataset
- Go to [mavros github](https://github.com/mavlink/mavros) and download it
- Install the geographiclib dataset
```
./install_geographiclib_datasets.sh
```
4. Launch MAVROS according to Flight controller (APM or PX4). 
```
roslaunch mavros apm.launch # APM launch
roslaunch mavros px4.launch # PX4 launch
```

## Commands
- Changing flight mode
  Do a Service call `/mavros/set_mode` with mode from apm in custom mode
  
  APM mode list: `GUIDED_NOGPS AVOID_ADSB THROW BRAKE POSHOLD AUTOTUNE FLIP SPORT DRIFT OF_LOITER LAND POSITION CIRCLE RTL LOITER GUIDED AUTO ALT_HOLD ACRO STABILIZE`

  PX4 mode list: Known modes are: `AUTO.PRECLAND AUTO.FOLLOW_TARGET AUTO.RTGS AUTO.LAND AUTO.RTL AUTO.MISSION RATTITUDE AUTO.LOITER STABILIZED AUTO.TAKEOFF OFFBOARD POSCTL ALTCTL AUTO.READY ACRO MANUAL`
  The efforts outlined within this document utilize PX4 and thus adopt the PX4 mode list.
  
- Arming 
  Do a service call to `/mavros/cmd/arming`
  
- Setting throttle
  Publish on the rostopic: `setpoint_attitude/attitude`
 
## MAVROS Parameters
- Circuit Breaker
  - `CBRK_SUPPLY_CHK`
  - `CBRK_USB_CHK`, disable USB and supply voltage check
 - `PWM_MIN`, stop mot arming spin
 These parameters should also be able to be modified through QGroundControl
 
## Using QGroundControl
QGroundControl is recommended as a Ground Control software due to heartbeat issues when other GCS is used such as Mission Planner or APMplanner. However, due to serial port loading, the FTDI USB breakout from the flight controller should be connected to the development computer while the Drone should interface with a seperate GCS computer running QGroundControl over USB serial or RF.
 
## Change log
 
### 16/9

1. Changed param `MAV_USEHILGPS` to 1
2. Published `global_position/global` msg
3. Able to go pub `setpoint_position/local` and bypass position error.
4. Unable to stay in offboard mode
---

### 16/9

1. Pre-flight
  - Calibrated the Compass to eliminate any Mag Inconsistency issues
  - Reset `EKF2_AID_GPS` to 1
  - Reset `CBRK_GPSFAIL` to 0
2. For the most part, `global_position/raw/fix` outputs raw data but `global_position/global` frequently fails to reflect that the Drone has received a GPS lock (indicator lights remain blue)
3. No home output published on the rostopic`/mavros/home_position/home`. Unable to set home through mavcmd service or topic
4. Somehow or rather, the Drone managed to get a GPS Fix (success was not replicable after a power cycle). Some possible things that fixed it could have been:
  - Set flight mode to `AUTO.TAKEOFF` - might have got the drone to set auto home
  - Set flight mode to `STABILIZE` - on second try, this got the drone to blink green for a while before it reverted to failsafe mode
5. Unable to publish to `setpoint_position/global` and change to offboard. 
6. What worked after getting a GPS fix:
  - Publish on the `mavros/setpoint_position/local` topic at a rate of 100 with only the `z` component set to 2
  - Set the flight mode to `OFFBOARD`
  - Drone motors will spin up


## TODO
1. OFFBOARD timeout 0.5s, need to constantly send mavlink command, command is queueing up serial line.
2. Somehow exiting offboard mode, to alt ctrl, altitude not reached? need actual flying drone?

## Changes made to note
1. timesynce
2. launch file baudrate, port
