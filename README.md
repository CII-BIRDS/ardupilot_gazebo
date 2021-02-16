
# BIRDS V2 Virtual Environment

## System
* Ubuntu 20.04

---


## Overview
This project uses many different packages to be successful:
* ROS Noetic
* Gazebo
* PX4
* Ardupilot
* MAVProxy
* DroneKit
<br/>

---
## ROS Installation and Catkin Setup
1. ROS
    * Add permissions to access serial port and remove modemmanager
        ```bash
        sudo usermod -a -G dialout $USER
        sudo apt-get remove modemmanager -y
        ```
    * Download [ROS Noetic](http://wiki.ros.org/noetic/Installation/Ubuntu).

        * Use Desktop-Full Install

2. Catkin
    * Install
        ```bash
        sudo apt-get install python3-wstool python3-rosinstall-generator python3-catkin-lint python3-pip python3-catkin-tools
        pip3 install osrf-pycommon
        ```
    * Initialize
        ```bash
        mkdir -p ~/catkin_ws/src
        cd ~/catkin_ws
        catkin init
        ```
        <br/>

---
## Gazebo Installation

* Preconditions

    ```bash
    sudo sh -c 'echo "deb http://packages.osrfoundation.org/gazebo/ubuntu-stable `lsb_release -cs` main" > /etc/apt/sources.list.d/gazebo-stable.list'
    wget http://packages.osrfoundation.org/gazebo.key -O - | sudo apt-key add -
    sudo apt update
    ```

* Use the one-liner installation to easily install Gazebo.
    1. Install
    ```bash
    curl -sSL http://get.gazebosim.org | sh
    ```
    2. Test
    ```bash
    gazebo --verbose
    ```
    This should open an empty world. If it does, close the world and continue.
<br/>

---
## PX4 Firmware
Install PX4 Firware
```bash
    sudo apt install git
    mkdir px4
    cd px4
    git clone https://github.com/PX4/Firmware.git
```
<br/>

---
## Making Drone
Before building the drone, add GSTREAMER packages
```bash
    sudo apt install libgstreamer1.0-dev
    sudo apt install gstreamer1.0-plugins-good
    sudo apt install gstreamer1.0-plugins-bad
    sudo apt install gstreamer1.0-plugins-ugly
```
Use the following code to build the SITL Drone.
```bash
    cd ~/px4/Firmware
    make px4_sitl gazebo_solo
```

If you fail making the drone, the output should tell you packages that you are missing. Install packages using 'pip3', and try again as below:

```bash
    make clean
    make px4_sitl gazebo_solo
```
<br/>

---
## ArduPilot Installation

* Installation
    ```bash
    git clone https://github.com/ArduPilot/ardupilot.git
    cd ardupilot
    git submodule update --init --recursive
    ```
* Dependencies and Reload Profile
    ```bash
    cd ardupilot
    Tools/envrionment_install/install-prereqs-ubuntu.sh -y
    . ~/.profile
    ```
* Checkout Latest Copter Build
    ```bash
    git checkout Copter-4.0.4
    git submodule update --init --recursive
    ```
* Run SITL to set Parameters
    ```bash
    git checkout Copter-4.0.4
    git submodule update --init --recursive
    ```

* Install and Build ArduPilot Gazebo
    ```bash
    git clone https://github.com/donnybadamo/ardupilot_gazebo
    cd ardupilot_gazebo
    mkdir build
    cd build
    cmake ..
    make -j4
    sudo make install
    ```

* Set Path 

    ```bash
    echo 'source /usr/share/gazebo/setup.sh' >> ~/.bashrc
    echo 'export GAZEBO_MODEL_PATH=~/ardupilot_gazebo/models' >> ~/.bashrc
    echo 'export GAZEBO_RESOURCE_PATH=~/ardupilot_gazebo/worlds:${GAZEBO_RESOURCE_PATH}' >> ~/.bashrc
    source ~/.bashrc
    ```
    <br/>
----
## MAVProxy Installation

* Use the below code to install MAVProxy 
    ```bash
    sudo apt-get install python3-dev python3-opencv python3-wxgtk4.0 python3-pip python3-matplotlib python3-lxml python3-pygame
    pip3 install PyYAML mavproxy --user
    echo "export PATH=$PATH:$HOME/.local/bin" >> ~/.bashrc
    ```
* If issues are found, please go [here](https://ardupilot.org/mavproxy/docs/getting_started/download_and_installation.html#linux)

Set Path
    ```bash
    echo "export PATH=$PATH:$Home
    ```
    <br/>
---
## Testing Simulator
You will need two windows for this:

1. In window one, start Gazebo with the bridge Gazebo world
    ```bash
    gazebo --verbose ~/ardupilot_gazebo/worlds/bridge.world
    ```

2. In window two, start SITL with ArduCopter
    ```bash
    cd ~/ardupilot/ArduCopter
    sim_vehicle.py -v ArduCopter -f gazebo-iris --console
    ```
    <br/>
--- 

## DroneKit Installation

```bash
sudo -H pip install dronekit
git clone https://github.com/dronedojo/droneProgrammingCourse

```

## Test DroneKit

1. In window one, start Gazebo with the bridge Gazebo world
    ```bash
    gazebo --verbose ~/ardupilot_gazebo/worlds/bridge.world
    ```

2. In window two, start SITL with ArduCopter
    ```bash
    cd ~/ardupilot/ArduCopter
    sim_vehicle.py -v ArduCopter -f gazebo-iris --console
    ```
3. In window three, start the DroneKit auto mission script
    ```bash
    cd droneProgrammingCourse/dk/
    Python auto_mission.py --connect 127.0.0.1:14550
    ```
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>


---
## Sources
 * https://ardupilot.org/dev/docs/sitl-simulator-software-in-the-loop.html

 * https://github.com/Intelligent-Quads/iq_tutorials/blob/master/docs/Installing_Ardupilot_20_04.md

