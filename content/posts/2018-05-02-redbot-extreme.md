---
id: 96
title: 'Redbot Extreme'
date: '2018-05-02T01:38:43+00:00'
author: 'Nagarjun Redla'
layout: post
guid: 'https://impulsiverobotics.com/?p=96'
permalink: /2018/05/redbot-extreme/
image: /wp-content/uploads/2018/04/WhatsApp-Image-2018-04-21-at-9.28.01-PM-e1524447762671.jpeg
categories:
    - Builds
    - Working
---

After watching [Professor Magnus Egerstedt](https://magnus.ece.gatech.edu/) speak at my college about swarm robotics, I thought I’d take the course he taught on Coursera called [Control of Mobile Robots](https://www.coursera.org/learn/mobile-robot). It’s a very good course that was designed a few years ago and has an optional hardware track included. They made this part optional since it’s very difficult to keep up with ever changing DIY hardware and availability across the world. I wanted to try and follow the hardware part as well, so I went on a search for similar parts and got the newer Redbot with the Shadow Chassis. Although very sturdy, the robot didn’t have enough space or mounting options for Sharp IR sensors that were needed for the course. I made do with some double sided tape, but it was still a pain. This robot was much sturdier than the usual robots with mounting holes everywhere, but still lacked something to make it a better learning platform. So I thought I’d make a few enhancements.

### Parts

- 1x [Sparkfun Inventor’s Kit for RedBot](https://www.sparkfun.com/products/12649)
- 5x [Sharp GP2Y0A21YK0F IR Range Sensor](https://www.robotshop.com/en/sharp-gp2y0a21yk0f-ir-range-sensor.html)
- 5x [SIRC-01 Sharp GP2 IR Sensor Cable – 8″](https://www.robotshop.com/en/sirc-01-sharp-gp2-ir-sensor-cable-8.html)
- 1x [7.4v 5200mAh Lipo battery](https://www.amazon.com/gp/product/B00N9SU5U0/ref=oh_aui_search_detailpage?ie=UTF8&psc=1)
- 1x Raspberry Pi 3
- 1x [Raspberry Pi Camera Module v2](https://www.raspberrypi.org/products/camera-module-v2/)
- 1x [Pololu 5V, 5A Step-Down Voltage Regulator D24V50F5](https://www.pololu.com/product/2851)
- Hookup Wire
- 1x Deans Ultra Plug Male
- M3 screws and nuts
- Nylon Screws and nuts for Raspberry Pi camera
- Nylon Standoffs

### 3D Printed parts

- 5x [Sparkfun Shadow Chassis Side Strut &amp; Mounts](https://www.thingiverse.com/thing:1196071)
- 1x [Sparkfun Shadow Chassis Front Strut Raspberry Pi Camera Mount](https://www.thingiverse.com/thing:2853753)

### Assembly

#### Battery

Remove the battery pack included in the kit. Then put the battery in the center, making sure it doesn’t push your hall effect sensors away from the magnets on the motor shafts. This is a very powerful battery so be very careful with it(I learnt it the hard way by accidentally shorting wires, I’m surprised I didn’t burn the house down). Take the included velcro strap and instead of wrapping it around the battery, cut off the ends and stick one side to the bot and the other side to the battery. This should make sure the battery doesn’t hit the encoders.

Disclaimer: You will lose the capability of stuffing a servo in the servo slot. Sparkfun mentioned building a robot arm on this thing, but I won’t be doing that ever on this robot so it’s fine I guess.

Don’t attach the top stage yet.

![](https://i0.wp.com/impulsiverobotics.com/wp-content/uploads/2018/05/IMG_20180502_124232.jpg?resize=1024%2C768&ssl=1)

#### Fasten Raspberry Pi Camera and sensors

Attach the Raspberry Pi camera on to the 3D printed front strut. You won’t get to access this easily once assembled so make sure your connections are good.

Now would be a good time to screw the sensors onto the 3D printed side struts as well. Also wire the proximity sensors and push them through the hole under the RedBot board, so you don’t have to struggle to push them through tight spaces after the battery is in. The wires should be long enough.

![](https://i0.wp.com/impulsiverobotics.com/wp-content/uploads/2018/05/IMG_20180502_124611.jpg?resize=1024%2C768&ssl=1)

#### Attach the struts

I found an existing design on Thingiverse for struts with holes for a ToF sensor (which was in turn supposed to be a drop-in replacement for the Sharp IR sensor) so I got those printed and decided to modify the part for a Raspberry Pi camera.

Pretty straightforward, attach the struts to the bottom base first. Begin with the camera strut in the front center, and then add the four sensors on the corners and, if you want, you can add a fifth proximity sensor in the back (if you don’t mind charging the battery when it is in the chassis, since the sensor at the back blocks the battery).

![without the top on](https://i0.wp.com/impulsiverobotics.com/wp-content/uploads/2018/05/IMG_20180502_124412.jpg?resize=1024%2C813&ssl=1)

If you want to go beyond this step, I don’t think you should attach the top portion yet. Otherwise, you can attach it right now, making sure the LiPo battery terminals stick out the back end.

#### Make Mounting holes and connect Raspberry Pi

Pretty straightforward here too. Place your pi on the top part of the chassis, mark the holes and drill, I used a Dremel. Also make mounting holes on the side for the regulator. You can technically drill a hole after assembling the top part, but there’s a very high chance of puncturing the battery and causing an explosion. Don’t be stupid. Don’t do that. Please. I know it’s tempting.

Screw on some Nylon spacers and then the Raspberry Pi on top of that (I used spacers long enough to stick a Neural Compute Stick in the side without hitting the wheels, but that’s pointless, as you’ll soon realize).

![closeup](https://i0.wp.com/impulsiverobotics.com/wp-content/uploads/2018/05/IMG_20180502_124640.jpg?resize=1024%2C736&ssl=1)

#### Check connections, snap together, drive around

Check if you connected everything and then attach the top part of the RedBot. I used the Raspberry Pi 3 B+ not to look fancy but because many complex-er libraries aren’t compatible with it yet, but the [RPi-Cam-Web-Interface](https://elinux.org/RPi-Cam-Web-Interface) works, and it’s fun when you drive around with it. I had the ZigBee module on the Redbot and a controller from earlier, so that’s how it’s being driven around. I don’t think it’d be straightforward to have ZigBee comms coexist with serial communications with the Raspberry Pi, so don’t try adding a ZigBee module if you have no use for it.

![POV cam gif](https://i0.wp.com/impulsiverobotics.com/wp-content/uploads/2018/05/ezgif-1-b9451edde9.gif?resize=600%2C338&ssl=1 "A gif of a video of a video.")

(On a side note, the text on the top looks familiar from a [TechCrunch Disrupt video](https://youtu.be/sUfqmjM9_VE?t=4m).. and this web interface only works with the Raspberry Pi Camera Module. hmm..)

### Conclusions

If I had such sturdy proximity sensors when taking the Coursera course, it would’ve helped a lot. The simple single phase hall effect sensor based encoders kept me from overhauling the robot thinking it would be pointless. Quadrature encoders would’ve been sweet. After adding the camera, I realized it was pointless trying to extract a lot of info from a POV that was so low, so point in adding a Neural Compute stick or trying to piggyback a Google Vision kit on the RedBot: it’s just way too low for object recognition (or to just fit an entire object in its frame)

Want to take it further? Only sensible thing without going over the top would be to connect via I2C or USB. I was talking to a friend about this robot and he suggested we implement a machine learning algorithm on the robot. Since the algorithm’s output only needs to be throttle values for the two motors, we don’t really need the encoders. I was interested and saw a [similar project video on Youtube](https://www.youtube.com/watch?v=WtEYMELvRHI) for obstacle avoidance. Both of us have finals soon, so more on that after.