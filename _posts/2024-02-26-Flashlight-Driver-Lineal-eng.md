---
title: "Linear driver for diving flashlight DV-S9 Led Open Source Hardware."



toc: true
categories:
  - English  
full-width: true
tags:
  - Flashlight
  - Driver Lineal
  - OSHW

 

  

excerpt: "This post explain the design of a variable linear driver for the DV-S9 diving flashlight, the driver is capable of handling currents of up to 3 A the  input voltage range is between  3V and 4.2V, and has low standby current. The driver achieves an efficiency of between 90% and 70% at maximum current. This design is open source hardware, with the design documents stored on Github."

header:  
      
    teaser: /assets/images/2024/Lineal_Led/Teaser.jpg
---
[Español]({% post_url 2024-02-25-Flashlight-Driver-Lineal %})

# Introduction.
In this article, a design for a driver for a commercial Ultrafire DV-S9 diving light LED flashlight will be presented. Many clones of this flashlight can be found online at very affordable prices. It is a widely used flashlight in spearfishing, this torch is functional but with a several problems that will be explained later."

{:style="text-align:center;"}
![Ultrafire DV-S9](/assets/images/2024/Lineal_Led/Teaser.jpg "DV-S9") 

<!-- <img src="/assets/images/2024/DV-S9.jpg" alt="DV-S9" align=”middle”>  -->
This flashlight is powered by a lithium-ion battery either 18650 or 2320, and uses a Cree XM-L2 type LED. Based on the electrical characteristics of the XM-L2,voltages between 3V and 3.4V are needed for currents between 1.5A and 3A. This voltage must be supplied from the batteries, but the voltage provided by lithium-ion batteries ranges from 2.5V to 4.2V. These flashlights use a driver to maintain constant current, typically uses a step-down driver that reduces the battery voltage to the necessary level to supply the required current. Using a step-down driver, a small percentage of battery energy cannot be utilized, as if the battery voltage is lower than the LED voltage, it cannot been illuminated. These drivers can be of the linear type, simpler, or of the switched type, which requires a more complex circuit. 

{:style="text-align:center;"}
![XML2_EC](/assets/images/2024/Lineal_Led/XML2_EC.png "Características eléctricas XML2") 

The stock flashlight is provided of a linear driver based on the AMC7136 integrated circuit, which controls three SOT23 transistors and achieves current control through an Attiny13 microcontroller by activating the regulator via a PWM signal. According to my measurements, the driver can provide a maximum current of 1.5 A and has a standby consumption of 10 mA. To activate the flashlight, it uses a Hall effect switch. The standby current is produce by the Hall effect switch and the microcontroller is relatively high, and causes the battery discharge if it is not disconnected when not in use. Additionally, under certain circumstances, electrolysis can occur in the aluminum casing. 

<!-- {:style="text-align:center;"}
![Driver DV-S9](/assets/images/2024/Lineal_Led/DriverDVS9.jpg "Driver")  -->

{:style="text-align:center;"}
<img src="/assets/images/2024/Lineal_Led/DriverDVS9.jpg" title="Diver Original" width="300" height="auto">
<img src="/assets/images/2024/Lineal_Led/Driver_d.jpg" title="Diseño" width="300" height="auto">

The objective is to design a low-cost driver to replace the original one, allowing current regulation between 1A and 3A without the need for a microcontroller to decrease the standby current.   

# Driver specifications.
The driver design has the following characteristics:

  - Linear type regulator.
  - Adjustable current up to 3A.
  - Input voltage from 3V to 4.2V.
  - Standby current of 4mA.

# The block diagram of the proposed design is as follows:
The block diagram of the proposed design is as follows:

{:style="text-align:center;"}
![Block](/assets/images/2024/Lineal_Led/Sch_LED_Lineal.jpg "Block") 

Basically, the driver design utilizes a linear regulator implemented with an operational amplifier and a MOSFET  transistor to supply the required intensity to the LED. The operational amplifier is fed back with a current meter to control the output intensity on the LED and is controlled by a Hall effect sensor

# Schematics.
The schematic used to implement the design is the following:

{:style="text-align:center;"}
![Schematic](/assets/images/2024/Lineal_Led/Schematic.png "Sch") 

This schematic can be downloaded here: [Esquema](/assets/images/2024/Lineal_Led/Regulador_Lineal_Sch.pdf)

## Power supply
To power the driver, a Li-ion 18650 battery is used, which can supply between 2.5V and 4.2V, the XM-L LED requires voltages between 3V and 3.4V. Therefore, a voltage range between 3V and 4.2V will be designed. Components will be selected for a maximum voltage of 4.2V, and a 3V LDO model HT3170 is used power the control part.

