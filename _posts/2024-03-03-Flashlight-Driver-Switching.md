---
title: "Driver Conmutado para Linterna de buceo DV-S9 Led Open Source Hardware."
language: español


toc: true
categories: 
  - Linterna LED
  
full-width: true
tags:
  - Linterna
  - Driver Switching
  - OSHW
   

excerpt: "En este post se explica el diseño de un driver con regulador conmutado (switching) regulable para linterna de buceo tipo DV-S9 para una intensidad de hasta 1 A y un tensión de entrada entre 3V y 4,2V además de una reducida intensidad de vacío. El driver consigue una eficiencia de hasta 85% a intensidad máxima. Este diseño es open source hardware con los documentos diseño almacenados en Github."

header:
    teaser: /assets/images/2024/Switching_Led/Teaser.jpg
---
[English]({% post_url 2024-03-04-Flashlight-Driver-Switching-Eng %})

# Introducción.
Después del diseño de un driver lineal [Link]({% post_url 2024-02-25-Flashlight-Driver-Lineal %}), en este articulo se va ha mostrar un diseño de un driver conmutado para una linterna led comercial tipo Ultrafire DV-S9 diving light, de esta manera se conseguirá más eficiencia y se aprovechará mejor la energía de la batería.

{:style="text-align:center;"}
![Ultrafire DV-S9](/assets/images/2024/Switching_Led/Teaser.jpg "DV-S9") 

<!-- <img src="/assets/images/2024/DV-S9.jpg" alt="DV-S9" align=”middle”>  -->

El objetivo es diseñar un driver de bajo coste para sustituir el original que permita regular la corriente hasta  1 A pero que utilice un driver utilizando un convertidor DC/DC conmutado para su regulación. 

{:style="text-align:center;"}
<img src="/assets/images/2024/Lineal_Led/DriverDVS9.jpg" title="Diver Original" width="300" height="auto">
<img src="/assets/images/2024/Switching_Led/Driver_SW.png" title="Diseño" width="300" height="auto">
 
# Especificaciones Driver.
El diseño driver tiene las siguientes características:  
- Regulador de tipo conmutado.
- Intensidad regulable de hasta 1 A.
- Tensión de entrada de 3 V a 4,2 V.
- Intensidad en vacío de 4 mA.

# Diagrama de bloques.
El diagrama de bloques de diseño propuesto es el siguiente: 

{:style="text-align:center;"}
![Block](/assets/images/2024/Switching_Led/LED_switching.drawio.png "Block") 

El diseño del driver utiliza un regulador DC/DC conmutado de tipo reductor realimentado con un medidor de corriente que mide la intensidad suministrada al LED. Para encender o apagar el sistema se utiliza un pin de encendido del regulador DC/DC.  

# Esquema.
El esquema que he utilizado para implementar el diseño es el siguiente:

{:style="text-align:center;"}
![Schematic](/assets/images/2024/Switching_Led/Schematic.png "Sch") 

Este esquema se puede descargar aquí: [Esquema](/assets/images/2024/Switching_Led/Schematic.pdf)

## Alimentación.
Al igual que en el caso del driver lineal, el driver diseñará para una tensión de alimentación de entre 3V y 4,2 V. De esta manera los componentes se seleccionarán para un tensión máxima de 4,2 V y para poder poder alimentar de manera estable la parte de control se utiliza un LDO de 3V modelo HT3170. 

## Sensor de corriente.
Al igual que en el caso del driver lineal, como sensor de corriente se utiliza un resistencia de 10mΩ y un amplificador para elevar la tensión de medida. Para la implementación del amplificador se ha ha elegido un OPAMP de precisión y de bajo offset como el SGM8511, el mismo que el utilizado para el diseño del driver lineal.

## Regulador conmutado.
El circuito utiliza el chip integrado MT3410L que implementa un convertidos reductor (Buck) síncrono, funciona a una frecuencia de conmutación de hasta 1.5 MHz y tiene una resistencia Rdson muy baja de hasta 200mΩ. El chip integrado tiene un circuito de realimentación de 600 mV que se compara con la tensión medida a partir del sensor de corriente. Para evitar problemas de oscilaciones la medida de corriente, se ha añadido un compensador con los condensadores C3 y C5 en el circuito de amplificación de corriente.

## Interruptor.
En este tipo de linterna el interruptor controla la distancia entre un imán y un sensor de efecto hall S49E. En este diseño se han implementado dos opciones para poder activar y desactivar la linterna a partir del pin de ENABLE del controlador MT3410L. En una primara opción se utiliza un OPAMP para comparar con la tensión de umbral de posición intermedia del interruptor (1.78 V). En la segunda opción, la tensión se compara con la tensión de la unión PN (0,6 V) de un transistor BJT, mediante un divisor de tensión.

# PCB.
Se ha diseñado una PCB de dos caras que se ajusta al al carcasa de la linterna comercial y que sea fácilmente sustituible. Se ha tenido en cuenta las intensidades admisibles de las pistas y reglas de control de EMIs para que la medida de corriente con una resistencia pequeña  sea fiable. Además se ha  minimizado las superficies de las pistas de los Mosfets de conmutación para evitar la emisión de perturbaciones.  

