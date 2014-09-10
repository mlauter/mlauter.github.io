---
layout: post
title: "How to Make Your Own Smart AC"
description: Or, the Internet of Things comes to my room. 
date: 2014-09-08T19:56:51-04:00
tags: internet of things, home automation, DIY, instructable, raspberry pi, flask
---

<img align="right" src="http://techli.com/wp-content/uploads/2011/10/415.jpeg">
Remember that late-90s Disney Channel Original Movie [*Smart House*](http://en.wikipedia.org/wiki/Smart_House_(film)) in which a young, motherless, computer-genius boy wins a fully automated robot-house, only to have the house-software come alive as an overbearing 50s-esque mother who locks the boy and his family indoors 'for their own good'? 
<p><br /></p>
...maybe you don't remember. But *I* do, and I have always wanted to build that. Except without the sexist/robot-takeover part (jeez Disney, what is your deal with mothers?).

<p><br /></p>
<!-- <p><br /></p>  -->

Well, I didn't quite do that. But, I did build a pretty neat device and web app for controlling my air conditioner. 
<!-- <p><br /></p> -->
<!-- how to phrase?? -->

# Here's a step-by-step guide to what I did.
*Note, before starting this project, I'd never used a raspberry pi (I'd done just a tiny bit of Arduino), I'd never written a server, and I'd never made a web app of any kind. So, if you're coming to this with just a bit of programming experience, take heart! This is a totally doable project.*

## Parts and Tools

![toolkit](../images/ac_project_toolkit.jpg)

### Components of the finished project
1. Raspberry Pi (I used [this](https://www.adafruit.com/products/1914) one)
1. Power Switch (or just a relay)
1. USB wifi for raspberry pi
1. A micro SD card
1. Digital temperature sensor
1. Micro USB charger for the raspberry pi
1. 4.7K resistor
1. Jumper wires, both [male](http://www.ebay.com/itm/40-Pin-20cm-Dupont-Wire-Connector-Cable-2-54mm-Male-to-Male-1P-1P-For-Arduino-/221455439425?ssPageName=ADME:X:RTQ:US:1123) and [female](http://www.ebay.com/itm/Dupont-Wire-20cm-Cable-Line-Color-40P-40P-Test-Lines-Connector-/221402540531)
  *Tip! this ebay seller is reliable and cheap*
1. [Prototype Paper PCB ](http://www.ebay.com/itm/10pcs-DIY-Prototype-Paper-PCB-Universal-Experiment-Matrix-Circuit-Board-5-x-7cm-/221542018120]) (DIY circuit board)
1. Your computer

### Tools
1. Soldering iron and solder
1. A very very skinny phillips head screwdriver
1. A hot glue gun

### For testing/setup only
1. A [breadboard](https://www.adafruit.com/products/64)
1. An [LED](https://www.sparkfun.com/products/9590)
1. A monitor, keyboard, and ethernet cable (for setting up the pi)

## Setting up the Pi
I cannot stress this strongly enough: __Adafruit tutorials for allll the things!__ Their tutorials are super informative and easy to follow. Plus they also sell all the parts. It's a one stop super shop for all things hardware.

#### Download an OS image 
I used Raspbian, and followed this [adafruit tutorial](https://learn.adafruit.com/adafruit-raspberry-pi-lesson-1-preparing-and-sd-card-for-your-raspberry-pi/downloading-an-image)

#### Find your pi on the network and configure wifi
