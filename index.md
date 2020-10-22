video link: https://vimeo.com/471164010

## Introduction

### Summary
Our project is to create a connected system of storm drain sensors that will detect changes in water level in order to provide real time flood updates at a more granular level.  In addition to the water level changes we will also include temperature and humidity sensors to provide more context for the water level rise and potentially increase fidelity of the readings.

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

#### Temperature and Humidity Sensor DHT11

<img src="https://images-na.ssl-images-amazon.com/images/I/61MKNxCnIwL._SL1280_.jpg" width="250">

Figure 2: Temperature and Humidity Sensor (source:https://images-na.ssl-images-amazon.com/images/I/61MKNxCnIwL._SL1280_.jpg)

##### Physical Principles
Temperature is sensed through semiconductive material thermistor component of the sensor, which changes its resistivity based on the temperature. The resistivity decreases exponentially with an increase in temperature. Additionally, humidity is sensed through two electrodes containing a moisture capturing substrate between them. The substrate decreases the electrodes’ resistivity with an increase in humidity. 
##### Sensor Parameters
The DHT11 sensor has three pins, a digital output data pin, a VCC pin for power, and a ground connection. a
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
### Experiments and Results

In order to test the water level sensor and ensure it was accurately measuring the current level we ran seperate tests to calibrate our sensors.  We found that the sensor does not behave linearly to rising water and begins to act logarithmically as the water reached its highest levels.  Below we have the trasfer functions we used for each of our sensors:

INSERT TRANSFER FUNCTIONS

After we had calibrated our sensors we began to look at how we would sample the data and convert it into a change in water level.  When conducting out experiments we noticed that the sensor took time to settle down and get an accurate measurement due to ripples in the water.  This led us to use a moving average of the data in order to eleiminate the potential problems caused from these effects.  

Once we had settled on the moving average we began to decide on a threshold to trigger an alert that flooding was possible.  Using the containers we had we calibrated a threshold change in flow rate that would trigger the alarm and alert the nearby sensors that flooding would be occuring.

Finally we implemented openchirp to allow for the system of sensors to communicate with eachother and enable a network of sensors that would have information about surrounding areas and potential flooding effects.

Steps of final experiment:

1. Both sensors were turned on and published the current water level to openchirp.
2. Once water level was published sensor takes measurements every 0.5 seconds over a 2.5 second time period and averages the flow rate. If current flow rate is safe and turns on a green LED if safe and a red LED if unsafe.  
3.  Once this is determined the data is published to Openchirp and the sensor pulls the information from other sensors to process.
4. Once the sensor receives the information from the other sensors it compares this to the threshold for that sensor container and turns on a yellow LED if the flow rate is unsafe nad a blue LED if it is safe.
5.  This process repeats

### Discussion

From this project we were able to implement everything we learned throughout the course and make something that has a practical use case.  From knowing very little about sensors and signal processing to being able to create a system of sensors that communicate with eachother.  From our results it is clear that an inexpensive and distributed system could be implemented in storm drains to increase awareness of potential flooding in real time.  In addition to this being able to create a network of these sensors can allow for emergnecy personal to easily navigate around flooded areas in case of emergencies.  In the future and going forward it would be beneficial to look at adding additional sensors to detecting the rising water level and possible alternative means for measuring flow rate that does not require calibration of each sensor.  




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
