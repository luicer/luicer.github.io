---
title: "Diseño de un sistema Ecosonar - Modelado y simulación de un transductor piezoeléctrico"
language: español

toc: true
categories: 
  - Ecosonda  

full-width: true
tags:
  - Piezoeléctrico
  - Transductor
  - Ultrasonidos
  - Simulación
  - LTspice


excerpt: En esta entrada se implementará un modelo en SPICE de un transductor ultrasónico. Para validar este modelo, se comprobará que los resultados de la simulación se ajustan a las medidas realizadas sobre el transductor diseñado y construido, descritas en el post anterior. A partir de estos datos se estimará la TVR del transductor.

header:
    teaser: /assets/images/2025/Simulacion/Teaser.png
---

[English]({% post_url 2025-05-12-Simulación-transductor-piezoelectrico-Eng %})

## Introducción

Una vez desarrollado un sistema para poder medir la respuesta en frecuencia de los transductores, se ha implementado un modelo en LTspice del transductor de 20 mm de diámetro, construido en base a un cristal PZT4. De esta manera, podemos ajustar el modelo con los resultados de la medición. Aunque se utilizarán parámetros físicos aproximados, el objetivo del trabajo no es un modelado exacto del transductor, sino disponer de un modelo que, a grandes rasgos, describa su comportamiento.

{:style="text-align:center;"}
![20mm](/assets/images/2025/Respuesta en frecuencia/Transductor_20mm.jpg "20mm")

