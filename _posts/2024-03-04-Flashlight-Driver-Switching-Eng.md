---
title: "Switching Driver for Diving Flashlight DV-S9 Led Open Source Hardware."
language: english

toc: true
categories: 
  - LED Flashlight
  
full-width: true
tags:
  - Flashlight
  - Driver Switching
  - OSHW

excerpt: "This post explains the design of an adjustable switching regulator driver for DV-S9 diving flashlight, supporting current up to 1 A and input voltage between 3V and 4.2V, along with low standby current. The driver achieves up to 85% efficiency at maximum current. This design is open source hardware with design documents stored on Github."

header:
    teaser: /assets/images/2024/Switching_Led/Teaser.jpg
---

[Spanish]({% post_url 2024-03-03-Flashlight-Driver-Switching %})

# Introduction
Following the design of a linear driver [Link]({% post_url 2024-02-26-Flashlight-Driver-Lineal-eng %}), this article presents a design of a switching driver for a commercial LED flashlight, specifically the Ultrafire DV-S9 diving light. This design aims to achieve higher efficiency, thereby better utilizing battery energy.

{:style="text-align:center;"}
![Ultrafire DV-S9](/assets/images/2024/Switching_Led/Teaser.jpg "DV-S9") 

The objective is to design a low-cost driver to replace the original one, allowing current regulation up to 1 A using a DC/DC switching converter-based driver for regulation.

{:style="text-align:center;"}
<img src="/assets/images/2024/Lineal_Led/DriverDVS9.jpg" title="Original Driver" width="300" height="auto">
<img src="/assets/images/2024/Switching_Led/Driver_SW.png" title="Design" width="300" height="auto">
 
# Driver Specifications
The designed driver features:
- Switching regulator type.
- Adjustable current up to 1 A.
- Input voltage from 3 V to 4.2 V.
- Standby current of 4 mA.

# Block Diagram
The proposed design's block diagram is as follows:

{:style="text-align:center;"}
![Block](/assets/images/2024/Switching_Led/LED_switching.drawio.png "Block") 

The driver design utilizes a DC/DC step-down regulator with current sensing to measure the supplied current to the LED. A DC/DC regulator enable pin is used to turn the system on or off.

# Schematic
The schematic used to implement the design is as follows:

{:style="text-align:center;"}
![Schematic](/assets/images/2024/Switching_Led/Schematic.png "Sch") 

This schematic can be downloaded [here](/assets/images/2024/Switching_Led/Schematic.pdf).

## Power Supply
Similar to the linear driver case, the driver is designed for a supply voltage between 3V and 4.2V. Components are selected for a maximum voltage of 4.2V, and an LDO of 3V model HT3170 is used to stably power the control section.

## Current Sensor
Similar to the linear driver, a 10mΩ resistor and an amplifier are used as the current sensor. For the amplifier implementation, a precision, low-offset OPAMP like SGM8511 is chosen, similar to the one used in the linear driver design.

## Switching Regulator
The circuit uses the integrated chip MT3410L, implementing a synchronous buck converter. It operates at a switching frequency of up to 1.5 MHz with a very low Rdson resistance of up to 200mΩ. The integrated chip has a 600 mV feedback circuit compared with the voltage measured from the current sensor. To avoid oscillation issues, a compensator is added with capacitors C3 and C5 in the current amplification circuit.

## Switch
In this flashlight, the switch controls the distance between a magnet and a hall effect sensor S49E. This design incorporates two options to activate and deactivate the flashlight using the ENABLE pin of the MT3410L controller. In the first option, an OPAMP is used to compare with the threshold voltage of the switch's middle position (1.78 V). In the second option, the voltage is compared with the PN junction voltage (0.6 V) of a BJT transistor using a voltage divider.

# PCB
A double-sided PCB has been designed to fit into the commercial flashlight casing and be easily replaceable. Consideration has been given to track currents and EMI control rules to ensure reliable current measurement with a small current measuring resistor and minimize PWM track surfaces to avoid disturbance emissions.

{:style="text-align:center;"}
<img src="/assets/images/2024/Switching_Led/Top.png" title="Top" width="300" height="auto">
<img src="/assets/images/2024/Switching_Led/Top_PCB.png" title="Top_pcb" width="300" height="auto">

