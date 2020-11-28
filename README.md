## 4180 Project - Gas Detection Rover
A Raspberry Pi 4 will be used to add additional functions to a mindtank's built-in system. The MIND tank will be communicated and controlled through a mobile app using wifi. The camera will be on the front of the MIND Tank and send video feedback to the controlling computer, while the chemical sensors on the sides will print the concentration of their corresponding gas. A laser temperature sensor will be used to detect human temperature, which will also be printed on the feed. A red LED will indicate whether the chemical sensor is reading over 30 degrees Celsius.

<img src="https://github.com/abidrun/4180project/blob/main/Gas%20Rover.jpg" alt="" width="400">

## Demo

Click on the image to play.

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


## Presentation

Click on the image to play.

[![IMAGE ALT TEXT HERE](https://img.youtube.com/vi/jCLSl4soMps/hqdefault.jpg)](https://youtu.be/jCLSl4soMps)

## Changes from Original Proposal
The following changes were made to the original proposal:

- A red LED was used instead of a RGB LED since only one color was needed.
- Instead of the red LED lighting up after a specific gas concentration, it was coded to light up after a specific temperature instead; this was decided since a high gas concentration could not be safely demonstrated.
- Due to authentication issues, a webpage was not set up and data was sent over SSH instead.
- A Raspberry Pi 4 was used instead of the Raspberry Pi 0 due to more powerful specs.

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

1. Using the IP address of the Raspberry Pi, [SSH](https://www.raspberrypi.org/documentation/remote-access/ssh/) to the Raspberry Pi using the controlling computer.
2. Connect the Raspberry Pi to a monitor.
3. This screen will pop up:
<img src="https://github.com/abidrun/4180project/blob/main/gstreamScreen.jpg" alt="" width="400">

4. Enter the IP address of the controlling computer. 
5. Enter these commands into Terminal.
```
ON MAC:
gst-launch-1.0 -v udpsrc port=9000 caps='application/x-rtp, media=(string)video, clock-rate=(int)90000, encoding-name=(string)H264' ! rtph264depay ! video/x-h264,width=640,height=480,framerate=30/1 ! h264parse ! avdec_h264 ! videoflip method=rotate-180 ! videoconvert ! autovideosink sync=false


ON RPI (USING MAC IP):
raspivid -n -w 640 -h 480 -t 0 -o - | gst-launch-1.0 -v fdsrc ! h264parse ! rtph264pay config-interval=10 pt=96 ! udpsink host=192.168.199.1 port=9000
```
6. The video and sensor information should pop up automatically.
7. Unplug the Raspberry Pi now that everything is verified to work correctly.



## Schematic
![Schematic](https://raw.githubusercontent.com/abidrun/4180project/main/4180ProjectSchematic.jpg)

## Code

### Gas Sensor Code
```python
import RPi.GPIO as GPIO
import time

# change these as desired - they're the pins connected from the
# SPI port on the ADC to the Cobbler
SPICLK = 11
SPIMISO = 9
SPIMOSI = 10
SPICS = 8
mq7_dpin = 26
mq7_apin = 0
mq4_dpin = 20
mq4_apin = 1

#port init
def init():
         GPIO.setwarnings(False)
         GPIO.cleanup()			#clean up at the end of your script
         GPIO.setmode(GPIO.BCM)		#to specify which pin numbering system
         # set up the SPI interface pins
         GPIO.setup(SPIMOSI, GPIO.OUT)
         GPIO.setup(SPIMISO, GPIO.IN)
         GPIO.setup(SPICLK, GPIO.OUT)
         GPIO.setup(SPICS, GPIO.OUT)
         GPIO.setup(mq7_dpin,GPIO.IN,pull_up_down=GPIO.PUD_DOWN)
         GPIO.setup(mq4_dpin,GPIO.IN,pull_up_down=GPIO.PUD_DOWN)

#read SPI data from MCP3008(or MCP3204) chip,8 possible adc's (0 thru 7)
def readadc(adcnum, clockpin, mosipin, misopin, cspin):
        if ((adcnum > 7) or (adcnum < 0)):
                return -1
        GPIO.output(cspin, True)	

        GPIO.output(clockpin, False)  # start clock low
        GPIO.output(cspin, False)     # bring CS low

        commandout = adcnum
        commandout |= 0x18  # start bit + single-ended bit
        commandout <<= 3    # we only need to send 5 bits here
        for i in range(5):
                if (commandout & 0x80):
                        GPIO.output(mosipin, True)
                else:
                        GPIO.output(mosipin, False)
                commandout <<= 1
                GPIO.output(clockpin, True)
                GPIO.output(clockpin, False)

        adcout = 0
        # read in one empty bit, one null bit and 10 ADC bits
        for i in range(12):
                GPIO.output(clockpin, True)
                GPIO.output(clockpin, False)
                adcout <<= 1
                if (GPIO.input(misopin)):
                        adcout |= 0x1

        GPIO.output(cspin, True)
        
        adcout >>= 1       # first bit is 'null' so drop it
        return adcout
#main loop
def main():
         init()
         print"please wait..."
         time.sleep(20)
         while True:
                  COlevel=readadc(mq7_apin, SPICLK, SPIMOSI, SPIMISO, SPICS)
                  CH4level=readadc(mq4_apin, SPICLK, SPIMOSI, SPIMISO, SPICS)

                  if GPIO.input(mq7_dpin) or GPIO.input(mq4_dpin):
                           print("Nothing leaks")
                           time.sleep(0.5)
                  else:
                           print("Gas detected")
                           print"Current CO density is:" +str("%.2f"%((COlevel/1024.)*100))+" %"
                           print"Current CH4 density is:" +str("%.2f"%((CH4level/1024.)*100))+" %"
                           time.sleep(0.5)

if __name__ =='__main__':
         try:
                  main()
                  pass
         except KeyboardInterrupt:
                  pass

GPIO.cleanup()
```
### Temperature Sensor Code
```python
import wiringpi as wp
from gpiozero import LED
from time import sleep

led = LED(16)

class TN9():

    __IRTEMP_DATA_SIZE = 5
    __IRTEMP_TIMEOUT = 2000  # milliseconds
    # Each 5-byte data packet from the IRTemp is tagged with one of these
    __IRTEMP_DATA_AMBIENT = 0x66
    __IRTEMP_DATA_IR =      0x4C
    #__IRTEMP_DATA_JUNK =    0x53; # ignored, contains version info perhaps?

    def __init__(self, pinAcquire, pinClock, pinData, scale):
        self.__pinAcquire = pinAcquire
        self.__pinClock =   pinClock
        self.__pinData =    pinData
        self.__scale =      scale

        # One of the following MUST be called before using IO functions:
        #wp.wiringPiSetup()      # For sequential pin numbering
        # OR
        #wp.wiringPiSetupSys()   # For /sys/class/gpio with GPIO pin numbering
        # OR
        wp.wiringPiSetupGpio()  # For GPIO pin numbering

        if self.__pinAcquire != -1:
            wp.pinMode(self.__pinAcquire, 1)
            wp.digitalWrite(self.__pinAcquire, 1)

        wp.pinMode(self.__pinClock,   0)
        wp.pinMode(self.__pinData,    0)

        wp.digitalWrite(self.__pinClock,   1)
        wp.digitalWrite(self.__pinData,    1)

        self.__sensorEnable(False)

    def getAmbientTemperature(self):
        return self.__getTemperature(self.__IRTEMP_DATA_AMBIENT)

    def getIRTemperature(self):
        return self.__getTemperature(self.__IRTEMP_DATA_IR)

    def __getTemperature(self, dataType):
        timeout = wp.millis() + self.__IRTEMP_TIMEOUT
        self.__sensorEnable(True)

        while True:
            data = [0] * self.__IRTEMP_DATA_SIZE
            for data_byte in range(0, self.__IRTEMP_DATA_SIZE, 1):
                for data_bit in range(7, -1, -1):
                    # Clock idles high, data changes on falling edge, sample on rising edge
                    while wp.digitalRead(self.__pinClock) == 1 and wp.millis() < timeout:
                        pass # Wait for falling edge
                    while wp.digitalRead(self.__pinClock) == 0 and wp.millis() < timeout:
                        pass # Wait for rising edge to sample
                    if wp.digitalRead(self.__pinData):
                        data[data_byte] |= 1 << data_bit
            if wp.millis() >= timeout:
                self.__sensorEnable(False)
                return float('nan')

            if data[0] == dataType and self.__validData(data):
                self.__sensorEnable(False)
                temperature = self.__decodeTemperature(data);
                if self.__scale == "FAHRENHEIT":
                    temperature = self.__convertFahrenheit(temperature)
                return temperature

    def __convertFahrenheit(self, celsius):
        return celsius * 9 / 5 + 32

    def __decodeTemperature(self, data):
        msb = data[1] << 8
        lsb = data[2]
        return (msb + lsb) / 16.0 - 273.15

    def __sensorEnable(self, state):
        if self.__pinAcquire != -1:
            wp.digitalWrite(self.__pinAcquire, not state)

    def __validData(self, data):
        checksum = (data[0] + data[1] + data[2]) & 0xff
        return data[3] == checksum  and  data[4] == 0x0d


if __name__ == "__main__":

    from time import sleep

    PIN_DATA    = 25 # Choose any pins you like for these
    PIN_CLOCK   = 6
    PIN_ACQUIRE = 5
    SCALE="CELSIUS" # Options are CELSIUS, FAHRENHEIT

    SCALE_UNITS = {"CELSIUS" : "°C",
                   "FAHRENHEIT" : "°F"}

    tn9 = TN9(PIN_ACQUIRE, PIN_CLOCK, PIN_DATA, SCALE)

    while True:
        irTemperature = tn9.getIRTemperature()
        ambientTemperature = tn9.getAmbientTemperature()
        if irTemperature > 30:
            led.on()
        else:
            led.off()
        print("Object temperature = %.1f %s, Ambient temperature = %.1f %s" % (irTemperature, SCALE_UNITS[SCALE], ambientTemperature, SCALE_UNITS[SCALE]))
        sleep(1)
```
