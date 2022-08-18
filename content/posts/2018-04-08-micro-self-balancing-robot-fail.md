---
id: 23
title: 'Micro Self Balancing Robot'
date: '2018-04-08T23:15:05+00:00'
author: 'Nagarjun Redla'
layout: post
guid: 'http://impulsiverobotics.com/?p=23'
permalink: /2018/04/micro-self-balancing-robot-fail/
image: /wp-content/uploads/2018/04/IMG_20180408_180340.jpg
categories:
    - Builds
    - 'Not Working'
---

My interest in Control Systems grew in graduate school. Weirdly it was one of the subjects we wanted to run away from during my undergraduate days when we weren’t given the independence to choose our own subjects. At UIUC, I grew fond of the subject after attending a few lectures. The only problem was that there’s a chunk of Control Systems basics that isn’t covered in graduate school at all. Many Professors said I could skip the undergraduate class altogether, but it had a lot of hands on lab assignments that I wanted to do.

I went over to the [College of Engineering Control Systems Lab website](http://coecsl.ece.illinois.edu/) and found the [Segbot](http://coecsl.ece.illinois.edu/segbot/segbot.html). Since I stay in a very small apartment, I didn’t think I had enough runway for something as big as the Segbot. This constraint also ruled out a lot of [other](https://www.thingiverse.com/thing:2306541) good designs you can find online.

I came across a [hackaday post](https://hackaday.io/project/11614-worlds-smallest-balancing-robot-minipiddy) for the [MiniPIDDY](https://vine.co/v/OUwrqYE6jHV/embed/simple) by Sean Hodgins. It looked so cute I wanted one for myself. And I wasn’t going to order or wait for a printed board. So I began my impulsive planning to build a mini piddybot.

### Requirements

The requirements were simple, I wanted a small self balancing bot that is easy to tune. I read comments on the hackaday post and found the motors, and also got amazed at how well the miniPIDDY balanced without motor encoders! I was definitely going to make this. I saw upgraded versions of Sean’s self balancing bot, but I didn’t want potentiometers for PID tuning, I want wireless! I would’ve bought the MiniPIDDY if it was available, so printed boards are not going to cut it, I want something modular! I don’t want to remove batteries to charge them, I want a charging circuit built in! Asking for too much? I guess I was!

### Parts

- 1x [Adafruit Feather M0 Bluefruit LE](https://www.adafruit.com/product/2995)
- 1x [MPU-9250 IMU Breakout](https://www.sparkfun.com/products/13762)
- 1x [DRV8835 Dual Motor Driver](https://www.pololu.com/product/2135)
- 2x [Sub Micro Planetary Gearmotor (136:1)](https://www.pololu.com/product/2358)
- 2x [Wheels for Sub Micro Gearmotor](https://www.pololu.com/product/2356)
- 1x [3.7v Lithium Ion Battery – 1200mAh](https://www.adafruit.com/product/2011)
- 1x [SPDT Slide Switch (PCB Friendly)](https://www.adafruit.com/product/805)
- 1x [Perforated Prototype PCB Board](https://www.amazon.com/9x15CM-Single-Prototype-Perforated-Through/dp/B075F2LWZD/ref=sr_1_4?s=industrial&ie=UTF8&qid=1522543545&sr=1-4&keywords=perforated+pcb&dpID=51NHWh0SjrL&preST=_SX342_QL70_&dpSrc=srch)
- 1x 3D Printed base
- [Hookup Wire](https://www.sparkfun.com/products/11375)
- Male and Female header pins

### Connections

The connections are pretty straightforward. The MPU-9250 is connected to the micro controller via I2C and the motor driver via PWM. If you’ve noticed that the motors run on 6v and that the maximum battery output is 4.2v, you’ve already started realizing why this might fail. I decided to go for it anyway. The website said the motors would still move at voltages as low as 3v but with lower torque, seemed fair enough.

I don’t have many photos of the making process, so I tried using Fritzing to make it simpler. I’m new to Fritzing and I hope it’s understandable.

![Fritzing Circuit](https://i0.wp.com/impulsiverobotics.com/wp-content/uploads/2018/04/Screen-Shot-2018-04-08-at-6.08.10-PM.png?resize=1024%2C659&ssl=1 "Fritzing Breadboard Circuit. Sensor stick: MPU 9250, Micro Metal Gearmotor: Sub Micro Plastic Gearmotor")

![Circuit back](https://i0.wp.com/impulsiverobotics.com/wp-content/uploads/2018/04/IMG_20180408_180542.jpg?resize=128%2C300&ssl=1 "'Art.'")

### Design

This clearly needs a base/chassis, so I decided to 3D print it, why not? The only problem is that I never worked on CAD software. I searched for the best CAD software for dummies and found 123D Design. I literally made cylinder holes in a cuboid for the motors and made two versions: one with 35% of the cuboid cut off from the bottom and the other 40%.

![design](https://i0.wp.com/impulsiverobotics.com/wp-content/uploads/2018/04/Screen-Shot-2018-04-08-at-5.17.42-PM.png?resize=1024%2C534&ssl=1 "123D Design in various stages")
I don’t intend to put the design up on Thingiverse. If you still want it, [you can have it](https://impulsiverobotics.com/wp-content/uploads/2018/04/piddytry.zip).

### Assemble

To my surprise the earlier design technique worked and the motors directly snap fit into the 35% version. I was so excited by my first snap fit mechanism that I took a picture.

This is the earliest one I have.

![](https://i0.wp.com/impulsiverobotics.com/wp-content/uploads/2018/04/IMG_20170401_145912-220x300.jpg?resize=220%2C300)

This was my first experience with the [Adafruit Feather](https://www.adafruit.com/feather) line of products. Little did I know that I’d fall in love with them so much. You can clearly see in the picture that the battery is too big.

Everything assembled like I imagined. The board fit snugly and the battery didn’t fall off easily. After assembling everything and holding it in my hand, I knew this wouldn’t work. Those toy car wheels couldn’t control the weight of the thing.

At this point I was prepared for the worst, there could be a random short or the battery could just puff up, I wouldn’t care. I was very happy I got back to DIY-learning after coming to the US with close to no tools to begin with.

I then wrote a simple blink program to test the motors, switched it on, and it worked! Everything was fine, so I decided to go ahead with porting [Sean Hodgins’ code](http://www.idlehandsproject.com/piddybot-a-self-balancing-teaching-tool/). It was pretty straightforward, I only had to change from the sensor stick to MPU9250 libraries and slightly tweak reading the IMU values.

Aaaaand, it never balanced itself. Technically, all of the code worked the way it was supposed to, the motors compensate for the tilt, but the ratio between the robot’s weight and the motor power was way off. The motors could lift the weight of the robot, but weren’t nearly fast enough at that power supply. Getting the faster version of those motors would mean lesser torque.

### Problems

There were quite a few problems with this attempt:

1. **Encoders:** Use them. The original Piddybot works, but use encoders.
2. **Motors:** Get good ones.
3. **IMU Mount:** I got way too inspired by the Piddybot so I wanted to mount my IMU the same way the Sensor stick was mounted on the Piddybot. I never took the time to do a side by side visual comparison of the two IMUs. Should’ve soldered it on aligning a different axis of the IMU with that of the tilt angle.

### Conclusion

I had a lot of fun making this since I was getting used to the new campus and now I know where to get all my stuff for future experiments. Most of the problems encountered were because of the physical properties of the robot and not the electronics, so I guess I got something right. I’m not done with this though, I will try again some time, with [better motors](https://www.pololu.com/category/60/micro-metal-gearmotors) and [encoders](https://www.pololu.com/product/3081).