## 4180 Project - Gas Detection Rover
A Raspberry Pi 4 will be used to add additional functions to a mindtank's built-in system. The MIND tank will be communicated and controlled through a mobile app using wifi. The camera will be on the front of the MIND Tank and send video feedback to the controlling computer, while the chemical sensors on the sides will print the concentration of their corresponding gas. A laser temperature sensor will be used to detect human temperature, which will also be printed on the feed. A red LED will indicate whether the chemical sensor is reading over 30 degrees Celsius.

<img src="https://github.com/abidrun/4180project/blob/main/Gas%20Rover.jpg" alt="" width="400">

## Demo

Click to play.

[![IMAGE ALT TEXT HERE](https://img.youtube.com/vi/puAbpk_hmp0/0.jpg)](https://youtu.be/puAbpk_hmp0)

Project features:
- Communicate and control through mobile app: 

   <img src="https://github.com/abidrun/4180project/blob/main/DemoPhotos/MobileApp.png" alt="" width="400">
   
- Camera sends video feedback:

   <img src="https://github.com/abidrun/4180project/blob/main/DemoPhotos/VideoView.png" alt="" width="400">
   
- Print concentration of gases:

   <img src="https://github.com/abidrun/4180project/blob/main/DemoPhotos/GasData.png" alt="" width="400">
   
- Print temperature:

   <img src="https://github.com/abidrun/4180project/blob/main/DemoPhotos/TemperatureData.png" alt="" width="400">
 
 - LED indication:
 
    <img src="https://github.com/abidrun/4180project/blob/main/DemoPhotos/LEDUnlit.png" alt="" height="200"><img src="https://github.com/abidrun/4180project/blob/main/DemoPhotos/LEDLit.png" alt="" height="200">
    
    Under 30C &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Above 30C


## Changes from Original Proposal
The following changes were made to the original proposal:

- A red LED was used instead of a RGB LED since only one color was needed.
- Instead of the red LED lighting up after a specific gas concentration, it was coded to light up after a specific temperature instead; this was decided since a high gas concentration could not be safely demonstrated.
- Due to authentication issues, a webpage was not set up and data was sent over SSH instead.
- A Raspberry Pi 4 was used instead of the Raspberry Pi 0 due to its additional capabilities.

Presentation slides: https://docs.google.com/presentation/d/1capM-I1cgk78rcUiF17W0KNmnumCjsVagRdHJYt3gas/edit?usp=sharing

## Team Members
- Abigail Drun
- De-pei Ho
- Manan Patel
- Vandan Patel

## Parts List
[Raspberry Pi 4](https://www.raspberrypi.org/products/raspberry-pi-4-model-b/?resellerType=home)

[MIND Tank](https://www.vincross.com/mindkit/index.html)

[Carbon Monoxide Sensor](https://www.amazon.com/gp/product/B016KABTDK/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=B016KABTDK&linkCode=as2&tag=geek07f-20&linkId=d1923ce3e7e3300909bcb6c569e342a7)

[Methane Sensor](https://www.amazon.com/gp/product/B016KABTDK/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=B016KABTDK&linkCode=as2&tag=geek07f-20&linkId=d1923ce3e7e3300909bcb6c569e342a7)

[Laser Temperature Sensor](https://www.freetronics.com.au/products/irtemp-ir-temperature-sensor-module)

[Pi Camera V2](https://www.raspberrypi.org/products/pi-noir-camera-v2/?resellerType=home)

[Red LED](https://www.sparkfun.com/products/9590)

[Level Shifter](https://www.amazon.com/KeeYees-Channels-Converter-Bi-Directional-Shifter/dp/B07LG646VS/ref=sr_1_2?dchild=1&keywords=level+shifter&qid=1605756694&sr=8-2)

[ADC](https://www.adafruit.com/product/856)

## How to Use
Before use: Install [gst launch](https://gstreamer.freedesktop.org/documentation/installing/on-mac-osx.html?gi-language=c)

1. Using the IP address of the Raspberry Pi, SSH to the Raspberry Pi using the controlling computer.
2. Connect the Raspberry Pi to a monitor.
3. This screen will pop up:
<img src="https://github.com/abidrun/4180project/blob/main/gstreamScreen.jpg" alt="" width="400">

4. Enter the IP address of the controlling computer. 
5. Enter [these](https://github.com/abidrun/4180project/blob/main/gstreamRPI.rtf) commands into Terminal.
6. The video and sensor information should pop up automatically.
7. Unplug the Raspberry Pi now that everything is verified to work correctly.



## Schematic
![Schematic](https://raw.githubusercontent.com/abidrun/4180project/main/4180ProjectSchematic.jpg)
