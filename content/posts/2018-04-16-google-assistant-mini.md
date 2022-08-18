---
id: 38
title: 'Google Assistant Mini'
date: '2018-04-16T09:16:02+00:00'
author: 'Nagarjun Redla'
layout: post
guid: 'https://impulsiverobotics.com/?p=38'
permalink: /2018/04/google-assistant-mini/
image: /wp-content/uploads/2018/04/IMG_20180408_155151-1-e1523870390292.jpg
categories:
    - Builds
    - 'In Progress'
---

When Google announced the AIY Voice Kit, I was very excited. The last time I was that excited, the Raspberry Pi Zero was announced. Once I realized it was being sold along with MagPi, the realization kicked in: I wasn’t going to get my hands on one of these easily, since they sell very very fast. I remember subscribing to a whole year of MagPi and getting it shipped to India so I could get my hands on that one Raspberry Pi Zero with the magazine which was otherwise sold out. Forget WiFi, it didn’t even have a camera connector then! So I REALLY wanted the Voice Kit, and like I guessed, it was sold out.

As of writing this post, the Google AIY Voice Kit is available and pretty cheap too. But then the hot AIY product now is the Google AIY Vision Kit, <del>which is also something I REALLY want at the moment, but is sold out</del> which is in stock now. It ships with the Movidius 2 like the Neural Compute Stick, and I want it so bad that I <del>often find myself scrolling through eBay questioning myself if spending &gt;$120 on it is worth it. (It would otherwise cost $45)</del> got it.

Since saying “I couldn’t build the DIY kit because it was sold out” is self-contradictory, I looked through the interwebs to see if I could find anything. I went on a long journey of evaluating various boards with mic arrays, which I will talk about in another post. There was a small board that I liked called the ReSpeaker 2 mics Hat and it was by Seeed Studio, a company with a stronger track record for DIY projects than the other boards I tried.