{:style="text-align:center;"}
<img src="/assets/images/2024/Switching_Led/Top.png" title="Top" width="300" height="auto">
<img src="/assets/images/2024/Switching_Led/Top_PCB.png" title="Top_pcb" width="300" height="auto">

{:style="text-align:center;"}
<img src="/assets/images/2024/Switching_Led/Bottom.png" title="Bottom" width="300" height="auto">
<img src="/assets/images/2024/Switching_Led/Bottom_PCB.png" title="Bottom_pcb" width="300" height="auto">

La PCB montada queda de la siguiente manera:
{:style="text-align:center;"}
![Assembly](/assets/images/2024/Switching_Led/Assembly.png "Assembly") 

En el repositorio de github se incluyen los archivos Gerber de fabricación generados para le fabricante de PCB JLCPCB, pero que son compatible con al mayoría de casas de fabricación de PCBs. 

# BOM.
El listado de materiales necesarios es el siguiente, se ha incluido el código LCSC de los componentes.

{:style="text-align:center;"}
![BOM](/assets/images/2024/Switching_Led/BOM.png "BOM") 

En el momento de la realización del post el precio de los componentes para la fabricación de 10 unidades en la pagina web de LCSC es de 23.13 $ por tanto el coste de componentes por unidad sería de unos 2,313$/u. 

# Medidas Realizadas.
Se han hecho las siguientes mediciones:

Primeramente se mide la intensidad en vacío, con el interruptor de efecto hall desactivado.

{:style="text-align:center;"}
![Vacío](/assets/images/2024/Switching_Led/Fun_1.jpg "Vacío") 


Al igual que en el caso del driver lineal, intensidad consumida en vacío es básicamente las del sensor efecto hall dando un total de 4mA.

Después se activa el sensor de efecto hall y se pude medir la corriente con el potenciómetro ajustado a 1 A.

{:style="text-align:center;"}
![2A](/assets/images/2024/Switching_Led/Fun_2.jpg "1A") 

La intensidad consumida debería ser aproximadamente 1A · 3,7V / 3V · eff ≈ 0.77 A que se ajusta con la intensidad medida.

La intensidad medida a partir de la tensión en la resistencia de 10mΩ con el osciloscopio es la siguiente:

{:style="text-align:center;"}
![Isense](/assets/images/2024/Switching_Led/Iout=1A.png "Isense")

En el canal 1 se mide la intensidad suministrada al led midiendo la tensión sobre la resistencia de 10mΩ teniendo en cuenta el error de medición (2mV aprox) la intensidad es de 1A con un rizado de 350mA, la intensidad no pasa por 0 por tanto no provoca flicker. En el canal 2 se mide la tensión de realimentación que es de 600mV, tal como se indica en datasheet del regulador DC/DC, pero también se ve que hay oscilaciones que provocan el rizado medido.

Para mejorar este comportamiento y reducir el rizado se ha ajustado los condensadores C3 y C5, haciendo pruebas con varios valores ha dado el mejor resultado con C3=15pF y C5=1nF.

Si hacemos un zoom con una base de tiempo de 500ns/div podemos ver la señal que se provoca en la conmutación del Mosfet.

 {:style="text-align:center;"}
![Isense_1](/assets/images/2024/Switching_Led/Iout=1A_1.png "Isense_1") 

Se puede ver que la señal del realimentación de tensión (canal 2) varia de forma brusca en las conmutaciones de Mosfet, esto es lo que provoca probablemente las oscilaciones. Esto hace sospechar que el opamp de medida de corriente es demasiado lento, para poder analizar este problema en el futuro se estudiará utilizar un opamp más rápido como el AD8605 Precision, Low Noise, CMOS, Rail-to-Rail, Input/Output Operational Amplifier.

# Medida de eficiencia y tensión de funcionamiento.
Con el objetivo de poder obtener la eficiencia del driver se han hecho medidas variando la tension de entrada entre 2,9 V y 4,2 V, midiendo la tensión e intensidad de entrada y la tensión e  intensidad de salida.

La intensidad de salida en función de la intensidad de entrada:

{:style="text-align:center;"}
![Intensidad](/assets/images/2024/Switching_Led/Iout.png "Intensidad")

La intensidad es variable con la tensión de entrada.

La eficiencia en función de la tensión de entrada:

{:style="text-align:center;"}
![Eficiencia](/assets/images/2024/Switching_Led/efficiency.png "Eficiencia")

Se comprueba que la linterna es utilizable de 3 V hasta 4,2 V con una eficiencia constante de aproximadamente 85%.


# Comparación de haces de linternas.
La comparación entre los dos haces de una linterna estándar de 1,5 A. y una linterna con el driver diseñado regulado a 1 A es el siguiente:

{:style="text-align:center;"}
![Haces](/assets/images/2024/Switching_Led/Haces.jpg "Haz")

Se observa que el driver diseñado y el driver original alumbran de manera parecida.

# Documentación de diseño y Licencia.
Toda la documentación del diseño incluido el proyecto en KICAD de la PCB, los documentos de fabricación, el esquema y el listado de material se encuentra en le siguiente repositorio de Github:

[https://github.com/luicer/Linear-Flashlight-DV-S9-Driver](https://github.com/luicer/Switching-Flashlight-DV-S9-Driver)


Este diseño es open source hardware según la licencia CERN OHL V2 permisiva.
