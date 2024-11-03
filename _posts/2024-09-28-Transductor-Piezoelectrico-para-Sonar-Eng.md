---
title: "Design of an Echosonar system - Introduction"
language: english

toc: true
categories: 
  - Ecosonar  
 

  
full-width: true
tags:
  - Piezoelectric
  - Transducer
  - Ultrasound 
  - 3d Print

excerpt: This article introduces the design of a high-precision echosounder system that includes the design of a 3D-printed transducer, the design of the controller, as well as the software needed to visualize the data

header:
    teaser: /assets/images/2024/Intro_Sonda/Teaser.png
---

[Spanish]({% post_url 2024-09-28-Transductor-Piezoelectrico-para-Sonar %})

# Introduction
An ultrasonic echosounder is a device capable of detecting objects based on echoes produced by an ultrasonic pressure wave generator.

There are many commercial echosounders available on the market, used for applications like analyzing the ocean floor, fishfinders, and localization systems for ROVs. Here, we present a first version of an echosounder system as a proof of concept, which can serve as a foundation for developing a specific product in the future.

The system consists of the following parts:

1. The piezoelectric transducer, which is the component that converts an electrical signal into a pressure wave and vice versa.
2. The ultrasonic signal generator, which produces the signal needed by the transducer to generate the appropriate pressure waves.
3. The signal receiver, which detects signals produced by the echoes.
4. Interpretation software, which allows us to interpret the received signals

{:style="text-align:center;"}
![assembly](/assets/images/2024/Intro_Sonda/Img_1.jpg "img1") 
 
# Design of the Piezoelectric Transducer for the Sonar
The transducer is the main component of the echosounder system. In our case, a piezoelectric crystal is used, which both generates ultrasonic pressure waves and converts the echoes into an electrical signal.

The transducer being designed consists of a 3D-printed housing made of PETG filament, coated with epoxy resin. Inside are the elements that make up the transducer:

1. The Piezoelectric Crystal: In this design, the material chosen is PZT5, in a disc format with a thickness of 4mm and a diameter of 20mm.
2. The Matching Layer: This is the layer between the piezoelectric crystal and the water. It must have an acoustic impedance that is intermediate between the material of the piezoelectric crystal and the water; PETG has this characteristic. The thickness of this layer has been defined as 1/4 of the pressure wavelength.
3. The Backing Layer: This layer surrounds the crystal and must have a high degree of acoustic wave absorption to prevent ringing. In our case, cork has been selected.

The low-cost piezoelectric disc chosen has a thickness of 4mm, which gives it a resonance frequency in axial mode of 500 kHz. The specific disc used is as follows:
<!-- {:style="text-align:center;"}
<img src="/assets/images/2024/Intro_Sonda/Piezo.jpg" title="Piezo" width="300" height="auto"> -->

{:style="text-align:center;"}
![Piezo](/assets/images/2024/Intro_Sonda/Piezo.jpg "Piezo") 

The design was created using FreeCAD, and in this first version, it was chosen to make it detachable with a threaded connection that allows the two parts to be separated for internal analysis. An O-ring seal was used to ensure watertightness. The design is as follows:

{:style="text-align:center;"}
![Freecad](/assets/images/2024/Intro_Sonda/Freecad_2.png "Freecad") 

To improve watertightness and strength of the element, it has been post-processed by coating it with epoxy resin. The final result is as follows:

{:style="text-align:center;"}
![Trasnducer](/assets/images/2024/Intro_Sonda/Transducer.jpg "Transducer") 

In the final design, the interior will be completely encapsulated with epoxy resin to ensure complete watertightness.

# Controller Design
The designed controller has the following block diagram:

{:style="text-align:center;"}
![Block](/assets/images/2024/Intro_Sonda/Sonar.jpg "Block") 

The controller is based on a "black pill" board with the STM32F401 microcontroller and consists of three parts:

