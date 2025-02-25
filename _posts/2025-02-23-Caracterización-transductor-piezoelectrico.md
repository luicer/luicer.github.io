---
title: "Diseño de un sistema Ecosonar - Medida de la respuesta en frecuencia de un transductor piezoeléctrico"
language: español

toc: true
categories: 
  - Ecosonda  

  
full-width: true
tags:
  - Piezoeléctrico
  - Transductor
  - Ultrasonidos
  - Medidas

excerpt: En este artículo se presenta un montaje para la caracterización transductores piezoeléctricos, obteniendo su respuesta en frecuencia a partir del controlador sonar diseñado. El sistema permitirá comprobar las frecuencias más adecuadas para el funcionamiento del transductor y la potencia ultrasónica que es capaz de trasmitir al agua. 

header:
    teaser: /assets/images/2025/Respuesta en frecuencia/Teaser.jpg
---

[English]({% post_url 2025-02-23-Caracterización-transductor-piezoelectrico-Eng %})


# Introducción
Un vez construidos varios transductores ultrasónicos surge la necesidad de analizar su comportamiento para asegurar que los cristales piezoeléctricos cumplen con las especificaciones declaradas.

Un primer análisis factible de realizar es medir su respuesta en frecuencia, para ello es necesario excitar el sensor a diferentes frecuencias y medir su impedancia equivalente. Esto nos permitirá comprobar las frecuencias más adecuadas para el funcionamiento del transductor y la potencia que puede trasmitir al agua.

En este post se analizarán dos transductores, uno construido a partir de un cristal piezoeléctrico de 4 mm de espesor con una frecuencia de resonancia teórica de 500 kHz y otro construido a partir de un cristal piezoeléctrico de 10 mm de espesor con una frecuencia de resonancia teórica de 325kHz :

Transductor con cristal de 20mm de diámetro y 4mm de espesor:

{:style="text-align:center;"}
![20mm](/assets/images/2025/Respuesta en frecuencia/Transductor_20mm.jpg "20mm") 

Transductor con cristal de 50mm de diámetro y 10mm de espesor:

{:style="text-align:center;"}
![20mm](/assets/images/2025/Respuesta en frecuencia/Transductor_50mm.jpg "50mm") 
 
# Esquema y funcionamiento.
El esquema del sistema utilizado para obtener el la respuesta en frecuencia de transductores sonar es el siguiente:

{:style="text-align:center;"}
![Esquema](/assets/images/2025/Respuesta en frecuencia/Esquema.jpg "Esquema") 

Se utiliza el controlador sonar diseñado como generador de señal para excitar al transductor, después se utiliza un osciloscopio comercial para medir la tensión aplicada al transductor, la intensidad del transductor se mide con el osciloscopio a partir de la la lectura de tensión en una resistencia en serie.

Tanto el controlador sonar como el osciloscopio se comunica con un ordenador para generar el barrido de frecuencias y poder calcular la impedancia.

# Montaje. 
El montaje para realizar las medidas es el siguiente:

{:style="text-align:center;"}
![Montaje](/assets/images/2025/Respuesta en frecuencia/Montaje.jpg "Montaje") 

Se ha programado una interfaz gráfica en Python con el framework KIVY para automatizar el proceso. Este software se comunica mediante bluetooth con la placa del controlador sonar para generar el barrido de frecuencias y por Visa con el osciloscopio para obtener los valores rms de tensión y corriente.

Las medidas se hacen con el transductor sumergido en un recipiente con agua.


# Resultados.
Los resultados de las medidas son los siguientes:

Transductor de 20mm de diámetro.

{:style="text-align:center;"}
![Figure 4_20mm](/assets/images/2025/Respuesta en frecuencia/Figure 4_20mm.png "Figure 4_20mm") 

Transductor de 50mm de diámetro.

{:style="text-align:center;"}
![Figure 5_50mm](/assets/images/2025/Respuesta en frecuencia/Figure 5_50mm.png "Figure 5_20mm") 

En ambos casos se observan dos picos principales uno a la frecuencia de resonancia de espesor y otro en la frecuencia de resonancia radial.

- En el caso del primer transductor la frecuencia de resonancia de espesor es 510 kHz y la frecuencia de resonancia radial es de 100kHz
- En el caso del segundo transductor a frecuencia de resonancia de espesor es 325 kHz y la frecuencia de resonancia radial es de 40kHz

Estos valores coinciden con las especificaciones del fabricante y los cálculos teóricos. Además se pueden observar otras frecuencias de resonancia que depende de la construcción del cristal piezoeléctrico.

Un resultado importante que se puede deducir de la medida, es la impedancia de resonancia de los transductores, que seria la resistencia de radiación del transductor en el agua. A partir de éste valor se puede calcular la potencia que el transductor trasmite al agua. 

La impedancia de resonancia es la impedancia a la frecuencia de resonancia y se puede aproximar como el mínimo local de la gráfica.

Por ejemplo:
- En el caso de excitar el sensor de 20 mm con una señal de 150 Vrms a 510 kHz la potencia trasmitida seria V<sup> 2 </sup>/Zres=150<sup> 2 </sup>/200 =112,5 Watts.
- En el caso de excitar el sensor de 50 mm con una señal de 150 Vrms a 325 kHz la potencia trasmitida seria V<sup> 2 </sup>/Zres=150<sup> 2 </sup>/50 =450 Watts

La resistencia de impedancia del transductor de 50 mm es mas grande que la del transductor de 20mm, por tanto se demuestra que el sensor más grande es capaz de enviar mas potencia ultrasónica y por tanto tendrá más alcance.

# Conclusiones y trabajos futuros.

Se ha desarrollado un sistema para obtener la respuesta en frecuencia de los transductores sonar. A partir de estos datos en el futuro se pueden obtener un modelo del transductor y calcular los parámetros de un circuito equivalente.


