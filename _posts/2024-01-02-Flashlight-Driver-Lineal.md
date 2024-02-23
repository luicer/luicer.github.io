---
title: "Driver Lineal Linterna Led Open Hardware"



toc: true
categories:
  - Flashlight
full-width: true
tags:
  - LED
  - Flashlight
  - Linear

  

excerpt: "En este post se explica el diseño de un driver con regulador lineal regulable para linterna de buceo tipo DV-S9 para una intensidad de hasta 3 A y un tensión de entrada entre 3V y 4,2V además de una reducida intensidad de vacío. El driver consigue una eficiencia de entre 90 % y 70 % a intensidad máxima. Este diseño es open hardware con los documentos diseño almacenados en Github"

header:

    #image: /assets/images/Sensor1.jpg
    teaser: /assets/images/2024/Lineal_Led/teaser.jpg
---

# Introducción

En este articulo se va ha mostrar un diseño de un driver para una linterna led comercial tipo Ultrafire DV-S9 diving light, en internet se pueden encontrar muchos clones a precio muy ajustado. Es una linterna muy utilizada en pesca submarina, es funcional pero presenta una serie de problemas que se explicaran más adelante.

{:style="text-align:center;"}
![Ultrafire DV-S9](/assets/images/2024/Lineal_Led/teaser.jpg "DV-S9") 

<!-- <img src="/assets/images/2024/DV-S9.jpg" alt="DV-S9" align=”middle”>  -->

Esta linterna se alimenta con una batería de ión de litio 18650 o 2320 y utiliza un led tipo cree cree tipo XM-L2. A partir de las características eléctricas del XM-L2 se puede ver que para intensidades entre 1,5 A y 3 A se necesitan tensiones entre 3 V i 3.4 V este voltage se debe suministrar a partir de las baterías pero el voltage que proporcionan la baterías de ion de litio está entre entre 2,5 V y 4,2 V. Estas linternas utilizan un driver para mantener la corriente constante, normalmente se utiliza un diver reductor que permite disminuir la tensión de la batería hasta el valor necesario para suministrar la corriente necesaria, al ser un driver reductor hay un pequeño porcentaje de la energía de la batería que no se puede utilizar. Estos driver pueden ser del tipo lineal, más sencillos o del tipo conmutado que requiere un circuito más complejo. 

{:style="text-align:center;"}
![XML2_EC](/assets/images/2024/Lineal_Led/XML2_EC.png "Características eléctricas XML2") 

La linterna de serie consta de un driver lineal a partir del integrado AMC7136 que controla tres transistores sot23 y consigue el control de corriente a traves de un microcontrolador tipo Attiny13 activando y desactivando el regulador a traves de una señal PWM. Según mis medidas el driver puede dar una intensidad máxima de 1,5 A y en vacío tiene un consumo de 10 mA. Para activar la linterna utiliza un switch de efecto hall. El consumo en vacío debido al switch de efecto hall y al microcontrolador es relativamente alto y provoca que la batería se descargue si no se desconecta cuando no se utiliza. Además en determinadas circunstancias se puede provocar electrolisis en la carcasa de aluminio. 

{:style="text-align:center;"}
![Driver DV-S9](/assets/images/2024/Lineal_Led/DriverDVS9.jpg "Driver") 

El objetivo es diseñar un driver de bajo coste para sustituir el original que permita regular la corriente entre 1 A y 3 A y que no necesite un microcontrolador para disminuir la corriente de vacío.   

# Especificaciones Driver.
El diseño driver tiene las siguientes características:  
- Regulador de tipo lineal
- Intensidad regulable de hasta 3 A.
- Tensión de entrada de 3 V a 4,2 V.
- Intensidad en vacío de 4 mA

# Diagrama de bloques.
El diagrama de bloques de diseño propuesto es el siguiente: 

{:style="text-align:center;"}
![Block](/assets/images/2024/Lineal_Led/Sch_LED_Lineal.jpg "Block") 

Básicamente el diseño del driver utiliza un regulador lineal implementado con un amplificador operacional y un transistor de efecto campo MOSFET para poder suministrar la intensidad al LED requerida. El amplificador operacional se realimenta con un medidor de corriente para poder controlar la intensidad de salida en el LED y se controla a partir de un sensor de efecto Hall.

# Esquema.
El esquema que he utilizado para implementar el diseño es el siguiente:

{:style="text-align:center;"}
![Schematic](/assets/images/2024/Lineal_Led/schematic.png "Sch") 

Este esquema se puede descargara aquí: [Esquema](/assets/images/2024/Lineal_Led/Regulador_Lineal_Sch.pdf)

