---
title: 'Running your own AIS basestation. '
date: 2020-04-09T13:02:55.661Z
description: >-
  TLDR: How to setup your own AIS basestation with a Raspberry Pi to monitor the
  Port of Oakland and stream global vessel movements to your laptop.
---
Automatic Identification System, or AIS is the global system by which the location and movements of vessels are is shared. Each vessel as a transponder which emits radio signals in a coded system with various messages which typically include information like their GPS coordinates, whether the vessel is underway or not, bearing, and speed. 

Other vessels AIS systems can receive these messages to display the location of nearby vessels. In addition base stations on land and satellites receive AIS messages and send them over the internet to build a global map of vessel positions. 

If you live near navigable water then it's easy and relatively inexpensive to setup your own base station to monitor what is going by. In addition you can send your data to be aggregated by a service like [AISHub](https://www.aishub.net) which is basically a coop of AIS base stations where every member sends their data in exchange for being able to receive everyone elses. 

So let's get to it!

## Bill of materials

For this project you will need.

1. Raspberry Pi (or other linux based PC)\
   I picked up a Raspberry PI 4 with 4Gb ram, a power supply and a case for [$75 at PiShop.us](https://www.pishop.us)
2. dAISy 2+ AIS receiver\
   [$95 with BNC adapter](https://shop.wegmatt.com/products/daisy-2-dual-channel-ais-receiver-with-nmea-0183?variant=7104314245156)
3. VHF antenna\
   [$32 at time of purchase from Amazon](https://www.amazon.com/gp/product/B000FCP1NO/)

Total $202 plus taxes and shipping

Setting up dAISy

My Raspberry Pi was recently setup with the latest version of Raspbian via the Raspberry Pi Imager (I had already installed the excelletn PiHole on it). For convenient access I had already setup remote SSH and copied my public key over.

Setting up the dAISy device itself is a fairly lengthy and complicated process:
 
1. Unbox
2. Attach the antenna
3. Attach the USB cable from your Pi to the device

At this point you're actually done and it's working! It should show up under `/dev/`. On my Pi it was `/dev/ttyACM0`. You can verify the output by catting.
```
cat /dev/ttyACM0
```

Which output something that looks like:
```
!AIVDM,1,1,,A,B5N`S`000=l5t4UIp@OQ3ws1nDIr,0*7D
!AIVDM,1,1,,B,ENkb9M>T9@@@@@@@@@@@@@@@@@@;WgTu:lmch00003vP000,2*3D
!AIVDM,2,1,6,A,55O6e082>?QULltb221<4L4lu8F2222222222216DpAE54WT0D5DsChB,0*38
!AIVDM,2,2,6,A,p88888888888880,2*6A
!AIVDM,1,1,,B,18LLsJ0viro?JqrE`r9CV2md25ah,0*1F
!AIVDM,1,1,,A,ENkb9M>T9@@@@@@@@@@@@@@@@@@;WgTu:lmch00003vP000,2*3E
!AIVDM,1,1,,A,15O0kl0019o@:pNEWwpkGjad0D1l,0*06
!AIVDM,1,1,,B,15N0E10P00G?f>lE`>w19?wh00RQ,0*40
!AIVDM,1,1,,B,15O0kl0019o@:q@EWwqCG2af0@R3,0*41
```

If it if just outputs empty lines and no messages at all it could be that you are not in range of any vessels. Helpfully the box itself has a status light for each channel which flashes green when it is receiving a message. 

So these are AIS messages. Technically they're a part of the NMEA 0183 standard. If you're interested in that there's a fantastically detailed article entitled [NMEA revealed](https://gpsd.gitlab.io/gpsd/NMEA.html) that explains it on gitlab.

If you just want to get to the juicy stuff, then you'll need to decode these messages. But first, how to get all this data off your Pi?

I just used netcat to broadcast the info to my host.

On your host instruct netcat to listen (-l) on local port (-p) 8888
```
netcat -lp 8888
```

On your pi cat the device as the before as pipe it through to netcat. By default netcat sends data to the specific address / port
```
cat /dev/ttyACM0 | netcat 192.168.0.208  8888
```
You should see the information like before appear on your computer. Now we just need a way to turn this into readable information. 

```
git clone https://github.com/tbsalling/aiscli.git
jar cf aiscli.jar aiscli
mv aiscli.jar /usr/local/bin/aiscli.jar



 