{:style="text-align:center;"}
<img src="/assets/images/2024/Switching_Led/Bottom.png" title="Bottom" width="300" height="auto">
<img src="/assets/images/2024/Switching_Led/Bottom_PCB.png" title="Bottom_pcb" width="300" height="auto">

The assembled PCB looks as follows:

{:style="text-align:center;"}
![Assembly](/assets/images/2024/Switching_Led/Assembly.png "Assembly") 

The Github repository includes Gerber files for manufacturing generated for the PCB manufacturer JLCPCB, compatible with most PCB manufacturing houses.

# BOM
The list of required materials is as follows, including LCSC component codes.

{:style="text-align:center;"}
![BOM](/assets/images/2024/Switching_Led/BOM.png "BOM") 

At the time of writing this post, the component cost for manufacturing 10 units on the LCSC website is $23.13. Therefore, the component cost per unit would be approximately $2.313.

# Measurements
The following measurements have been made:

Initially, standby current is measured with the hall effect switch deactivated.

{:style="text-align:center;"}
![Stand_by](/assets/images/2024/Switching_Led/Fun_1.jpg "Stand_by") 

Similar to the linear driver case, the standby current consumed is essentially that of the hall effect sensor, totaling 4mA.

Then, the hall effect sensor is activated, and current can be measured with the potentiometer adjusted to 1A.

{:style="text-align:center;"}
![2A](/assets/images/2024/Switching_Led/Fun_2.jpg "1A") 

The consumed current should be approximately 1A · 3.7V / 3V · eff ≈ 0.77A, which matches the measured current.

The current measured from the voltage across the 10mΩ resistor with the oscilloscope is as follows:

{:style="text-align:center;"}
![Isense](/assets/images/2024/Switching_Led/Iout=1A.png "Isense")

Channel 1 measures the current supplied to the LED, taking into account the measurement error (approximately 2mV), the current is 1A with a ripple of 350mA. The current does not pass through 0, thus avoiding flickering. Channel 2 measures the feedback voltage, which is 600mV as indicated in the DC/DC regulator datasheet, but oscillations causing the measured ripple are also observed.

To improve this behavior, capacitors C3 and C5 have been adjusted through testing with various values, yielding the best result with C3=15pF and C5=1nF.

Zooming in with a time base of 500ns/div, the signal caused by the Mosfet switching can be observed.

{:style="text-align:center;"}
![Isense_1](/assets/images/2024/Switching_Led/Iout=1A_1.png "Isense_1") 

It can be observed that the feedback voltage signal (channel 2) varies abruptly during Mosfet switching, which is likely causing the oscillations. This raises suspicion that the current measurement op-amp is too slow. To amend this issue in the future, consideration will be given to using a faster op-amp such as the AD8605 Precision, Low Noise, CMOS, Rail-to-Rail, Input/Output Operational Amplifier.

# Efficiency and Operating Voltage Measurement
With the aim of obtaining the efficiency of the driver, measurements were taken by varying the input voltage between 2.9 V and 4.2 V, measuring the input and output voltage and current.

The output current as a function of the input current:

{:style="text-align:center;"}
![Intensidad](/assets/images/2024/Switching_Led/Iout.png "Intensidad")

The efficiency as a function of the input current:

{:style="text-align:center;"}
![Eficiencia](/assets/images/2024/Switching_Led/efficiency.png "Eficiencia")

It confirms that the flashlight is usable from 3 V to 4.2 V with a consistent efficiency of approximately 85%.

# Flashlight Beam Comparison
The comparison between the beams of a standard 1.5 A flashlight and a flashlight with the designed driver regulated to 1 A is as follows:

{:style="text-align:center;"}
![Haces](/assets/images/2024/Switching_Led/Haces.jpg "Haz")

It can be observed that the designed driver and the original driver illuminate similarly.

# Design Documentation and License
All design documentation, including the KICAD PCB project, manufacturing documents, schematic, and bill of materials, are available in the following Github repository:

[https://github.com/luicer/Switching-Flashlight-DV-S9-Driver](https://github.com/luicer/Switching-Flashlight-DV-S9-Driver)

This design is open source hardware under the permissive CERN OHL V2 license.