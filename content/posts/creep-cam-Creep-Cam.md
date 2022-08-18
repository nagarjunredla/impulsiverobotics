---
id: 153
title: 'Creep Cam'
date: '2018-06-26T22:05:11+00:00'
author: 'Nagarjun Redla'
layout: post
guid: 'https://impulsiverobotics.com/?p=153'
permalink: '/?p=153'
image: /wp-content/uploads/2018/06/IMG_20180626_173739.jpg
draft: true
categories:
    - Builds
    - 'In Progress'
---

Back when I was overly fascinated by DIY voice assistant projects, I bought the [Matrix Creator](https://www.matrix.one/products/creator) and backed the [Matrix Voice crowdfunding campaign](https://www.indiegogo.com/projects/matrix-voice-open-source-voice-platform-for-all#/). These boards are pretty powerful, carrying mic arrays, FPGAs, LEDs and a ton of sensors on the Creator. I could set up Alexa back then but there were issues with pulseaudio initially that made it harder to integrate with Google Assistant. The Matrix Creator is packed with features to be a hub for your automated home. But I wasn’t convinced with it just lying there like any other Google Home though it was packed with better hardware.

I decided to borrow a few extras from a robot arm I was building, which were two spare Dynamixel AX-12A servos and an extra alternate 3D printed base. So I decided to make a pan tilt mechanism for the whole thing. With [Amazon Echo Look](https://www.amazon.com/Amazon-Echo-Look-Camera-Style-Assistant/dp/B0186JAEWK), [Google Clips](https://store.google.com/us/product/google_clips?hl=en-US) and [Google Vision Kit](https://aiyprojects.withgoogle.com/vision/) approaching, I think this is the next step for personal assistants anyway, and you’d rather have it search for you than stand in front of it, right?

The Matrix Creator has 8 Mics and a hole in the center for a camera. We could try beamforming to figure out where the user is, but I’ve seen my Echo Dot (which uses beamforming) point the light towards the wall (since my voice reflects off of it) when I spoke to it sometimes, so it isn’t reliable. The next best thing to do is to use the onboard camera. I know what you’re thinking, stick an NCS in there. This Raspberry Pi was the first one I got after coming to America, so I’m pretty surprised it’s still running right now. I will push its USB ports to the limit as you will soon see.

When I first got my DeepLens, I wanted to set its hostname to CreepLens, but decided this one deserves it better, so I named it Creep Cam. This device has just one purpose, to find you, and to look at you sometimes.

And by sometimes, I mean all times.

All the time.

[![Kevin Malone GIF - Find & Share on GIPHY](https://i0.wp.com/media.giphy.com/media/liYOrUY4vrHrO/giphy.gif?resize=245%2C199&ssl=1)](https://giphy.com/gifs/kevin-malone-liYOrUY4vrHrO)

## Hardware

- 1x Raspberry Pi 3B
- 1x [USB2AX XeveLabs](http://www.xevelabs.com/doku.php?id=product:usb2ax:usb2ax)
- 1x Movidius Neural Compute Stick
- 2x Robotis Dynamixel AX-12A Servo
- 1x [SMPS2Dynamixel](http://www.robotis.us/smps2dynamixel/)

If you’ve worked with Raspberry Pi + NCS before, you must be wondering how a second USB fits after the Neural Compute Stick. Well if you are gentle enough with the Pi, you can pull it off. USB2AX is the only one that fits. It’s still a hurdle though, since we’re using the camera and we need visual feedback during development, so I decided to do it in three phases. Figuring out face detection with the NCS, then figuring out servo motor control, and then putting those two together.

## Assembly

The assembly is straightforward. I printed parts made by [someone else on Thingiverse](https://www.thingiverse.com/thing:2272403), and didn’t check that the design included two versions of the base and ended up sending both out for printing. The arm deserved the base with a bottom disc for weight so the fancy 4 legged base goes to the Creep Cam. I [made a base for the Raspberry Pi](https://www.thingiverse.com/thing:2853780) which has holes that to be mounted to a usual Dynamixel Bracket. Attach the base to the bracket and then mount the Raspberry Pi with my favorite nylon standoffs.

[![](https://lh3.googleusercontent.com/KJVqKTwBjCbpCJh_oa3BvDdDhOaK36QE1gltHbj4DzGXZYixOdx2VfQSopTxtMu8NEp3vl3zazG0OZYGqxYhISIjM7p9qOaEZv_YUUHWnkEm2O4dv9hS9CYDvfW7QIvMPXHdhS5MLF8=w600)](https://photos.google.com/share/AF1QipON1N7jw72mDd-KQkPhzDgW8NA4IyozRl7Ih24ruEsexavzUaKz5T1x--jUXsws1g?key=OF9mOEVDOWQ0OHkyV3dIcnJMa3llTmdOeVl5Rk53&)

[![](https://lh3.googleusercontent.com/E5lqexMmw-5Er_JTd_6mpLtQq-r5JROYYM_Mve0yYQbGP_Zbkb3gWFXCLScCESVFgM7le9s1BY0JN02IHBCp5anmQFSXvTCYxaeV_hUN-IFxZO82zNNhYyypvVyHik0MxOZ_D1NpheA=w600)](https://photos.google.com/share/AF1QipNgf-yFl3TR1jS1AUTTq56loOjuU2h5wvbwoTB80m27_f_0wFyarYZsRsiNdNYm2Q?key=MklFS0JDYi13NUF2ZC05RkJFRkpWMEpsSGg5cjRR)

[![](https://lh3.googleusercontent.com/e3iG50RRii_PPK4pu4pj_eJGgBTMFlS2ZzmBzAsnHUyAEkBp_8ajv7p4KZHcanYrZI80CxIFr2S-hCimbGMftMVgvbyr0ANCf9Cq3cu-R6u0iAax9JePl2YruZ6W8t991tFuZnk5e4k=w600)](https://photos.google.com/share/AF1QipMLt1uVErwmixG6Wm4-zozXHwJwOD3s_9fLQ0MAfM_AJquJDw6D7_BroZU_jvIiCA?key=ZllMT3o3YUZIT18waVRUS09VbzhKS0p1elNHTkdB)

[![](https://lh3.googleusercontent.com/u7Xgp7lidb6hBrv1OsClEs3S-Sfg8TV3YoGpwCYxCWfcUJq8ESIKl6X4uOC5R1Y8AswVQRCIDUOoRGzf2j7_D6ZRV0K1eUxAPr-RG-OdU9rnOIDbM0RIdrxhh9aOvojm9cX3kl31pi8=w600)](https://photos.google.com/share/AF1QipN2OTBK76ilQpN4psPGvZnZcOt3-VK2YKqyxgluvMvijUDTmWhbLNWU_4kQuz3rHA?key=TXR4aGF5bzdQQ19YSzVQbnBlT3hpcUYzb2ZpNlZn)

It fits perfectly on top of a Bose Build Speaker, and was the perfect height for a Google Home.

> <div style="padding:16px;"> [<div style=" display: flex; flex-direction: row; align-items: center;"><div style="background-color: #F4F4F4; border-radius: 50%; flex-grow: 0; height: 40px; margin-right: 14px; width: 40px;"></div><div style="display: flex; flex-direction: column; flex-grow: 1; justify-content: center;"><div style=" background-color: #F4F4F4; border-radius: 4px; flex-grow: 0; height: 14px; margin-bottom: 6px; width: 100px;"></div><div style=" background-color: #F4F4F4; border-radius: 4px; flex-grow: 0; height: 14px; width: 60px;"></div></div></div><div style="padding: 19% 0;"></div><div style="display:block; height:50px; margin:0 auto 12px; width:50px;"><svg height="50px" version="1.1" viewbox="0 0 60 60" width="50px" xmlns="https://www.w3.org/2000/svg" xmlns:xlink="https://www.w3.org/1999/xlink"><g fill="none" fill-rule="evenodd" stroke="none" stroke-width="1"><g fill="#000000" transform="translate(-511.000000, -20.000000)"><g><path d="M556.869,30.41 C554.814,30.41 553.148,32.076 553.148,34.131 C553.148,36.186 554.814,37.852 556.869,37.852 C558.924,37.852 560.59,36.186 560.59,34.131 C560.59,32.076 558.924,30.41 556.869,30.41 M541,60.657 C535.114,60.657 530.342,55.887 530.342,50 C530.342,44.114 535.114,39.342 541,39.342 C546.887,39.342 551.658,44.114 551.658,50 C551.658,55.887 546.887,60.657 541,60.657 M541,33.886 C532.1,33.886 524.886,41.1 524.886,50 C524.886,58.899 532.1,66.113 541,66.113 C549.9,66.113 557.115,58.899 557.115,50 C557.115,41.1 549.9,33.886 541,33.886 M565.378,62.101 C565.244,65.022 564.756,66.606 564.346,67.663 C563.803,69.06 563.154,70.057 562.106,71.106 C561.058,72.155 560.06,72.803 558.662,73.347 C557.607,73.757 556.021,74.244 553.102,74.378 C549.944,74.521 548.997,74.552 541,74.552 C533.003,74.552 532.056,74.521 528.898,74.378 C525.979,74.244 524.393,73.757 523.338,73.347 C521.94,72.803 520.942,72.155 519.894,71.106 C518.846,70.057 518.197,69.06 517.654,67.663 C517.244,66.606 516.755,65.022 516.623,62.101 C516.479,58.943 516.448,57.996 516.448,50 C516.448,42.003 516.479,41.056 516.623,37.899 C516.755,34.978 517.244,33.391 517.654,32.338 C518.197,30.938 518.846,29.942 519.894,28.894 C520.942,27.846 521.94,27.196 523.338,26.654 C524.393,26.244 525.979,25.756 528.898,25.623 C532.057,25.479 533.004,25.448 541,25.448 C548.997,25.448 549.943,25.479 553.102,25.623 C556.021,25.756 557.607,26.244 558.662,26.654 C560.06,27.196 561.058,27.846 562.106,28.894 C563.154,29.942 563.803,30.938 564.346,32.338 C564.756,33.391 565.244,34.978 565.378,37.899 C565.522,41.056 565.552,42.003 565.552,50 C565.552,57.996 565.522,58.943 565.378,62.101 M570.82,37.631 C570.674,34.438 570.167,32.258 569.425,30.349 C568.659,28.377 567.633,26.702 565.965,25.035 C564.297,23.368 562.623,22.342 560.652,21.575 C558.743,20.834 556.562,20.326 553.369,20.18 C550.169,20.033 549.148,20 541,20 C532.853,20 531.831,20.033 528.631,20.18 C525.438,20.326 523.257,20.834 521.349,21.575 C519.376,22.342 517.703,23.368 516.035,25.035 C514.368,26.702 513.342,28.377 512.574,30.349 C511.834,32.258 511.326,34.438 511.181,37.631 C511.035,40.831 511,41.851 511,50 C511,58.147 511.035,59.17 511.181,62.369 C511.326,65.562 511.834,67.743 512.574,69.651 C513.342,71.625 514.368,73.296 516.035,74.965 C517.703,76.634 519.376,77.658 521.349,78.425 C523.257,79.167 525.438,79.673 528.631,79.82 C531.831,79.965 532.853,80.001 541,80.001 C549.148,80.001 550.169,79.965 553.369,79.82 C556.562,79.673 558.743,79.167 560.652,78.425 C562.623,77.658 564.297,76.634 565.965,74.965 C567.633,73.296 568.659,71.625 569.425,69.651 C570.167,67.743 570.674,65.562 570.82,62.369 C570.966,59.17 571,58.147 571,50 C571,41.851 570.966,40.831 570.82,37.631"></path></g></g></g></svg></div><div style="padding-top: 8px;"><div style=" color:#3897f0; font-family:Arial,sans-serif; font-size:14px; font-style:normal; font-weight:550; line-height:18px;">View this post on Instagram</div></div><div style="padding: 12.5% 0;"></div><div style="display: flex; flex-direction: row; margin-bottom: 14px; align-items: center;"><div><div style="background-color: #F4F4F4; border-radius: 50%; height: 12.5px; width: 12.5px; transform: translateX(0px) translateY(7px);"></div><div style="background-color: #F4F4F4; height: 12.5px; transform: rotate(-45deg) translateX(3px) translateY(1px); width: 12.5px; flex-grow: 0; margin-right: 14px; margin-left: 2px;"></div><div style="background-color: #F4F4F4; border-radius: 50%; height: 12.5px; width: 12.5px; transform: translateX(9px) translateY(-18px);"></div></div><div style="margin-left: 8px;"><div style=" background-color: #F4F4F4; border-radius: 50%; flex-grow: 0; height: 20px; width: 20px;"></div><div style=" width: 0; height: 0; border-top: 2px solid transparent; border-left: 6px solid #f4f4f4; border-bottom: 2px solid transparent; transform: translateX(16px) translateY(-4px) rotate(30deg)"></div></div><div style="margin-left: auto;"><div style=" width: 0px; border-top: 8px solid #F4F4F4; border-right: 8px solid transparent; transform: translateY(16px);"></div><div style=" background-color: #F4F4F4; flex-grow: 0; height: 12px; width: 16px; transform: translateY(-4px);"></div><div style=" width: 0; height: 0; border-top: 8px solid #F4F4F4; border-left: 8px solid transparent; transform: translateY(-4px) translateX(8px);"></div></div></div><div style="display: flex; flex-direction: column; flex-grow: 1; justify-content: center; margin-bottom: 24px;"><div style=" background-color: #F4F4F4; border-radius: 4px; flex-grow: 0; height: 14px; margin-bottom: 6px; width: 224px;"></div><div style=" background-color: #F4F4F4; border-radius: 4px; flex-grow: 0; height: 14px; width: 144px;"></div></div>](https://www.instagram.com/p/BiP3KeiHjtB/?utm_source=ig_embed&utm_campaign=loading)[A post shared by Nagarjun Redla (@nagarjunredla)](https://www.instagram.com/p/BiP3KeiHjtB/?utm_source=ig_embed&utm_campaign=loading)
> 
> </div>

<script async="" src="//platform.instagram.com/en_US/embeds.js"></script>

Now let’s try making it work.

## Setup

I started with a fresh Raspbian Stretch full install, [removed bloat](https://github.com/raspberrycoulis/remove-bloat/blob/master/remove-bloat.sh) and enabled SSH and Camera.

### Neural Compute Stick Setup

Follow the instructions on [the website](https://movidius.github.io/ncsdk/) and install the NCS SDK. This takes a long time on the Raspberry Pi and you’ll encounter a bunch of errors so keep monitoring the installation. I had troubles with Cython. The apt-get method worked better for me than a pip install. Build the examples also since that sets up OpenCV for you as well. The installation/build process runs tests on the Neural Compute Stick (which needs to be connected btw). I wanted to check out the NCAppZoo repo but didn’t want to run out of space early.

Now that the NCS is ready lets get the Dynamixel SDK ready.

### Dynamixel Setup

[Clone the repository](https://github.com/ROBOTIS-GIT/DynamixelSDK), and follow the [instructions for Python](http://emanual.robotis.com/docs/en/software/dynamixel/dynamixel_sdk/library_setup/python_linux/#python-linux) in the e-manual. USB2AX is supposed to work directly with DynamixelSDK 2 so that shouldn’t be an issue.

This is where things get weird. I’ve owned these Dynamixels for almost a year now, and in my previous quests of finding the most elegant way to control these motors, I ended up with a [selectively compatible shield with no library support](https://www.dfrobot.com/product-958.html) and a [board from the manufacturer](http://support.robotis.com/en/product/controller/opencm9.04.htm) with [a big base](http://www.robotis.us/opencm-485-expansion-boar/). I used the OpenCM 9.04 to do the initial set up (ID) for the motors. The Baud Rate was initially set at 1Mbps. When running tests/read\_write.py , I encountered a lot of problems using 1Mbps so I eventually set it to their default 57600 (again using OpenCM).

At this point I realized the base is too slippery and light compared the the heavy head. Would recommend rubber stoppers on the bottom.

https://giphy.com/gifs/diy-Rd6FwzXXhNtRJ6bLSg

I’m amazed at how much abuse this thing can take.

## Build

### Neural Network

I expected this to be a lot more straightforward than it turned out to be. The [NCAppZoo](https://github.com/movidius/ncappzoo) doesn’t have any face detection algorithms that run on a single NCS, popular pre-trained networks like MTCNN use multiple neural networks, [other networks](https://github.com/yeephycho/tensorflow-face-detection) can’t be compiled easily to NCS graphs and to top it off, Google’s Vision kit comes with a [face detection demo](https://github.com/google/aiyprojects-raspbian/blob/aiyprojects/src/aiy/vision/models/face_detection.py) that works like a charm but [isn’t portable to the NCS](https://github.com/google/aiyprojects-raspbian/issues/330). There’s no point in downgrading to the Pi Zero just yet, and I don’t want to have a dangling Movidius sticks with USB extensions. I have two NC Sticks (Thanks for the free one Intel!), but no, I only want to use one.

This only means one thing, I need to train a model myself. I’ve been setting aside cloud resources to spend on the “A Neural Network a Day” project and training weekend is coming up.

Will come back with updates.