El modelo implementado está basado en la adaptación de Kubov V.I. del modelo de un transductor piezoeléctrico multicapa desarrollado por J. Deventer en "PSpice Simulation of Ultrasonic Systems. (2000) Jan van Deventer, Torbjorn Lofqvist". Esta adaptación se puede descargar en [Ultrasonic_Model](https://ltwiki.org/files/LTspiceIV/examples/PiezoAcoustic/).

Los valores de las propiedades físicas del material piezoeléctrico PZT4 se han obtenido a partir del datasheet del fabricante. Al carecer de algunos valores, se han estimado para que la respuesta se ajuste a las medidas realizadas.

Se ha utilizado el programa LTspice, que es gratuito y ampliamente probado en la industria electrónica.

## Modelo del transductor

El modelo de un transductor trabajando en modo espesor, implementado en LTspice, es el siguiente:

{:style="text-align:center;"}
![Modelo](/assets/images/2025/Simulacion/Modelo_SpiceAC_1.png "Modelo")
![Modelo](/assets/images/2025/Simulacion/Modelo_SpiceAC_1.png "Modelo")

El modelo consta de los siguientes elementos:

- Un cristal piezoeléctrico de material PZT4 de 20 mm de diámetro y 4 mm de espesor, con una frecuencia de resonancia en modo espesor de 500 kHz.
- Una capa "back layer" de corcho.
- Una capa "matching layer" de PETG (material del cual está construida la carcasa) de un cuarto de la longitud de onda de espesor.
- El agua, el medio donde emite el transductor, está modelada por la impedancia de radiación del transductor.

Los valores de la densidad y de la velocidad de sonido en los diferentes materiales se han aproximado a valores obtenidos de diferentes estudios publicados.

## Resultados de la simulación

La impedancia del transductor, medida como la división entre la tensión de entrada de la fuente y su intensidad, tiene el siguiente comportamiento:

{:style="text-align:center;"}
![Impedance](/assets/images/2025/Simulacion/Z_cork.png "Impedance")

La potencia transmitida por el transductor al agua, aplicando un voltaje de entrada senoidal de 100 voltios de Vrms, es la siguiente. La potencia para la frecuencia de 500 kHz sería de aproximadamente 5 W, que equivalen a 6.9 dBw:

{:style="text-align:center;"}
![Power](/assets/images/2025/Simulacion/Power_Cork.png "Power")

Los resultados de la simulación muestran una respuesta acorde a lo esperado, evidenciando una frecuencia de resonancia principal en torno a 500 kHz.

## Comparación de medidas

A continuación se presenta la comparación entre la impedancia medida experimentalmente durante la caracterización del transductor [link]({% post_url 2025-02-23-Caracterización-transductor-piezoelectrico-Eng %}) y la obtenida mediante simulación.

{:style="text-align:center;"}
![Comparasion](/assets/images/2025/Simulacion/Comp_Z.png "Comparasion")

Se observa una buena correlación entre los valores simulados y experimentales tras el ajuste de parámetros. Es importante señalar que el modelo implementado considera únicamente el modo de vibración fundamental en espesor; en la práctica, la presencia de modos superiores y modos espurios introduce resonancias adicionales en la impedancia de entrada, lo que explica las discrepancias residuales.

Adicionalmente, la variabilidad inherente en las propiedades físicas de los materiales piezoeléctricos, debida a tolerancias de fabricación y heterogeneidad del material, limita la precisión predictiva del modelo.

## Estimación del TVR

Para la caracterización electroacústica del transductor se emplea el parámetro TVR ("Transmit Voltage Response"), definido como el nivel de presión sonora (SPL) generado a 1 metro de distancia por 1 V de tensión de entrada, referido a $$1~\mu Pa$$, en función de la frecuencia. Si bien el TVR se determina habitualmente mediante hidrófonos calibrados, en este trabajo se estima a partir del modelo validado.

El TVR se calcula mediante la expresión:

$$
TVR = 10 \cdot \log_{10}\left(\frac{Power_{Zl}}{V_{in}^2}\right) + DI + 170.08
$$

donde $$Power_{Zl}$$ es la potencia acústica radiada, $$V_{in}$$ la tensión de entrada y $$DI$$ el índice de directividad. Para un disco de 20 mm de diámetro operando a 500 kHz, $$DI \approx 26~dB$$.

La evolución simulada del TVR en función de la frecuencia se muestra a continuación:

{:style="text-align:center;"}
![TVR](/assets/images/2025/Simulacion/TVR_AC.png "TVR")

## Estudio de varios materiales para la capa "Backlayer"

Para poder estudiar la capa "backlayer", se ha simulado el comportamiento de tres materiales:

- Aire, de muy baja impedancia acústica.
- Corcho, baja impedancia acústica.
- Resina epoxi, impedancia acústica media.

El modelo empleado para el análisis comparativo es el siguiente:

{:style="text-align:center;"}
![Comp Spice](/assets/images/2025/Simulacion/Modelo_SpiceAC_1_Comp.png "Comp Spice")

A continuación se muestra la potencia acústica radiada al agua para los diferentes materiales seleccionados en la capa "backlayer":

{:style="text-align:center;"}
![Power_Comp](/assets/images/2025/Simulacion/Power_Cork_Comp.png "Power_Comp")

El cálculo correspondiente del TVR para cada caso es el siguiente:

{:style="text-align:center;"}
![TVR_Comp](/assets/images/2025/Simulacion/TVR_Comp.png "TVR_Comp")

Se observa que, a menor impedancia acústica de la capa backlayer, se incrementa la potencia radiada al medio y se optimiza el TVR, lo que evidencia la importancia de la selección de materiales en el diseño acústico del transductor.

## Conclusiones y trabajos futuros

Se ha desarrollado un modelo numérico aproximado de un transductor piezoeléctrico, validado frente a resultados experimentales. Este modelo ha permitido estimar el TVR y analizar la influencia de distintos materiales en la capa backlayer. En el futuro, este enfoque podrá emplearse para optimizar el diseño de transductores y predecir su comportamiento bajo diferentes configuraciones.

Para mejorar la precisión del modelo, se recomienda una caracterización más exhaustiva de las propiedades físicas de los materiales empleados, así como la inclusión de modos de vibración adicionales y la consideración de efectos no lineales o disipativos presentes en aplicaciones reales.

Los archivos de simulación se pueden descargar en mi página de Github. [Link](https://github.com/luicer/Echosounder/tree/main/Simulation/LTspice/).