## Alimentación
Para alimentar el driver se utiliza una batería tipo LiON 18650 que puede suministrar entre 2,5 V y 4,2 V y el LED XM-L necesita tensiones entre 3V y 3.4 V por tanto se diseñará unas tension de alimentación entre 3V y 4.2V.De esta manera los componentes se seleccionarán para un tensión máxima de 4.2 V y para poder poder alimentar de manera estable la parte de control se utiliza un LDO de 3V modelo HT3170. 

## Sensor de corriente
Como sensor de corriente se utiliza un resistencia de 10mOHms y un amplificador para elevar la tensión de medida. Se ha utilizado un resistencia pequeña para que su caída de tensión no limite la tensión de alimentación del led, pudiendo alimentar la linterna con una tensión de sólo 3V. Para la implementación del amplificador se ha ha elegido un OPAMP de precisión y de bajo offset como el SGM8511.


## Regulador lineal
El circuito utiliza un típico regulador lineal con un MOSFET y un OPAMP realimentado con el sensor de corriente para poder controlar la intensidad que pasa por el led en función de la tensión de entrada. Para la implementación del regulador se ha elegido el mismo OPAMP que el utilizado para el sensor de corriente, el MOSFET IRLR2905 que con una Ron muy baja, para poder utilizar la linterna hasta un tensión de entrada de 3V.  

## Interruptor
El interruptor controla la distancia entre un imán y un sensor de efecto hall S49E. Para activar la linterna se compara la salida del sensor de efecto hall con el valor de tensión medido de cuando el interruptor está en posición intermedia del recorrido, este valor es de aprox 1.72V. Para implementar el comparador se utiliza un OPAMP del mismo modelo que el utilizado en el resto del driver y un transistor para eliminar la consigna del regulador lineal.

# PCB.
Se ha diseñado una PCB de dos caras que se ajusta al al carcasa de la linterna comercial y que sea fácilmente sustituible. Se ha tenido en cuenta las intensidades admisibles de las pistas y reglas de control de EMIs para que la medida de corriente con una resistencia pequeña de medida de corriente sea fiable.  

{:style="text-align:center;"}
<img src="/assets/images/2024/Lineal_Led/Top.png" title="Top" width="300" height="auto">
<img src="/assets/images/2024/Lineal_Led/Top_pcb.png" title="Top_pcb" width="300" height="auto">

{:style="text-align:center;"}
<img src="/assets/images/2024/Lineal_Led/Bottom.png" title="Bottom" width="300" height="auto">
<img src="/assets/images/2024/Lineal_Led/Bottom_pcb.png" title="Bottom_pcb" width="300" height="auto">

La PCB montada queda de la siguiente manera:

![Assembly](/assets/images/2024/Lineal_Led/build.jpg "Assembly") 


# Medidas Realizadas.
Se han hecho las siguientes mediciones:

Primeramente se mide la intensidad en vacío, con el interruptor de efecto hall desactivado.

{:style="text-align:center;"}
![Vacío](/assets/images/2024/Lineal_Led/Fun_1.png "Vacío") 


Se comprueba que la intensidad consumida en vacío es básicamente las del sensor efecto hall dando un total de 4mA.

Después se activa el sensor de efecto hall y se pude medir la corriente con el potenciómetro ajustado a 2 A.

{:style="text-align:center;"}
![2A](/assets/images/2024/Lineal_Led/Fun_2.png "2A") 


La intensidad medida a partir de la tensión en la resistencia de 10 miliohms con el osciloscopio es la siguiente:

{:style="text-align:center;"}
![Isense](/assets/images/2024/Lineal_Led/Isense_1.png "Isense")

Se comprueba que la intensidad es bastante estable.


# Medida de eficiencia y tensión de funcionamiento.

Con el objetivo de poder obtener la eficiencia del driver se han hecho medidas variando la tension de entrada entre 2,9 V y 4,2 V, midiendo la tensión e intensidad de entrada y la tensión e  intensidad de salida.

La intensidad de salida en función de la intensidad de entrada:

{:style="text-align:center;"}
![Intensidad](/assets/images/2024/Lineal_Led/Intensidad.jpg "Intensidad")

La eficiencia en función de la intensidad de entrada:

{:style="text-align:center;"}
![Eficiencia](/assets/images/2024/Lineal_Led/Eficiencia.jpg "Eficiencia")

Se comprueba que la linterna es utilizable de 3 V con una intensidad de 1 A y una eficiencia del 98% hasta 4,2 V con una eficiencia minima del 70%.


# Comparación de haces de linternas.

La comparación entre los dos haces de una linterna estándar de 1,5 A. y una linterna con el driver diseñado regulado a 2 A es el siguiente:

{:style="text-align:center;"}
![Haces](/assets/images/2024/Lineal_Led/Haces.jpg "Haz")

Se observa que el driver propuesto alumbra un poco más que el original.

# Licencia

Este diseño es open hardware según la licencia CERN OHL V2 permisiva.
