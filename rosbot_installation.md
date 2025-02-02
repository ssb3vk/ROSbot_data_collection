# ROSbot Setup

This document contains instructions to set up the data collection scripts on your ROSbot so you can run these scripts natively and collect a dataset.

Hardware required:
* ROSbot
* Wireless Xbox controller
* small form-factor external USB drive (like [this one](https://a.co/d/4fmXYWw))
* Bluetooth 4.0 dongle (like [this one](https://a.co/d/e7X3SpB))


## Setup Instructions

N.B. READ THE INSTRUCTIONS ALL THE WAY THROUGH AND SKIM THE TROUBLESHOOTING SECTION BEFORE YOU BEGIN. 
This will make setup a lot faster as you can make sure you have everything you need ready to go (keyboard, mouse, bluetooth dongle, wifi connection...).

1. Plug the USB drive into a computer and format the USB drive as Fat32. Reformat the drive if needed. Be sure to retain a copy of the data on your local computer if reformatting is required. 
2. Copy `start_rosbot.sh` file and `src` directory to your USB drive. Plug the USB drive into your ROSbot.
3. Connect to your ROSbot (ssh, plug in a monitor and mouse/keyboard, etc.).
4. Copy `start_rosbot.sh` to `~` (aka `/home/husarion`). 
5. Give `start_rosbot.sh` executable permissions by running `chmod +x start_rosbot.sh`.
6. Copy the `datacoll` package inside `src` to `~/husarion_ws/src`.
7. Add executable permissions to all `.py` scripts in the `datacoll` package: `cd ~/husarion_ws/src/datacoll/src; chmod +x *.py`
8. `cd ~/husarion_ws; source devel/setup.bash`
9. Refer to the instructions [here](https://husarion.com/tutorials/howtostart/rosbot---quick-start/#connecting-rosbot-to-your-wi-fi-network) to connect your ROSbot to wifi or ethernet. 
10. Install the `joy_node` package: `sudo apt install ros-<distro>-joy`. Refer to troubleshooting for determining your distro and updating your ros repo and authentication.
11. Install `bluez` and `bluetoothctl`: ` sudo apt install bluez`
12. Run `sudo service bluetooth start; bluetoothctl scan on`. Try connecting your Xbox controller to the bluetooth. Refer to troubleshooting if your bluetooth is disconnecting and reconnecting.
13. Try running the startup script: `./start_rosbot.sh`. If you experience errors, refer to troubleshooting.
14. If steps #11-12 run smoothly, add `./start_rosbot.sh` to your startup routine. For more detailed screenshots, see section below on adding scripts to your startup routine.
    1. Go to Dash and type "Start" into the search bar. On HusarionOS, this is the magnifying glass icon at the bottom of the screen.
    2. Double click to open "Session and Startup".
    3. Hit the "Add" icon to add a new routine to startup.
    4. Write a name and description. In the "Command" field, type `/bin/bash -c "sleep 10 & /home/husarion/start_rosbot.sh"`.
15. Test if your script starts on startup. Turn off your ROSbot and turn it back on again. The LiDAR turret should spin within 20-30 seconds. Let it run for a few seconds and then check the external USB drive to see if the dataset wrote to disk.
16. Follow the instructions linked [here](https://support.xbox.com/en-US/help/hardware-network/controller/update-xbox-wireless-controller) to update the Xbox controller firmware.
17. If step #10 goes smoothly, refer to [README.md](README.md) "ROSbot Setup" section for running the ROSbot. If you experience errors, refer to troubleshooting.

## Adding scripts to your startup routine on HusarionOS
1. Go to Dash and type "Start" into the search bar. On HusarionOS, this is the magnifying glass icon at the bottom of the screen.
![husarion OS home screen](figures/husarionOS-homescreen.png)
2. Double click to open "Session and Startup".
![](figures/session-and-startup.png)
3. Hit the "Add" icon to add a new routine to startup.
![](figures/session-and-startup-add.png)
4. Write a name and description. In the "Command" field, type `/bin/bash -c "sleep 10 & /home/husarion/start_rosbot.sh"`.
![](figures/session-and-startup-add-complete.png)

## Troubleshooting

### Installing Packages
The apt repository preinstalled on the ROSbot may be stale and may require an update. 
Check your ros distro by running `rosverion -d` or `ls /opt/ros`.
If you cannot install new ros packages using `apt`, [update your apt repository](http://wiki.ros.org/melodic/Installation/Ubuntu) if necessary.

To install new packages from ROS and Husarion, you will need to update two GPG keys in order to access those apt repositories.
If your ROSbot was manufactured before June 2022, then ROS has updated their GPG key since your ROSbot's software was installed.
- To update your ROS apt GPG key: (link)[https://community.husarion.com/t/very-important-update-for-gpg-keys-in-ros-repositories/660]
- To update your Husarion apt GPG key: (link)[https://community.husarion.com/t/husarion-repository-expired-key/1240/6]
- "husarnet signature needs updating" error from husarion package repo: (link)[https://community.husarion.com/t/apt-update-invalid-signatures-fresh-system-reinstall/1054]

### WiFi and Bluetooth
If you are using a wireless network and intend to operate the ROSbot outside that network, test your ROSbot startup routine when not connected to that network. 
You may need to disable the WiFi on your ROSbot to successfully complete the startup routine. 
You may also want to change the `ROS_IP` set in `start_rosbot.sh`.

Fix for controller bluetooth disconnect-reconnect: [Bluetooth Problem Ubuntu 18.04 LTS](https://askubuntu.com/questions/1040497/bluetooth-problem-ubuntu-18-04-lts)

### Bluetooth controller connection fix

1. Ensure bluetooth service is started "sudo service bluetooth start"
2. start bluetoothctl via "bluetoothctl" in the command line
3. systemctl restart bluetooth
4. forget [Xbox Wireless Controller] (via the Xbox Controller's mac address)
5. remove [Xbox Wireless Controller]
6. set Xbox controller to pairable mode
7. set pairable on
8. start scan via "scan on"
9. connect [Xbox Wireless Controller]


### Husarion OS (Ubuntu 18.04 LSB OS)
Here's a [reference](https://net2.com/how-to-run-applications-at-startup-on-ubuntu-18-04/) for adding scripts to an Ubuntu 18 LSB system.

Extra docs from Ubuntu to install `bluez`: [link](https://ubuntu.com/core/docs/bluez/install-configure/install)


### Camera calibration
Basics of ROS camera calibration: [ros wiki](http://wiki.ros.org/camera_calibration)


### Hardware diagrams and teardown
Husarion guide: [link](https://husarion.com/manuals/rosbot/)

Hackaday guide: [link](https://cdn.hackaday.io/files/21885936327840/ROSbot_assembly_instruction.pdf)

RK-370CA-22170 motor datasheet (DC 6.0V 181129): [link](https://datasheetspdf.com/pdf/1017717/MABUCHI/RK-370CA/1)


### Getting Python files written on Windows Machines to run in linux

Probably going to need to chmod +x the file. 

Also use: sed -i 's/\r//' fileWindows.txt
To convert between DOS (Windows) and CRLF (Linux) line endings