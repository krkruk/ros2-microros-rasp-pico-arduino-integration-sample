Huge thanks to the author of https://gist.github.com/Redstone-RM/0ca459c32ec5ead8700284ff56a136f7 .

This text presents a quick and dirty way to boot up an application with Micro ROS on Raspberry Pico Board using Arduino as its main framework.

1. Compile your app in VSCode. You need to configure PlatformIO and all corresponding uDev rules https://docs.platformio.org/en/latest/core/installation/udev-rules.html

2. Assuming you managed to upload your firmware in VSCode, it's time to use ROS2. To install ROS2, simply follow the original instructions. Then:

Run this command to list all serial devices by its ID. It's better to use a simlink rather than a full physical address:
`ls /dev/serial/by-id/`

Then launch micro-ros Agent:

`docker run -it --rm -v /dev:/dev -v /dev/shm:/dev/shm --privileged --net=host microros/micro-ros-agent:iron serial --dev /dev/serial/by-id/usb-Arduino_RaspberryPi_Pico_601861E62A67324B-if00 -v6`


Then reset the microcontroller.
Once the microcontroller boots up, you can trigger your commands

`ros2 topic pub /ros_chassis_driver_topic geometry_msgs/msg/Twist "{linear: {x: 255.0, y: 0.0, z: 0.0}, angular: {x: 0.0, y: 0.0, z: 1.0}}"`

`ros2 topic pub --once /ros_chassis_driver_topic geometry_msgs/msg/Twist "{linear: {x: 0.0, y: 0.0, z: 0.0}, angular: {x: 0.0, y: 0.0, z: 1.0}}"`

The LED should change its intensity (max and then off).

