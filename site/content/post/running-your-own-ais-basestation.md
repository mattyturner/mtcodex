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

My Raspberry Pi was recently setup with the latest version of Raspbian via the Raspberry Pi Imager (I had already installed the excelletn PiHole on it)