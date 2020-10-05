## Introduction

### Summary
Our project is to create a connected system of storm drain sensors that will detect changes in water level in order to provide real time flood updates at a more granular level.  In addition to the water level changes we will also include temperature and humidity sensors to provide more context for the water level rise and potentially increase fidelity of the readings

### Motivation
As flooding becomes a larger problem due to climate change areas that are not usually flooded are now becoming susceptible to flash flooding.  Currently notification systems work at a high level and are not in real time.  With these sensors we can quickly and accurately provide real time information for flood risks at a granular level.
## Goals
What are you going to achieve by the end of the project specifically?
For Progress Report
## Current Progress

Currently, our experiment is being conducted remotely with one storm drain setup for each of the two members of our team. The circuit setup is as follows, where the water level sensor and the temperature and humidity sensor are connected to the Raspberry Pi. We have gotten both sensors to successfully collect data will soon implement IOT capabilities by publishing to OpenChirp.

<img src="https://i.gyazo.com/4401253eb3fafcbbb1ed63f1fe160c8b.jpg" alt="Image from Gyazo" width="500"/></a>

### Highlights: In particular, articulate thing(s) you have learned / solved outside of what was taught in class
### Problems Encountered
We are still working on converting the voltage reading from the water level sensor to a water height, because the voltage doesn’t seem to scale linearly with the height of the water as water was added to the container.
## Future Plan
Once the water level sensor’s sampling behavior is more thoroughly tested in settings with waves/ripples, we plan to work on obtaining a rate of flow from the change in water height and size of the container. We also plan to further improve the complexity of our storm drain sensor system.
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

##### Static and Dynamic Behavior
This water level sensor is measuring a relatively static phenomena, and it is uncertain how dynamic physical phenomena such as waves and ripples will be detected and identified as a voltage change.

#### Temperature and Humidity Sensor DHT11

<img src="https://images-na.ssl-images-amazon.com/images/I/61MKNxCnIwL._SL1280_.jpg" width="250">

Figure 2: Temperature and Humidity Sensor (source:https://images-na.ssl-images-amazon.com/images/I/61MKNxCnIwL._SL1280_.jpg)

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
### Experiments and Results
Describe the experiments you did and present the results; Use tables and plots if possible
### Discussion
Discuss the insights from the project




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
