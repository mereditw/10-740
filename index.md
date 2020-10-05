## Introduction

### Summary
Our project is to create a connected system of storm drain sensors that will detect changes in water level in order to provide real time flood updates at a more granular level.  In addition to the water level changes we will also include temperature and humidity sensors to provide more context for the water level rise and potentially increase fidelity of the readings

### Motivation
As flooding becomes a larger problem due to climate change areas that are not usually flooded are now becoming susceptible to flash flooding.  Currently notification systems work at a high level and are not in real time.  With these sensors we can quickly and accurately provide real time information for flood risks at a granular level.
## Goals
By the end of the project we are hoping to acheive the following:
  1. Create a working model for our storm drain sensor that accurately detects changing levels of water and gives off an alert
  2. Connect our sensors to create a system of detection
  
 
For Progress Report
## Current Progress
Sp far we have began building out the circuits for our sensor and writing the code for our sensors in order for them to work individually
### Highlights: In particular, articulate thing(s) you have learned / solved outside of what was taught in class
### Problems Encountered

## Future Plan
In the next two weeks we will complete the following:
  - Create a methodology for testing our sensor under varying conditions to finalize and adjustments that need to be made to ensure a proper system
  - utilize that methodology to create and test a working system
  - connect our sensors to simulate our usecase
  - continue to update the github page as progress continues
## Methodology
### Phenomena of Interest
Describe the physical phenomena of interest, e.g. physical principles, static and dynamic behavior, and signal characteristics
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
