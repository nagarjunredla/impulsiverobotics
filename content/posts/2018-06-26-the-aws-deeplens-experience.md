---
id: 143
title: 'The AWS DeepLens Experience'
date: '2018-06-26T06:33:27+00:00'
author: 'Nagarjun Redla'
layout: post
guid: 'https://impulsiverobotics.com/?p=143'
permalink: /2018/06/the-aws-deeplens-experience/
image: /wp-content/uploads/2018/06/IMG_20180626_170916.jpg
categories:
    - Posts
---

I had a great time at Intel AI DevCon 2018. They gave out a few goodies which included an AWS DeepLens preorder code. To be honest, I didn’t really understand the point of it when I preordered it, but I did it anyway since it was free. I didn’t look at the specs till it arrived. I was expecting a device locked into using AWS apps. But boy was I wrong.

## First Impressions

When the DeepLens first arrived, I was surprised to see how big it was. I’d expected it to be smaller, I guess? The Intel Inside badge is what separates this thing from other common developer devices. The device is pretty light and uses a 5V 4A power supply.

![Deeplens and some amazon boxes](https://i0.wp.com/impulsiverobotics.com/wp-content/uploads/2018/06/IMG_20180626_170815.jpg?resize=1024%2C768&ssl=1)

### Tech Specs

This thing has a Dual Core Intel Atom E3930, 8GB RAM and 16GB in-built memory. Compared to the UP Core, it’s two cores lesser but 3 Quarters newer, so it had a bunch of standardized security stuff. A pretty powerful OOTB dev tool if you ask me.

<iframe allowfullscreen="" frameborder="0" height="281.25" scrolling="no" src="https://gfycat.com/ifr/goodnatureddefenselessbongo" width="500"></iframe>

### I/O

![Deeplens IO](https://i0.wp.com/impulsiverobotics.com/wp-content/uploads/2018/06/IMG_20180626_170959.jpg?resize=320%2C427&ssl=1)

The DeepLens has a ton of I/O that you can make use of. It has a micro SD card slot, a micro HDMI, an audio line out and two very misleading USB2.0 ports that are blue like USB3.0. A quick `lsusb` shows that the device is capable of USB3.0, but the camera and the USB ports on the back are on the USB2.0 bus. The AWS component of this device makes it easy to use without a monitor, but I’d recommend a micro HDMI adapter for this.

### Software

Onto the bigger questions. How locked is it? When setting it up, I thought I followed the instructions wrong (or forgot the password I assigned to it) and lost access to the device. I quickly looked up recovery methods and found [this](https://s3.amazonaws.com/deeplens-public/factory-restore/DeepLens_System_Restore_Instruction.pdf), which asks the user to make a bootable Ubuntu 16.04 flash drive along with an SD card with AWS stuff. Looking at that made me realize this isn’t really locked much. This is a full x86 computer with a good camera. You could use the device without the AWS component but I think you’d lose access to the camera.

The DeepLens comes with common frameworks and libraries installed, like OpenCV. The main theme of Intel AI DevCon 2018 was Intel showing off how they’ve optimized the Machine Learning Inference process to run faster on Intel chipsets. They kept pushing the Model Optimizer and Inference Engine included in the OpenVINO toolkit (formerly known as deep learning deployment toolkit) and how you can leverage AI on the edge. The OpenVINO toolkit and Inference Engine documentation talk about Greengrass and lambdas, so I’m assuming this is included.

Looking at the `/opt/` folder, I found `/opt/intel/` and `/opt/aws_cam/` . The Intel folder contained the Deep Learning Deployment Toolkit and the aws\_cam folder included the webserver that streams camera data. Upon close inspection we see that the camera live feed can be found at `/opt/aws_cam/out/ch1_out.h264` and `/opt/aws_cam/out/ch2_out.mjpeg`. This should be enough to run a simple demo.

## Demo

The workflow is simple. Train a model in TensorFlow, Caffe or MXNet (at the moment), then for inference, we need to convert the saved model to what Intel calls Intermediate Representation files (.xml and .bin) using the Model Optimizer on the Development Platform. These files are then fed to the Inference Engine on your Target Platform (device). The [OpenVINO toolkit](https://software.intel.com/en-us/openvino-toolkit) doesn’t include Intel Atom under Development Platform, but I tried and it worked.

### Model Optimizer

The Deep Learning Deployment Toolkit only includes the Model Optimizer for MXNet models, so that’s no good. I went ahead and installed OpenVINO directly. This includes Model Optimizer for Caffe and TensorFlow models.

Download the [Caffe GoogLeNet SSD model](https://software.intel.com/file/609199/download) provided by Intel and unzip it. Then run the model optimizer using

```shell
python3 /opt/intel/computer_vision_sdk/deployment_tools/model_optimizer/mo.py --input_model ~/Downloads/SSD_GoogleNetV2.caffemodel --input_proto ~/Downloads/SSD_GoogleNetV2_Deploy.prototxt --output_dir ~/Downloads/
```

This generates two IR (Intermediate Representation) files `SSD_GoogleNetV2.xml` and `SSD_GoogleNetV2.bin` in the Downloads folder.

### Inference Engine

There are many samples included in the inference engine directory. I’ll try the object detection SSD sample. First, build the examples.

```shell
cd /opt/intel/computer_vision_sdk/deployment_tools/inference_engine/samples
cmake .
sudo make install
cd intel64/Release
```

Now run the Object Detection Demo SSD Async using

```shell
./object_detection_demo_ssd_async -i /opt/awscam/out/ch2_out.mjpeg -m ~/Downloads/SSD_GoogleNetV2.xml -d CPU
```

You should see a stream with predictions. The frame rate averages between 1 and 2 frames per second.

![SSD Inference example](https://i0.wp.com/impulsiverobotics.com/wp-content/uploads/2018/06/screenshot.png?resize=1024%2C576&ssl=1 "Inference Engine SSD Sample")
## Conclusion

This is a very cool tool to have. I had trouble getting a screenshot of better fps performance, since performing any other task, like taking a screenshot, hangs the application for a few seconds. It is critically designed to handle the Inference Engine and AWS lambda and greengrass. I wouldn’t use it for timing critical applications, in such cases it’s better to offload inference to the Movidius NCS anyway. But to try out vision based deep learning at home, this is awesome. The toolkit currently under active development. The Python SDK was in preview when I got the DeepLens but it is now available in a newer release. Unlike my usual problem with running out of ports on SBCs, the DeepLens has a few free ports. Let’s see how I can integrate this into a project.