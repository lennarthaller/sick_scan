# sick_scan
##### Master-Branch
## Table of contents

- [Supported Hardware](#supported-hardware)
- [Start node](#start-node)
- [Bugs and feature requests](#bugs-and-feature-requests)
- [Creators](#creators)

This stack provides a ROS driver for the SICK series of laser scanners mentioned in the following list.
The SICK MRS6124 is a multi-layer, multi-echo 3D laser scanner that is geared
towards rough outdoor environments. 

## Supported Hardware

This driver should work with all of the following products. However, this driver is brand new (Feb 2018) 
The migration with the master-branch of the already published sick_scan-driver (see github.com/SICKAG/sick_scan ) is in preparation
and will be finished until end of March 2018. Since 22nd of February 2018 the new driver is a devel-branch of
the already published driver.

ROS Device Driver for Sick Laserscanner - supported scanner types: 


| **device name**    |  **part no.**                                                                                                                | **description**                                | **tested?**     |
|--------------------|------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------|:---------------:|
| MRS6124            | [6065086](https://www.sick.com/de/de/mess-und-detektionsloesungen/3d-lidar-sensoren/mrs6000/mrs6124r-131001/p/p533545)                                                                                                                     | 24 layer (standard)                            | ✔ [experimental]|
| MRS1104            | [1081208](https://www.sick.com/sg/en/detection-and-ranging-solutions/3d-lidar-sensors/mrs1000/mrs1104c-111011/p/p495044) | 4 layer                                        | ✔ [experimental]|
| LMS1104            | [unpublished] | 1 layer (based on MRS1104 technology) |  ✔ [experimental]|
| TiM551             | [1060445](https://www.sick.com/media/docs/9/29/229/Operating_instructions_TiM55x_TiM56x_TiM57x_de_IM0051229.PDF)           | 1 layer max. range: 10m, ang. resol. 1.00[deg] | ✔ [experimental]|
|                    |                                                                                                                              |  ang. resolution: 1.00[deg] Scan-Rate: 15 Hz   |                 |
| TiM561             | [1071419](https://www.sick.com/media/docs/9/29/229/Operating_instructions_TiM55x_TiM56x_TiM57x_de_IM0051229.PDF)           | 1 layer max. range: 10m, ang. resol. 0.33 [deg]| ✔ [experimental]|
|                    |                                                                                                                              |  ang. resolution: 0.33[deg] Scan-Rate: 15 Hz   |                 |
| TiM571             | [1079742](https://www.sick.com/media/docs/9/29/229/Operating_instructions_TiM55x_TiM56x_TiM57x_de_IM0051229.PDF)           | 1 layer max. range: 25m, ang. resol. 0.33 [deg]| ✔ [experimental]|
|                    |                                                                                                                              |  ang. resolution: 0.33[deg] Scan-Rate: 15 Hz   |                 |

##  Start Node

Use the following command to start ROS node:

For MRS6124:
roslaunch sick_scan sick_mrs_6xxx.launch

For MRS1104:
roslaunch sick_scan sick_mrs_1xxx.launch

For LMS1104:
roslaunch sick_scan sick_lms_1xxx.launch

For TiM551, TiM561, TiM571:
roslaunch sick_scan sick_tim_5xx.launch

## Bugs and feature requests

- General: Brand new driver especially for MRS6124 
- Stability issues: Driver is experimental 
- Binary mode / ASCII-Mode: - COLA-A/COLA-B-protocol:
The driver tests the protocol setting (ASCII or BINARY) at startup and then programs the scanner to the expected protocol. If the protocol is switched from BINARY to ASCII or vice versa during startup by the driver, it is recommended to restart the driver, otherwise the communication may be interrupted.
Cola protocol mapping:
-- MRS6124: COLA-B (Binary)
-- All other scanners: COLA-A (Ascii)
- Software should be further tested, documented and beautified
- Setting of "topic" should not be hardcoded to /cloud in the future. This allows the simultaneous operation of several scanners. Each point cloud can then be converted using its own TF transformation.

## Troubleshooting 

1. Check Scanner IP in the launch file. 
2. Check Ethernet connection to scanner with a ping. 
3. View node startup output wether the IP connection could be established 
4. Check the scanner status using the LEDs on the device. The LED codes are described in the above mentioned operation manuals.
5. Further testing and troubleshooting informations can found in the file test/readme_testplan.txt
6. If you stop the scanner in your debugging IDE or by other hard interruption (like Ctrl-C), you must wait until 60 sec. before
   the scanner is up and running again. During this time the MRS6124 reconnects twice. 
   If you do not wait this waiting time you could see one of the following messages:
   * TCP connection error
   * Error-Message 0x0d
7. Amplitude values in rviz: If you see only one color in rviz try the following:
   Set the min/max-Range of intensity display in the range [0...200] and switch on the intensity flag in the lauch file  
8. In case of network problems check your own ip address and the ip address of your laser scanner (by using SOPAS ET).
   * List of own IP-addresses: ifconfig|grep "inet addr"
   * Try to ping scanner ip address (used in launch file) 
9. If the driver stops during init phase please stop the driver with ctrl-c and restart (could be caused due to protocol ASCII/Binary cola-dialect).
   
## SUPPORT
 
* In case of technical support please open a new issue. For optimal support, add the following information to your request:
 1. Scanner model name,
 2. Ros node startup log,
 3. Sopas file of your scanner configuration.
  The instructions at http://sickusablog.com/create-and-download-a-sopas-file/ show how to create the Sopas file.
* In case of application support please use [https://supportportal.sick.com ](https://supportportal.sick.com).


## Installation

In the following instructions, replace `<rosdistro>` with the name of your ROS distro (e.g., `indigo`).

### From binaries

The driver has not been released yet. But once that happens, you can install it directly by typing:

~~`sudo apt-get install ros-<rosdistro>-sick_scan`~~

### From source

```bash
source /opt/ros/<rosdistro>/setup.bash
mkdir -p ~/ros_catkin_ws/src/
cd ~/ros_catkin_ws/src/
git clone -b devel --single-branch git://github.com/SICKAG/sick_scan.git
cd ..
catkin_make
source ~/ros_catkin_ws/install/setup.bash
```

## Quick Start

```bash
roslaunch sick_scan sick_mrs6xxx.launch
rosrun rviz rviz
publish to point cloud
```

## Creators

**Michael Lehning**

- <http://www.lehning.de>

on behalf of SICK AG 

- <http://www.sick.com>

------------------------------------------------------------------------

![SICK Logo](https://sick-syd.data.continum.net/static_2018013123/_ui/desktop/common/images/base/pics/logo.png "SICK Logo")
![Lehning Logo](http://www.lehning.de/style/banner.jpg "LEHNING Logo")


