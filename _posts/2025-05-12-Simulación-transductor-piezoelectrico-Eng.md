---
title: "Echosounder System Design - Modeling and Simulation of a Piezoelectric Transducer"
language: english

toc: true
categories: 
  - Echosounder  

full-width: true
tags:
  - Piezoelectric
  - Transducer
  - Ultrasound
  - Simulation
  - LTspice

excerpt: This post presents the implementation of a SPICE-based model for an ultrasonic transducer. The model is validated by comparing simulation results with empirical measurements from a custom-built transducer, as described in the previous post. Based on these results, the TVR of the transducer is estimated.

header:
    teaser: /assets/images/2025/Simulacion/Teaser.png
---

[Español]({% post_url 2025-05-12-Simulación-transductor-piezoelectrico %})

## Introduction

After developing a measurement system for the frequency response of piezoelectric transducers, a numerical model was implemented in LTspice for a 20 mm diameter transducer fabricated with a PZT4 ceramic. This model was calibrated using experimental data. Although some physical parameters were estimated due to lack of manufacturer data, the objective is to obtain a representative model that captures the main electromechanical behavior of the device, rather than an exact reproduction.

{:style="text-align:center;"}
![20mm](/assets/images/2025/Respuesta en frecuencia/Transductor_20mm.jpg "20mm")

The model is based on the adaptation by Kubov V.I. of the multilayer piezoelectric transducer model developed by J. Deventer, as described in "PSpice Simulation of Ultrasonic Systems" (2000, Jan van Deventer, Torbjorn Lofqvist). The adaptation is available at [Ultrasonic_Model](https://ltwiki.org/files/LTspiceIV/examples/PiezoAcoustic/).

Material properties for PZT4 were sourced from the manufacturer's datasheet. Where data was unavailable, values were estimated to fit the measured response.

LTspice, a widely adopted and freely available circuit simulation tool, was used for all simulations.

## Transducer Model

The transducer model, operating in thickness mode, was implemented in LTspice as follows:

{:style="text-align:center;"}
![Model](/assets/images/2025/Simulacion/Modelo_spiceAC_1.png "Model")

The model includes the following components:

- A PZT4 piezoelectric disk, 20 mm in diameter and 4 mm thick, with a thickness mode resonance at 500 kHz.
- A cork back layer.
- A PETG matching layer (the housing material), with a thickness of one quarter wavelength at the operating frequency.
- The acoustic load of water, represented by the radiation impedance at the transducer interface.

The density and sound velocity for each material were approximated from published literature.

## Simulation Results

The simulated electrical impedance of the transducer, defined as the ratio of input voltage to current, is shown below:

{:style="text-align:center;"}
![Impedance](/assets/images/2025/Simulacion/Z_cork.png "Impedance")

The output acoustic power delivered to water, for a 100 Vrms sinusoidal excitation, is depicted below. At 500 kHz, the radiated power is approximately 5 W (6.9 dBw):

{:style="text-align:center;"}
![Power](/assets/images/2025/Simulacion/Power_Cork.png "Power")

The simulation predicts a resonance frequency near 500 kHz, consistent with the design specifications.

## Experimental Validation

A comparison between simulated and measured impedance is presented below ([see characterization]({% post_url 2025-02-23-Caracterización-transductor-piezoelectrico-Eng %})):

{:style="text-align:center;"}
![Comparison](/assets/images/2025/Simulacion/Comp_Z.png "Comparison")

The simulated and experimental results show good agreement after parameter adjustment. Note that the model only considers the thickness vibration mode; in practice, additional modes contribute to the measured impedance spectrum, accounting for some discrepancies.

It is also important to note that the physical parameters of piezoelectric ceramics can vary significantly due to manufacturing tolerances, limiting the accuracy of the model.

## TVR Estimation

The Transmit Voltage Response (TVR) is a key parameter, defined as the sound pressure level (SPL) at 1 meter from the transducer per 1 V input referred to a  $$ 1 \mu Pa $$ , as a function of frequency. TVR is typically measured using calibrated hydrophones; in this study, it is estimated from the validated simulation model.

The TVR is calculated as:

$$ TVR = 10 \cdot \log_{10}\left(\frac{P_{Zl}}{V_{in}^2}\right) + DI + 170.08 $$

where $$ P_{Zl} $$ is the radiated acoustic power, $$ V_{in} $$ is the input voltage, and $$  DI $$ is the directivity index. For a 20 mm disk at 500 kHz, $$ DI \approx 26 dB $$.

The simulated TVR as a function of frequency is shown below:

{:style="text-align:center;"}
![TVR](/assets/images/2025/Simulacion/TVR_AC.png "TVR")

## Backlayer Material Study

To evaluate the influence of the backlayer, simulations were performed using three materials:

- Air (very low acoustic impedance)
- Cork (low acoustic impedance)
- Epoxy resin (medium acoustic impedance)

The model configuration is illustrated below:

{:style="text-align:center;"}
![Comp Spice](/assets/images/2025/Simulacion/Modelo_SpiceAC_1_Comp.png "Comp Spice")

The simulated radiated power for each backlayer material is shown:

{:style="text-align:center;"}
![Power_Comp](/assets/images/2025/Simulacion/Power_Cork_Comp.png "Power_Comp")

The corresponding TVR calculations are presented below:

{:style="text-align:center;"}
![TVR_Comp](/assets/images/2025/Simulacion/TVR_Comp.png "TVR_Comp")

Results indicate that lower backlayer acoustic impedance increases radiated power and improves TVR.

## Conclusions and Future Work

A representative LTspice model of a piezoelectric transducer has been developed and validated against experimental data. The model enables estimation of TVR and analysis of design parameters, such as backlayer material selection. For further refinement, more precise characterization of material properties is recommended, as well as inclusion of additional vibration modes for improved accuracy.

The simulation files can be downloaded in my github page. [Link](https://github.com/luicer/Echosounder/tree/main/Simulation/LTspice/).