## Current Sensor
A 10mΩ resistor is used as the current sensor, along with an amplifier to raise the measurement voltage. A small resistor is used so that its voltage drop does not limit the LED's supply voltage, allowing the flashlight to be powered with only 3V. For the amplifier implementation, a precision and low-offset OPAMP like the SGM8511 has been chosen.

## Linear Regulator
The circuit uses a typical linear regulator with a MOSFET and an OPAMP, fed back with the current sensor to control the intensity passing through the LED based on the input voltage. For the regulator implementation, the same OPAMP used for the current sensor is chosen, along with the IRLR2905 MOSFET with very low Ron and a low Vgs voltage, enabling the flashlight to be used with an input voltage as low as 3V.

## Switch
The switch controls the distance between a magnet and a S49E Hall effect sensor. To activate the flashlight, the output of the Hall effect sensor is compared with the measured voltage value when the switch is in the middle position of its travel, which is approximately 1.72V. To implement the comparator, an OPAMP of the same model used in the rest of the driver and a transistor are used to eliminate the command from the linear regulator.

## PCB
A two-sided PCB has been designed to fit into the commercial flashlight casing and be easily replaceable. Considerations have been made for the permissible track currents and EMI control rules to ensure reliable current measurement with a small current measuring resistor.
  

{:style="text-align:center;"}
<img src="/assets/images/2024/Lineal_Led/Top.png" title="Top" width="300" height="auto">
<img src="/assets/images/2024/Lineal_Led/Top_pcb.png" title="Top_pcb" width="300" height="auto">

{:style="text-align:center;"}
<img src="/assets/images/2024/Lineal_Led/Bottom.png" title="Bottom" width="300" height="auto">
<img src="/assets/images/2024/Lineal_Led/Bottom_pcb.png" title="Bottom_pcb" width="300" height="auto">

The assembled PCB looks as follows:

![Assembly](/assets/images/2024/Lineal_Led/Build.jpg "Assembly") 

In the Github repository, the manufacturing Gerber files generated for the PCB manufacturer JLCPCB are included, but they are compatible with most PCB manufacturing houses. 

# BOM.
The bill of required materials is as follows; the LCSC component codes have been included

{:style="text-align:center;"}
![BOM](/assets/images/2024/Lineal_Led/BOM.png "BOM") 

At the moment what this post is written, the price of the components for manufacturing 10 units on the LCSC website is $49.75, therefore the component cost per unit would be about $4.975/unit. 

# Measurements.
The following measurements have been taken:

Firstly, the standby intensity is measured with the Hall effect switch deactivated.

{:style="text-align:center;"}
![Vacío](/assets/images/2024/Lineal_Led/Fun_1.jpg "Vacío") 


It is verified that the stand by current is basically that of the Hall effect sensor, totaling 4mA. 

Then, the Hall effect sensor is activated, and the current can be measured with the potentiometer adjusted to 2 A.

{:style="text-align:center;"}
![2A](/assets/images/2024/Lineal_Led/Fun_2.jpg "2A") 


The intensity measured from the voltage across the 10 milliohm resistor with the oscilloscope is as follows:

{:style="text-align:center;"}
![Isense](/assets/images/2024/Lineal_Led/Isense_1.png "Isense")

An intensity of 2A generates a voltage of approximately 20mV across the resistor. It is verified that the ripple of the intensity is small; in no case does it pass through 0..

# Measurement of efficiency and operating voltage.

In order to obtain the driver efficiency, measurements have been taken by varying the input voltage between 2.9 V and 4.2 V, measuring both the input voltage and current, as well as the output voltage and current.

The efficiency as a function of input intensity:

{:style="text-align:center;"}
![Intensidad](/assets/images/2024/Lineal_Led/Intensidad.jpg "Intensidad")

La eficiencia en función de la intensidad de entrada:

{:style="text-align:center;"}
![Eficiencia](/assets/images/2024/Lineal_Led/Eficiencia.jpg "Eficiencia")

It is verified that the flashlight is usable from 3 V with an intensity of 1 A and an efficiency of 98% up to 4.2 V with a minimum efficiency of 70%.


# Beam comparison 

The comparison between the  beam of a standard 1.5 A flashlight and a beam with the flashlight with the driver designed regulated at 2 A is as follows:

{:style="text-align:center;"}
![Haces](/assets/images/2024/Lineal_Led/Haces.jpg "Haz")

It can be seen that the designed driver illuminates slightly more than the original driver.

# Design files and License.

All the documentation for the design, including the KiCad PCB project, manufacturing documents, schematic, and bill of materials, can be found in the following GitHub repository:

[https://github.com/luicer/Linear-Flashlight-DV-S9-Driver](https://github.com/luicer/Linear-Flashlight-DV-S9-Driver)


This design is open source hardware under the permissive CERN OHL-p V2 license."


