---
id: 82
title: 'ROS enabled omnidirectional robot'
date: '2018-04-16T07:47:36+00:00'
author: 'Nagarjun Redla'
layout: post
guid: 'https://impulsiverobotics.com/?p=82'
permalink: /2018/04/ros-enabled-omnidirectional-robot/
image: /wp-content/uploads/2018/04/IMG_20180502_131631.jpg
categories:
    - Builds
    - Working
tags:
    - '3D camera'
    - SLAM
---

This was a project that incrementally grew to become one of the more complex robots I’ve ever built. I was interested in checking out how [omni wheels](https://en.wikipedia.org/wiki/Omni_wheel) work after I saw a video of a robot made by a high school friend’s startup that paints walls. I had no idea on how to control a robot with omni wheels. A quick google search later I realized they looked real cool and I decided to build one.

The wheels individually cost a lot. I found a really [good design on thingiverse](https://www.thingiverse.com/thing:1276446), but after realizing that working with pololu micro metal gearmotors with encoders in tiny spaces can be a pain, I thought I’d look around more. I finally came across a [chassis with motor kit](https://www.robotshop.com/en/3wd-48mm-omni-directional-triangle-mobile-robot-chassis.html) at Robotshop which looked cool and wasn’t that expensive either. I remember finding listings of the same chassis with an Arduino on AliExpress, but can’t find it anymore.

A lot of websites online mentioned omni wheels don’t perform that well on uneven surfaces and can’t carry a lot of weight. I was surprised to learn how heavy the robot was after it arrived. Seeing how big it was, I started overplanning as usual. I’ve never worked on the individual components used in this project before (other than the camera), and neither have I built a physical ROS robot from the scratch. I’m confident about it, and hopefully it should work.

### Parts

- 1x [Adafruit HUZZAH32](https://www.adafruit.com/product/3405)
- 1x [3WD 48mm Omni-Directional Triangle Mobile Robot Chassis](https://www.robotshop.com/en/3wd-48mm-omni-directional-triangle-mobile-robot-chassis.html)
- 2x L298N Motor Driver
- 1x [11.1v LiPo battery pack](https://www.amazon.com/gp/product/B071DJKXRD/ref=oh_aui_search_detailpage?ie=UTF8&psc=1)
- 1x [Aaeon UP Core](http://www.up-board.org/upcore/)
- 1x [Intel Realsense D435 3D Camera](https://click.intel.com/intelr-realsensetm-depth-camera-d435.html)

### Build

#### Power and Motor Drivers

Since it had two sections and the motors ran on 12V, I wanted to dedicate the first level for the motor drivers and a good battery, and I didn’t want to put a step-up to power motors. I ordered a few 11.1V LiPo batteries and decided to use two extra L298N motor drivers I had from a hydroponics controller project. The motor drivers and the battery fit perfectly in the lower level.

![](https://i0.wp.com/impulsiverobotics.com/wp-content/uploads/2018/04/IMG_20180502_131007-e1525301525382-1024x768.jpg?resize=1024%2C768&ssl=1)

#### Microcontroller Circuit

Looking at all the room I had on the top, I wanted to keep this as wire-clean as possible. The three motors needed 6 PWM capable pins (2 each) to drive, and 6 (preferably) interrupt capable pins for encoders. This rules out almost all AVR Arduino boards with a small footprint. This also rules out being able to add enough sensors to the microcontroller. For awesome compatibility, as usual, I decided to go for an Adafruit Feather board. I didn’t want a feather with an extra chip on-board for wireless (WiFi or BLE), I wanted to try out the new ESP32 instead. What’s more interesting is that the chip also supports rosserial, making it swappable with other Huzzah boards. ESP32 guzzles too much power? For what I had in mind for this project, it doesn’t really amount to that much. Plus, if you want to make the robot without the UP Core, the onboard 5v regulators on the L298N drivers should be more than enough to power the microcontroller.

The ARM M0+, Teensy and ESP32 feather boards have all interrupt capable pins.

~~*TODO : Solder I2C Female Header Pins for IMU*~~ Not required, yet.

~~*TODO : [Solder diode to protect USB from external power](https://forums.adafruit.com/viewtopic.php?f=24&t=112430&p=562071&hilit=diode+usb+power)*~~ – Done

*TODO : Add Step Down Regulator to power UP Core*

#### Computer Mount

After I thought of the possibility of using the realsense camera, I began thinking of how to mount the UP Board AND the microcontroller on the same plane (along with the camera). More than the space, I was worried about the connection to the camera. The UP Board has a USB 3.0 OTG port, which needs an adapter before connecting to the USB cable to the camera. I had troubles with the R200 before, and I didn’t want to take any chances. While I was thinking about this, I found that the UP core had released. While I was contemplating spending on the UP Core, the UP Board team decided to put a sticky banner on their homepage with a countdown announcing AI on the edge. I was very excited about it and decided to wait it out instead of buying the UP Core. After the countdown, I realized it was just a pre-order launch of a PCIe card for the Movidius 2 chip and an announcement of a really cool UP Core Plus board (with actual photos of expansion boards) which I can’t buy right now anyway.

Why the UP Core? Many reasons really.

- Integrated WiFi + BLE: Really necessary, I usually forget to account for the space USB devices take up.
- Full Sized USB 3.0 Port: Much more convenient to connect the camera instead of using the adapter on the UP Board (You could use the mini USB 3.0 to mini USB 3.0 cable, but I couldn’t find one anywhere outside the UP Board Shop.)
- Same Power, Smaller Size: Post-It sized x86 Single Board Computer!? Count me in!
- Fanless Heatsink: I couldn’t mount the UP Board with the case and an exposed fanned heatsink would mean dust accumulation, a fanless system is much better.
- [USB 2.0 expansion cable](https://up-shop.org/up-peripherals/181-usb-20-pin-header-cable-wo-uart.html): Conveniently break out USB devices away from the board and tuck the wires inside the robot. Still gets a USB 2.0, but I can accommodate a Movidius Neural Compute Stick if required (Without the huge thing sticking out of the robot)

I decided to hoist the UP Core above the microcontroller circuit with my favorite nylon standoffs. The WiFi antenna might be optional, but I really wanted to use it. Antennae are important, people. I was curious enough to buy the Aluminum case for the UP Core and kowing how it fit in the case that I popped open the stock heatsink and THEN realized I couldn’t put it back. At this point I didn’t really have a choice: I had to use the case. The mounting holes are in the same place so that shouldn’t affect anything if you want to mount the UP Core as is. The Aluminum case is pretty heavy, so I decided to go for a smaller PCB for the microcontroller stage to keep it from buckling under the weight of the case.

#### Camera Mount

I decided to 3D print the mount so I don’t accidentally damage the camera by trying out weird brackets lying around. I came up with a [simple design](https://www.thingiverse.com/thing:2864087) to mount it on the corner of the robot. The three holes on the robot are parts of the longer slit on the chassis rather than screw holes, it just needs to be securely tightly to avoid slipping. I removed material between screws on the camera to show off the Realsense logo (Represent).

At this point I decided to check if the circuit was working properly, and realized two of the three motor encoders weren’t working. One of them had a missing capacitor that broke the circuit, and I can’t solder such small SMT components. I emailed RobotShop and they contacted the manufacturer to send me replacement parts. We don’t need the encoders for a while till the entire build is complete, so let’s work on setting up the software instead.

![](https://i0.wp.com/impulsiverobotics.com/wp-content/uploads/2018/04/IMG_20180502_131530.jpg?resize=768%2C1024&ssl=1)

### Setting up the software

#### Setting up ORB-SLAM2

ORB-SLAM2 was one of the first SLAM libraries that I heard of. I decided to go with this. The software setup is pretty straightforward. Follow the instructions on the [Github page](https://github.com/raulmur/ORB_SLAM2). Setup the prerequisites as mentioned in the page carefully making sure all of them are installed properly. Before running the final `./build.sh` script, we need to make a very tiny change. The UP Core might not [run out of memory during the build](https://github.com/raulmur/ORB_SLAM2/issues/242). Mine kept getting stuck at 59% before it said virtual memory ran out. Open the `build.sh` file and changed the last line from

```shell
make -j
```

to

```shell
make -j1
```

you could try sensible higher numbers, I haven’t tried it though. I took help from a [medium article](https://medium.com/@j.zijlmans/orb-slam-2052515bd84c) and tried working on the TU-Munich dataset. Everything worked fine but the viewer kept freezing at the end. There were a few open issues mentioning that <del> and I don’t think it’s something that affects the core software I thought I’d go ahead with ROS</del> [and turns out you need to install an older version of Pangolin](https://github.com/raulmur/ORB_SLAM2/issues/547).

![ORB-SLAM2 gif](https://i0.wp.com/impulsiverobotics.com/wp-content/uploads/2018/04/ezgif-4-be7aa06151.gif?resize=600%2C338&ssl=1 "ORB-SLAM2 working on the TUM1 dataset. Another gif, sorry.")
#### Getting Camera Parameters

So the Intel D400 cameras come with factory loaded parameters, and they’re published on a topic in the realsense ROS node. We can get those parameters by running `rostopic echo /camera/color/camera_info`, This is what the output looks like

```yaml
header: 
  seq: 47
  stamp: 
    secs: 1529978641
    nsecs: 358682339
  frame_id: "camera_color_optical_frame"
height: 480
width: 640
distortion_model: "plumb_bob"
D: [0.0, 0.0, 0.0, 0.0, 0.0]
K: [616.7030029296875, 0.0, 300.7779235839844, 0.0, 616.4176025390625, 234.95497131347656, 0.0, 0.0, 1.0]
R: [1.0, 0.0, 0.0, 0.0, 1.0, 0.0, 0.0, 0.0, 1.0]
P: [616.7030029296875, 0.0, 300.7779235839844, 0.0, 0.0, 616.4176025390625, 234.95497131347656, 0.0, 0.0, 0.0, 1.0, 0.0]
binning_x: 0
binning_y: 0
roi: 
  x_offset: 0
  y_offset: 0
  height: 0
  width: 0
  do_rectify: False
---
```

The parameters published in the camera ROS topic gives us the values for fx, fy, cx, cy from K, and k1, k2, p1, p2, k3 from D ([more info](https://github.com/IntelRealSense/librealsense/blob/165ae36b350ca950e4180dd6ca03ca6347bc6367/third-party/realsense-file/rosbag/msgs/sensor_msgs/CameraInfo.h#L268)). From the [datasheet](https://www.intel.com/content/dam/support/us/en/documents/emerging-technologies/intel-realsense-technology/Intel-RealSense-D400-Series-Datasheet.pdf), we get the baseline to be 55mm for D415 and 50mm for D435. The value for bf is `bf = baseline (in meters) * fx`. This is the YAML file I used for D415

<script src="https://gist.github.com/nagarjunredla/8e5e33db93d06c1bd0ebdcd57e093294.js"></script>

#### Setting up ROS node

Since the realsense node publishes camera data on `/camera/color/image_raw` and depth data on `/camera/depth/image_rect_raw` we need to change it to these links in `~/ORB_SLAM2/Examples/ROS/ORB_SLAM2/src/ros_rgbd.cc`. After that make sure you have ROS\_PACKAGE\_PATH pointed to the right directory of ORB\_SLAM2 and run `./build_ros.sh`

#### Running the nodes

In one terminal, start `roscore`. In another terminal, source the environment variables of ORB SLAM2

```shell
source ~/ORB_SLAM2/Examples/ROS/ORB_SLAM2/build/devel/setup.bash
```

and then run the node with

```shell
rosrun ORB_SLAM2 RGBD PATH_TO_VOCABULARY PATH_TO_SETTINGS_FILE
```

Once the ORB SLAM node is running, open a third terminal window and run the Intel Realsense node. The output should look like this

<span class="embed-youtube" style="text-align:center; display: block;"><iframe allowfullscreen="true" class="youtube-player" height="360" loading="lazy" sandbox="allow-scripts allow-same-origin allow-popups allow-presentation" src="https://www.youtube.com/embed/yl6MgnYNDE8?version=3&rel=1&showsearch=0&showinfo=1&iv_load_policy=1&fs=1&hl=en-US&autohide=2&wmode=transparent" style="border:0;" width="640"></iframe></span>

As you can see, ORB SLAM initializes immediately and rarely loses tracking. I tried using Monocular version with footage from my Ryze Tello, but it took a while to initialize and kept losing track. The RGB-D version looks much more reliable. I was surprised to see that the UP Core handled it well.

I wanted to evaluate ORB-SLAM2 with an RGBD camera only because my monocular tests were failing because of calibration problems or poor image quality. To be fair I tried it on the Ryze (DJI?) Tello. I got some buggy results:

![Tello SLAM gif](https://i0.wp.com/impulsiverobotics.com/wp-content/uploads/2018/04/tello_slam_gif.gif?resize=600%2C290&ssl=1 "kept losing track often")
Next step would be to try RTAB-Map. It’s catkinized and has a better dev community.

Since the robot is built and and it’s compute capability is validated, next steps are to write Arduino code for the HUZZAH32 board. The kit came with faulty wheel encoders and even though Robotshop sent me replacements for two motors, one of them was still faulty. So now I have 5 motors with 2 functioning wheel encoders.