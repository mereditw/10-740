video link: [Link](https://vimeo.com/471164010)

## Introduction

### Summary
Our project created a connected system of storm drain sensors that will detect changes in water level in order to provide real time flood updates at a more granular level. Through comparing the rate of the water level rising in a storm drain to a threshold, a sensor can raise an alarm for flooding in real time. Furthermore, multiple storm drain sensors can publish and subscribe to data on OpenChrip, thus allowing individual sensors on the network to warn others of flood risks. In addition to the water level changes we also include temperature and humidity sensors to provide more context for the water level rise and potentially increase fidelity of the readings.

### Motivation
As flooding becomes a larger problem due to climate change areas that are not usually flooded are now becoming susceptible to flash flooding.  Currently notification systems work at a high level and are not in real time.  With these sensors we can quickly and accurately provide real time information for flood risks at a granular level.
## Goals
By the end of the project we are hoping to acheive the following:
  1. Create a working model for our storm drain sensor that accurately detects changing levels of water and gives off an alert
  2. Connect our sensors to create a system of detection
  
## Current Progress

Currently, our experiment is being conducted remotely with one storm drain setup for each of the two members of our team. So far we have began building out the circuits for our sensor and writing the code for our sensors in order for them to work individually. One circuit setup is as follows, where the water level sensor and the temperature and humidity sensor are connected to the Raspberry Pi. We have gotten both sensors to successfully collect data and will soon implement IOT capabilities by publishing to OpenChirp.

<img src="https://i.gyazo.com/4401253eb3fafcbbb1ed63f1fe160c8b.jpg" alt="Image from Gyazo" width="450"/></a>

### Highlights: In particular, articulate thing(s) you have learned / solved outside of what was taught in class
### Problems Encountered
We are still working on converting the voltage reading from the water level sensor to a water height, because the voltage doesn’t seem to scale linearly with the height of the water as water was added to the container.
## Future Plan
In the next two weeks we will complete the following:
  - Create a methodology for testing our sensor under varying conditions to finalize and adjustments that need to be made to ensure a proper system
  - utilize that methodology to create and test a working system
  - connect our sensors to simulate our usecase
  - once the water level sensor’s sampling behavior is more thoroughly tested in settings with waves/ripples, obtain a rate of flow from the change in water height and size of the tank
  - continue to update the github page as progress continues
  
## Methodology
### Phenomena of Interest
#### Water Level
We are interested in measuring the change in water level in a tank and subsequently the flow rate into a container. Given a rectangular tank of length L, width w, height h, and no flow rate out, we can obtain the rate of flow of water into the container through the mass balance equation:

<img src="https://render.githubusercontent.com/render/math?math={\rho(Q_{in} - Q_{out} )} = \rho\frac{dV}{dt}    \longrightarrow">    <img src="https://render.githubusercontent.com/render/math?math={Q_{in}} = L*w*\frac{dh}{dt}">

Knowing the water level and the change in water level over time will allow for sensing when the tank is too full and for further actuating functions, such as turning on a light or raising an alarm. Furthermore, knowing the flow rate into the tank is a useful metric that considers the volume of the tank and not just the height, thus allowing us to understand how fast water is flowing into the tank. Knowing the rate of flow in is useful for communicating with other water level sensors in the network as not all tanks may be of the same volume.
##### Threshold for Flooding
Using the equation above, we were able to determine a threshold for dh/dt at which would constitute an unsafe change in water level. This was done through finding a flow rate which would be considered unsafe given our storm drain mockup size (ex: if a 2.5L container filled up in 20s) and solving for dh/dt. By comparing the difference in water level measurements to the threshold, the system actuated LEDs based on whether the rate was above or below the threshold.

#### Temperature and Humidity
Temperature and relative humidity are useful measures for indicating the occurrence of a storm. Moisture in the air and rapidly rising warm air are common conditions before a thunderstorm, which can be damaging to society and infrastructure through lightning and flooding. Sensing the relative humidity and ambient temperature can be informative for a stormwater system to prepare for an incoming storm through identifying data outside a normal range of conditions.

### Sensor(s) Used
#### Water Level Sensor

<img src="https://images-na.ssl-images-amazon.com/images/I/61cPAXzZ0EL._AC_SX466_.jpg" width="250">

Figure 1: Water Level Sensor (source:https://images-na.ssl-images-amazon.com/images/I/61cPAXzZ0EL._AC_SX466_.jpg)

##### Physical Principles
The water level sensor is able to detect the height of the water level through changing its resistivity. The sensor is built with 5 power and 5 sensing traces which are connected to each other when submerged in water. The resistance of the sensor is inversely proportional to the height of the water, thus the voltage output by the sensor is proportional to the height of the water.
##### Sensor Parameters
This sensor has three pins, an analog voltage output that will be connected to an ADC channel, a VCC pin for power, and a ground connection. The following are parameters of the water level sensor as obtained from the sensor data sheet.
  - Operating voltage between 3.3V – 5V
  - Operating current: <20 mA
  - Water level sensing range: 40mm
  -	Output voltage signal range: 0 – 4.2V

Further testing is needed to confirm the full range of the output voltage signal range by measuring the voltage at the maximum water level height.

##### Static and Dynamic Behavior
This water level sensor is measuring a relatively static phenomena, and it is uncertain how dynamic physical phenomena such as waves and ripples will be detected and identified as a voltage change.

##### Sensor Calibration
Early into testing out our water level sensors, we realized that quality and behavior varied between both of our sensors, as the output voltage given the same water level would differ between our sensors. This could be due to the difference in the conductivity of the water used. Therefore, we each ran tests individually to determine the corresponding voltage given a known water level. For example, Figure 2 is the results of three trials of raising the water level by 0.5 cm about every minute. Fitting a logarithmic curve to the data resulted in a transfer function converting voltage to water height. This is completed for both sensors, yielding different transfer functions.

<img src="https://i.gyazo.com/420211b2c586f00f310eafaef5739518.png" alt="Image from Gyazo" width="600"/></a>

Figure 2: Logarithmic relationship between voltage and water height characterized during calibration

The temperature and humidity sensor did not require any calibration.

#### Temperature and Humidity Sensor DHT11

<img src="https://images-na.ssl-images-amazon.com/images/I/61MKNxCnIwL._SL1280_.jpg" width="250">

Figure 3: Temperature and Humidity Sensor (source:https://images-na.ssl-images-amazon.com/images/I/61MKNxCnIwL._SL1280_.jpg)

##### Physical Principles
Temperature is sensed through semiconductive material thermistor component of the sensor, which changes its resistivity based on the temperature. The resistivity decreases exponentially with an increase in temperature. Additionally, humidity is sensed through two electrodes containing a moisture capturing substrate between them. The substrate decreases the electrodes’ resistivity with an increase in humidity. 
##### Sensor Parameters
The DHT11 sensor has three pins, a digital output data pin, a VCC pin for power, and a ground connection.
  -	Operating voltage between 3.3V – 5V
  -	Operating current: 0.3 mA
  -	Output resolution: 16Bit digital output for both temperature and humidity
  -	Temperature range: 0°C - 50°C
  -	Humidity range: 20% - 90% Relative Humidity
  - Accuracy: ± 1°C, ±1% RH
  -	Sampling Rate: 1 Hz
##### Static and Dynamic Behavior
The temperature and humidity are relatively static phenomena, and the DHT11 sensor is limited to 1 measurement a second, making it appropriate for this application. 

### Signal Conditioning and Processing
Describe the signal conditioning and processing procedures
## Experiments and Results

In order to test the water level sensor and ensure it was accurately measuring the current level we ran seperate tests to calibrate our sensors.  We found that the sensor does not behave linearly to rising water and begins to act logarithmically as the water reached its highest levels.  Below we have the trasfer functions we used for each of our sensors:

INSERT TRANSFER FUNCTIONS

<img src="https://i.gyazo.com/8c425f62bf8ce916518a152af69eeb00.png" alt="Image from Gyazo" width="175"/></a>

After we had calibrated our sensors we began to look at how we would sample the data and convert it into a change in water level.  When conducting out experiments we noticed that the sensor took time to settle down and get an accurate measurement due to ripples in the water.  This led us to use a moving average of the data in order to eleiminate the potential problems caused from these effects.  

Once we had settled on the moving average we began to decide on a threshold to trigger an alert that flooding was possible.  Using the containers we had we calibrated a threshold change in flow rate that would trigger the alarm and alert the nearby sensors that flooding would be occuring.

Finally we implemented openchirp to allow for the system of sensors to communicate with eachother and enable a network of sensors that would have information about surrounding areas and potential flooding effects.

Steps of final experiment:

1. Both sensors were turned on and published the current water level to openchirp.
2. Once water level was published sensor takes measurements every 0.5 seconds over a 2.5 second time period and averages the flow rate. If current flow rate is safe and turns on a green LED if safe and a red LED if unsafe.  
3.  Once this is determined the data is published to Openchirp and the sensor pulls the information from other sensors to process.
4. Once the sensor receives the information from the other sensors it compares this to the threshold for that sensor container and turns on a yellow LED if the flow rate is unsafe nad a blue LED if it is safe.
5.  This process repeats

### OpenChirp

Here are links to both of our OpenChirp devices:

[Meredith's Device](https://openchirp.io/home/device/5f760066004d6e55c61dd47f#visualization)

[Stephen's Device](https://openchirp.io/home/device/5f760062004d6e55c61dd47a#visualization)

Relevant experiment data for transducers “waterlevel_sensor” and “water_level” are able to be viewed by clicking on data from “Last Month”, then zooming in on the graph around 10/20 at 12:00 PM to 12:50 PM, the time in which our experiment was conducted. Figures 4 and 5 show this data taken from OpenChrip, where the x-axis is time and the y-axis is change in water level (cm/s). Large “spikes” in the data show when water was added to the system and the sensor read a positive change in the water level. The sensor response is very different between both of our sensors, though both were tested in a relatively similar way (adding water to the mock-up both slowly and quickly until at capacity). Furthermore, the graphs show fluctuation between positive and negative changes in water level, and while the negative results are misleading (as there was no water removed to lower the water level) they can be explained by the waves and ripples still having an effect on the sensor. This shows the importance of having multiple sensors for the same storm drain to confirm results and reduce erroneous data.

<img src="https://i.gyazo.com/28ab40a1e8834e849e34555d60f5d3d1.png" alt="Image from Gyazo" width="1190"/></a>
Figure 4: Change in water level data - Meredith's sensor

<img src="https://i.gyazo.com/8f561abb890616ddaa68443e98af49c7.png" alt="Image from Gyazo" width="1188"/></a>
Figure 5: Change in water level data - Stephen's sensor

## Discussion

From this project we were able to implement everything we learned throughout the course and make something that has a practical use case.  From knowing very little about sensors and signal processing to being able to create a system of sensors that communicate with eachother.  From our results it is clear that an inexpensive and distributed system could be implemented in storm drains to increase awareness of potential flooding in real time.  In addition to this being able to create a network of these sensors can allow for emergnecy personal to easily navigate around flooded areas in case of emergencies.  In the future and going forward it would be beneficial to look at adding additional sensors to detecting the rising water level and possible alternative means for measuring flow rate that does not require calibration of each sensor.  

## Project Code

Here is the code used for this project. Both sensors used more or less the same code, with slight changes to the transfer function, flood rate theshold, and OpenChirp usenames and passwords.


    import sys
    import busio
    import digitalio
    import board
    import adafruit_mcp3xxx.mcp3008 as MCP
    from adafruit_mcp3xxx.analog_in import AnalogIn
    import time
    import RPi.GPIO as GPIO
    import math
    import numpy as np
    import os
    import glob

    import paho.mqtt.client as mqtt
    import paho.mqtt.subscribe as subscribe

    class Device(mqtt.Client):
        def __init__(self, username, password):
            super(Device, self).__init__()
            self.host = "mqtt.openchirp.io"
            self.port = 8883
            self.keepalive = 300
            self.username = username
            self.password = password

            # Set access credential
            self.username_pw_set(username, password) #set username and pass
            self.tls_set('cacert.pem')

            # Create a dictionary to save all transducer states
            self.device_state = dict()

            # Connect to the Broker, i.e. the OpenChirp server
            self.connect(self.host, self.port, self.keepalive)
            self.loop_start()

        def on_connect(self, client, userdata, flags, rc):
            if rc == 0:
                print("Connection Successful")
            else:
                print("Connection Unsucessful, rc code = {}".format(rc))
            # Subscribing in on_connect() means that if we lose the connection and reconnect then subscriptions will be renewed.
            self.subscribe("openchirp/device/"+self.username+"/#") # Subscribe to all tranducers

        # The callback for when a PUBLISH message is received from the server.
        def on_message(self, client, userdata, msg):
            print(msg.topic+" "+str(msg.payload.decode()))
            # Save device state
            transducer = msg.topic.split("/")[-1]
            self.device_state[transducer] = msg.payload.decode()

        def on_publish(self, client, userdata, result):
            print("Data published")

    # Modify here based on your own device
    username = '5f760062004d6e55c61dd47a' # Use Device ID as Username
    password = '4DODN832wngzxEOL9B0gClpNJRKLPlYo' # Use Token as Password

    username_sub = '5f760066004d6e55c61dd47f' # subscribed device ID and password
    password_sub = 'cf5VgXE3utZwJcoIJC2F68WPwTdBzR9'

    # Instantiate your own device
    water_sensor = Device(username, password)

    # Water level sensor ADC setup
    spi = busio.SPI(clock=board.SCK, MISO=board.MISO, MOSI=board.MOSI)
    cs = digitalio.DigitalInOut(board.D5)
    mcp = MCP.MCP3008(spi, cs)

    # Analog input channel on ADC pin 0
    channel = AnalogIn(mcp, MCP.P0)

    os.system('modprobe w1-gpio')
    os.system('modprobe w1-therm')

    base_dir = '/sys/bus/w1/devices/'
    device_folder = glob.glob(base_dir + '28*')[0]
    device_file = device_folder + '/w1_slave'

    GPIO.setmode(GPIO.BCM)

    red_led = 18 # high flow rate (own sensor)
    green_led = 23 # low flow rate (own sensor)
    yellow_led =  12 # high flow rate (from subscribed sensor)
    blue_led = 16 # low flow rate (from subscribed sensor)

    GPIO.setup(red_led, GPIO.OUT)
    GPIO.setup(green_led,GPIO.OUT)
    GPIO.setup(yellow_led, GPIO.OUT)
    GPIO.setup(blue_led, GPIO.OUT)

    def xfer(v):
        return math.exp((v-1.6)/0.1418)#math.exp((v-1.6)/0.1418)
        #return 0.2459*np.log(v) + 1.5

    def water():

        threshold = 0.022 #2 in or 0.022 #inches/sec
        sensor = "waterlevel_sensor"
        red = "red_led"
        yellow = "yellow_led"
        green = "green_led"
        blue = "blue_led"

        water_sensor.device_state[sensor] = 0

        while True:

            sensor_store = []
            average_store = []

            for i in range(0, 5):
                #print(xfer(channel.voltage))
                if i == 0:
                    sensor_store.append(xfer(channel.voltage))
                    average_store.append(0)
                else:
                    average_store.append(xfer(channel.voltage) - sensor_store[0])
                    sensor_store.append(xfer(channel.voltage))
                    # print(average_store)

                time.sleep(0.5)

            # Read from sensor
            sensor_reading = sum(average_store[1:])/5

            water_sensor.publish("openchirp/device/"+username+"/"+sensor, payload=sensor_reading, qos=0, retain=True)
            water_sensor.device_state[sensor] = sensor_reading
            #data_from = subscribe.simple("openchirp/device/"+username_sub+"/"+sensor, hostname = "mqtt.openchirp.io", port = 8883, auth = {'username' : username_sub, 'password': password_sub}, tls = {'ca_certs':'cacert.pem'})
            data_from = subscribe.simple("openchirp/device/"+username+"/"+sensor, hostname = "mqtt.openchirp.io", port = 8883, auth = {'username' : username, 'password': password}, tls = {'ca_certs':'cacert.pem'})
            print(sensor_reading, data_from)

            if sensor_reading > threshold: # if the flow rate is above some threshold
                GPIO.output(red_led, GPIO.HIGH)
                GPIO.output(green_led, GPIO.LOW)
            if sensor_reading <= threshold:
                GPIO.output(green_led, GPIO.HIGH)
                GPIO.output(red_led, GPIO.LOW)
            if float(data_from.payload) > threshold:
                GPIO.output(yellow_led, GPIO.HIGH)
                GPIO.output(blue_led, GPIO.LOW)
            if float(data_from.payload) <= threshold:
                GPIO.output(blue_led, GPIO.HIGH)
                GPIO.output(yellow_led, GPIO.LOW)
            else:
                pass



    if __name__ == '__main__': 
        try:
            water()
        except KeyboardInterrupt:
            GPIO.cleanup()
            sys.exit(0)




You can use the [editor on GitHub](https://github.com/masciolistephen/10-740/edit/gh-pages/index.md) to maintain and preview the content for your website in Markdown files.

Whenever you commit to this repository, GitHub Pages will run [Jekyll](https://jekyllrb.com/) to rebuild the pages in your site, from the content in your Markdown files.

### Markdown

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/masciolistephen/10-740/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://docs.github.com/categories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.
