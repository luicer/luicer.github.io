---
title: "Echosounder System Design - Sea Trial of the System"
language: english

toc: true
categories: 
  - Echosounder  

full-width: true
tags:
  - Piezoelectric
  - Transducer
  - Ultrasound
  - Sea Test
  - Kayak

excerpt: This post presents the real sea trial of a low-cost echosounder system, including the design of the waterproof enclosure, the visualization app, and the results obtained under real conditions.

header:
    teaser: /assets/images/2025/Prueba_Mar/Teaser.png
---

[Español]({% post_url 2025-06-23-Prueba-en-mar-transductor-piezoelectrico %})

## Introduction

Once the complete system was developed, it was time to test it at sea. The goal was to take the transducer and control board on a kayak and check its performance at different depths. For this, it was necessary to design a waterproof enclosure to protect the electronics and develop a mobile app that could communicate with the control board and display the data in real time.

For this first test, a transducer with a 20 mm diameter and 4 mm thick piezoelectric crystal operating at 500 kHz was used. Although it is a small sensor working at a relatively high frequency, it was expected to reach depths of up to 15 meters.

In the future, a larger transducer is planned to improve the system's range and sensitivity.

## Waterproof Enclosure for the System

The waterproof enclosure designed is shown below:

{:style="text-align:center;"}
![Sistema2](/assets/images/2025/Prueba_Mar/Sistema_Completo1.jpg "Sistema2")

The enclosure is completely waterproof and features an IP68 switch to activate the system. It also has a holder to secure the mobile phone.

Inside, the electronics are arranged as follows:

{:style="text-align:center;"}
![Sistema1](/assets/images/2025/Prueba_Mar/Sistema_Completo.jpg "Sistema1")

The control board is powered at 5 VDC by a DC/DC converter that steps down the voltage from two 18650 batteries connected in series.

## Kivy App

An Android app was developed using the Kivy framework. The application communicates with the board's HC-06 module via Bluetooth, using the Pyjnius library and the serial port profile.

The app has two main screens. On the first, the Bluetooth connection is established and measurement parameters are entered, such as:

- Ultrasound signal frequency
- Number of pulses
- Display scale
- Measurement interval

{:style="text-align:center;"}
![App2](/assets/images/2025/Prueba_Mar/App2.jpg "App2")

The second screen displays the measurement of ultrasound intensity as a function of depth, similar to a conventional fish finder.

{:style="text-align:center;"}
![App1](/assets/images/2025/Prueba_Mar/App1.jpg "App1")

## Sea Trial

For the sea tests, the entire system was transported on a kayak and its operation was checked at different depths.

{:style="text-align:center;"}
![Prueba3](/assets/images/2025/Prueba_Mar/Prueba3.png "Prueba3")

The transducer was manually submerged in the water:

{:style="text-align:center;"}
![Prueba2](/assets/images/2025/Prueba_Mar/Prueba2.png "Prueba2")

The measurement results were as follows:

{:style="text-align:center;"}
![Prueba1](/assets/images/2025/Prueba_Mar/Prueba1.png "Prueba1")

As you can see, the system is able to display the seabed at depths between 0 and 15 meters, since the bottom line appears in the correct position. However, it was observed that the depth detection algorithm does not work optimally in this range, as it is tuned for shorter distances.

## Video

Here you can watch a complete video showing the system in operation:

{% include video id="1098302244" provider="vimeo" %}

## Conclusions and Next Steps

The sea trial was successful, demonstrating that the system can measure depth up to about 15 meters using a small, low-cost transducer.

Looking ahead, the next steps will be:

- Improve the depth calculation algorithm, as it is currently optimized for short distances and high sampling rates.
- Design a larger transducer to increase sensitivity and radiated acoustic power. The current electronics, thanks to the use of a transformer and high-current MOSFETs, allow the use of more powerful transducers without issues.

We will continue to share updates and improvements