1. The Ultrasonic Generator: It uses the microcontroller's PWM module to generate pulses of 500 kHz. This signal, through power MOSFETs and a transformer to boost the signal, is injected into the piezoelectric transducer to generate the pressure wave. To achieve maximum precision, a 5-pulse train at a frequency of 500 kHz has been implemented as the generating signal.
2. The Ultrasonic Power Meter: A peak detector and an amplifier based on OPAMPS are used to transfer the signal received by the transducer to the microcontroller's ADC.
3. The Communication Interface: The commercial HC-06 module is used to send the information received via Bluetooth, allowing it to be transmitted to a computer or a smartphone.

A PCB has been designed with KICAD, and the final result looks as follows:

{:style="text-align:center;"}
![PCB](/assets/images/2024/Intro_Sonda/PCB.jpg "PCB") 

The firmware has been developed using the Arduino framework but utilizing the HAL libraries from ST-Microelectronics to leverage DMA and advanced timer functions for PWM and ADC.

For the initial tests, the transducer was used in a closed container, maintaining a distance of 10 cm from the bottom in order to analyze the waveform.       

{:style="text-align:center;"}
<!-- ![10cm"](/assets/images/2024/Intro_Sonda/10cm.jpg "10cm") -->
<img src="/assets/images/2024/Intro_Sonda//10cm.jpg" title="10cm" width="300" height="auto"> 

Using the oscilloscope, both the signal generated for the transducer (Channel 1) and the measured echo response (Channel 2) can be analyzed. 

{:style="text-align:center;"}
!["Osciloscope"](/assets/images/2024/Intro_Sonda/SN_1.4_2_e.png "Osciloscope")

We can observe that the generated signal is a train of 5 pulses at 170 V p-p, and that the echo takes 125 µs to return. The rest of the signal consists of reflections of the pressure waves in the container.

The time it takes for an echo to return is used to calculate the distance according to the following formula: ![F1](/assets/images/2024/Intro_Sonda/F1.png "F1")  where c is the speed of sound in water, and t is the time when an echo appears. In our case: ![F2](/assets/images/2024/Intro_Sonda/F2.png "F2")


# Control Software
A graphical interface has been programmed in Python using the KIVY framework to represent the measured signal. The classic interface of echosounders is utilized, displaying distance on the Y-axis and representing signal intensity with a color scale.

The presentation of the GUI is as follows:

{:style="text-align:center;"}
!["Inter3"](/assets/images/2024/Intro_Sonda/Interface3.png "Inter3")

The application communicates with the board via Bluetooth using the serial port profile and allows for a measurement to be taken every 1 second. Additionally, it is possible to configure the measurement distance and various display parameters.
It has been programmed with the KIVY framework, which allows the application to be compiled for Android, enabling the future development of a portable system to visualize data on a smartphone or tablet
 
# Test
To test the entire echosounder system, is used a plastic container, and the transducer is applied to the wall. We can see that the system can detect a screwdriver with a diameter of less than 1 cm at a distance of approximately 0.5 meters without any issues.   

{% include video id="1023944174" provider="vimeo" %}

We can also see that the system is capable of detecting signal reflections up to several meters away.

It is important to note that the crystal is small, the frequency is relatively high (500 kHz), and a limited number of pulses has been used; this allows for very precise detection. However, on the other hand, the high frequency increases the absorption losses of the ultrasonic waves in water, the small size of the transducer reduces its sensitivity, and using only 5 pulses limits the energy of the echoes. Therefore, the detection distance is relatively short.

# Conclusions and Future Work
This first version has been confirmed to be fully functional and would be useful for detecting small distances with great precision.

Moreover, this design paves the way for many improvements and variations that can be explored, such as:

- Studying different types of transducers for different needs. The diameter of the piezoelectric crystal could be increased to enhance sensitivity or the thickness could be varied to change the operating frequency.
- Varying the number of pulses. To increase the detection distance, the number of pulses could be increased, but this would reduce the precision of the measurements.
- Creating a system to determine the impedance of the transducers.
- Developing a compact transducer with the electronics integrated into the housing.
- Improving the sensitivity of the ultrasonic detector.
- Developing an Android app to make the echosounder portable.

In the future, efforts will be made to develop these improvements.