<figure aria-describedby="caption-attachment-41" class="wp-caption aligncenter" id="attachment_41" style="width: 459px">[![ReSpeaker 2-Mics Pi HAT](https://i0.wp.com/impulsiverobotics.com/wp-content/uploads/2018/04/dsz3Q3vnIlGIjbZvWpDcyYzD.jpg?resize=459%2C230&ssl=1)](https://www.seeedstudio.com/ReSpeaker-2-Mics-Pi-HAT-p-2874.html)<figcaption class="wp-caption-text" id="caption-attachment-41">Seeed Studio’s ReSpeaker 2-Mics Pi HAT</figcaption></figure>It was mysteriously out of stock at the time, and I attributed it to Seeed Studio pushing their other line of [ReSpeaker products](https://www.seeedstudio.com/ReSpeaker-Core-v2.0-p-3039.html), but as of writing this post they’re [back in stock](https://www.seeedstudio.com/ReSpeaker-2-Mics-Pi-HAT-p-2874.html), and at half the price I got them for (on Amazon).

This board has everything you need to get started on the Google Assistant. There is no EEPROM like the AIY kit, but the official Google Assistant SDK doesn’t work on the Raspberry Pi Zero out of the box anyway, so we don’t really need it. It even has a button and three RGB LEDs! If the AIY Voice Kit was the DIY equivalent of the Google Home, I guess this is the DIY equivalent of the Google Home Mini. As always, I over-planned all the things I thought I could do with this and hit a few roadblocks, but let’s see how far we can go.

### Parts

- 1x [Raspberry Pi Zero W](https://www.raspberrypi.org/products/raspberry-pi-zero-w/)
- 1x 2×20 male header pins
- 1x [Seeed Studio ReSpeaker 2-Mics Pi HAT](https://www.seeedstudio.com/ReSpeaker-2-Mics-Pi-HAT-p-2874.html)
- 1x Micro SD Card
- 1x [Audio Cable](https://www.amazon.com/AmazonBasics-3-5mm-Stereo-Audio-Cable/dp/B00NO73Q84/ref=sr_1_5?ie=UTF8&qid=1523216862&sr=8-5&keywords=line+in+cable&dpID=41VXocT-MNL&preST=_SX300_QL70_&dpSrc=srch)
- 1x Speaker with line in
- M2.5 Nylon screws and standoffs (Optional)

### Raspberry Pi Zero W Headless Setup

This is pretty straightforward. Download the latest [Raspberry Pi Operating System](https://www.raspberrypi.org/downloads/) (I recommend the CLI version since you don’t need the GUI) and [install it on the micro SD card](https://www.raspberrypi.org/documentation/installation/installing-images/). Enable SSH and set up WiFi the wpa\_supplicant way explained best in [this Adafruit post](https://learn.adafruit.com/raspberry-pi-zero-creation/text-file-editing). Insert the SD card and power up the Raspberry Pi Zero, you’re now ready to SSH into the pi. This is the simplest and fastest way to get your Raspberry Pi Zero W up and running.

### Assemble the electronics

Solder the GPIO headers on the Raspberry Pi Zero (or you can be fancy and buy it [with them soldered on](https://www.raspberrypi.org/blog/zero-wh/)) and attach the ReSpeaker hat. The nylon screws are optional but they add to the sturdiness.

### Configuring the Raspberry Pi

##### SSH into the Pi

When you’re building Raspberry Pi projects that are always connected to the internet, you must configure them to change default values. So let’s first ssh into the Pi, the default username is `pi` and password is `raspberry` and can be found at `raspberrypi.local` (or look up the pi’s IP address)

```
ssh pi@raspberrypi.local
```

##### Change default user password

Once you’ve logged in it’s good practice to get used to immediately changing the default user’s (`pi`) password. This ensures someone else doesn’t guess your Raspberry Pi’s default password. Change the password by using the `passwd` command.

##### Change hostname (Optional)

If you thought `raspberrypi.local` sounds too common, we’re going to change that next. If you’re amazing at remembering numbers you can just stick to the IP address (hence the optional step). Start by typing

```
sudo raspi-config
```

in the configuration menu select `Network Options` and then select `Hostname`. Change it to whatever you want. I’m naming this one `google-assistant-mini`. Also enable SPI in `Interfacing Options`. If the config menu doesn’t ask you to reboot when you say finish, do it yourself by typing `sudo reboot` in the terminal. The pi should now reboot, this time with the new hostname. (Optional: Now would be a good time to remove the old hostname from your computer’s ssh records by running `ssh-keygen -R raspberrypi.local`. This ensures there’s no problem when setting up another pi).  
SSH into the pi again, but with the new hostname this time

```
ssh pi@google-assistant-mini.local
```

Once you’re in, make sure everything is up-to-date by running

```
sudo apt-get update && sudo apt-get upgrade
```

If you’re like me and installed the lite version of the OS, git and pip aren’t included and we need them to install drivers for the ReSpeaker Hat. So do that by running

```
sudo apt-get install git python-pip
```

### Install drivers for the ReSpeaker Hat

Information for installing the software can be found on the product’s [Github page](https://github.com/respeaker/seeed-voicecard). Check out their [project page](https://github.com/respeaker/mic_hat) to test the RGB LEDs on the hat. The Google Assistant example on that page is for the library, which isn’t compatible on the pi zero.

### Set up Google Assistant

The Raspberry Pi Zero W is [not supported](https://developers.google.com/assistant/sdk/overview#features) by the Google Assistant Library. It did work for a while in the beginning, but it doesn’t work at all anymore. Since we can’t use the library, that leaves us with the Google Assistant Service with the gRPC API.

Follow the [Google Assistant Service setup guide](https://developers.google.com/assistant/sdk/guides/service/python/). Everything should be straightforward.

And that… was pretty simple to set up. There were driver issues with the hat and the Google Assistant setup for the Pi Zero wasn’t this straightforward when I first got the hat. Then again, that wasn’t too long ago. It’s amazing how fast this side of DIY is moving.

I really liked this board because it’s got just enough to properly emulate a Google Home device. The LEDs can be used to give feedback, the respeaker hat repository has a video of the push button example, but we could use that for the mute button on the Google Home. So the next thing to work on would be to run the assistant along with the LEDs <del>and probably try a headless setup.</del> ([*update: Google bein’ sly*](https://github.com/shivasiddharth/GassistPi/issues/136#issuecomment-362110323))

### Next Steps?

I had a few add-on ideas that could have been slightly overkill for the project, so I stopped myself from putting those on the main discussion.

#### Speaker.

Yeah, the ReSpeaker hat has a speaker output with a JST 2.0 on it. The [WM8960 Speaker Driver](https://www.cirrus.com/products/wm8960/) can drive 1W into an 8ohm speaker. Who would buy a 8ohm speaker with a JST 2.0 anyway? Well I did. There’s no extra setup that you need to do since the same device drives the headphones and the speakers. I could hear the assistant but I had to be close to the tiny speaker I found. I’m no speaker expert but I don’t think a well designed enclosure could produce loud enough results with that tiny a speaker. This could make a really cool mini HAL 9000 with the speaker on the bottom.

#### Make it entirely wireless

The Raspberry Pi Zero W was also meant to be a low power alternative to other pis, so it should make sense to make it wireless by running off of a battery, right? Well, yeah, but the pi still has to connect to a WiFi point and you have to configure it every time. So it is wireless but not entirely mobile. [Adafruit](https://blog.adafruit.com/2015/12/18/how-to-run-a-pi-zero-and-other-pis-from-a-lipo-including-low-battery-raspberry_pi-piday-raspberypi/) linked to a [good post](https://plus.google.com/+DanielBull/posts/gedcmQXaRyr) on adding a PowerBoost 1000C backpack to allow powering the pi with a LiPo battery with a charging circuit. The method mentioned looks like a pretty permanent add-on to the pi and since the hat makes it fat anyway, I don’t need to save so much space. I will add the PowerBoost if I can design a good case for it. The case and battery should add some weight to the device which will help keep it from falling on its side when cables are connected.

#### Add a display

The hat has a JST 2.0 pin for I2C. So I went ahead and bought a Grove RGB backlit LCD Display. The Google Home devices give more than enough feedback with a few LEDs, so I can’t think of a game changing advantage with a simple LCD display.

Or wait, what if you didn’t have access to a speaker but wanted to know what the assistant said anyway (On the go, like with a battery)? Well then…

*TODO: Try LCD possibility*