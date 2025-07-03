---
title: "Diseño de un sistema Ecosonar - Prueba en mar del sistema"
language: español

toc: true
categories: 
  - Ecosonda  

full-width: true
tags:
  - Piezoeléctrico
  - Transductor
  - Ultrasonidos
  - Prueba en Mar
  - Kayak


excerpt: En esta entrada se muestra la prueba en mar real de un sistema ecosonar de bajo coste, incluyendo el diseño de la carcasa estanca, la app de visualización y los resultados obtenidos en condiciones reales.

header:
    teaser: /assets/images/2025/Prueba_Mar/Teaser.png
---

[English]({% post_url 2025-06-23-Prueba-en-mar-transductor-piezoelectrico-Eng %})

## Introducción

Una vez desarrollado el sistema completo, era el momento de probarlo en el mar. El objetivo era llevar el conjunto transductor más placa de control en un kayak y comprobar su funcionamiento a distintas profundidades. Para ello, fue necesario diseñar una carcasa estanca para proteger la electrónica y desarrollar una app móvil que se comunicara con la placa de control y mostrara los datos en tiempo real.

En esta primera prueba se utilizó un transductor con un cristal piezoeléctrico de 20 mm de diámetro y 4 mm de espesor, trabajando a 500 kHz. Aunque es un sensor pequeño y opera a una frecuencia relativamente alta, se esperaba alcanzar profundidades de hasta 15 metros.

En el futuro, se planea diseñar un transductor de mayor tamaño para mejorar el alcance y la sensibilidad del sistema.

## Envolvente estanca para el sistema

La carcasa estanca diseñada es la siguiente:

{:style="text-align:center;"}
![Sistema2](/assets/images/2025/Prueba_Mar/Sistema_Completo1.jpg "Sistema2")

La carcasa es completamente estanca y cuenta con un interruptor IP68 para activar el sistema. Además, dispone de un soporte para fijar el móvil.

En el interior se aloja la electrónica de la siguiente manera:

{:style="text-align:center;"}
![Sistema1](/assets/images/2025/Prueba_Mar/Sistema_Completo.jpg "Sistema1")

La placa de control se alimenta a 5 VDC mediante un convertidor DC/DC que reduce la tensión de dos baterías 18650 conectadas en serie.

## App en Kivy

Se ha desarrollado una app para Android utilizando el framework Kivy. La aplicación se comunica con el módulo HC-06 de la placa mediante Bluetooth, usando la librería Pyjnius y el perfil de puerto serie.

La app tiene dos pantallas principales. En la primera, se realiza la conexión Bluetooth y se introducen los parámetros de medida, como:

- Frecuencia de la señal de ultrasonidos
- Número de pulsos
- Escala de representación
- Intervalo de medición

{:style="text-align:center;"}
![App2](/assets/images/2025/Prueba_Mar/App2.jpg "App2")

La segunda pantalla muestra la medición de la intensidad de ultrasonidos en función de la profundidad, de forma similar a una sonda de pesca convencional.

{:style="text-align:center;"}
![App1](/assets/images/2025/Prueba_Mar/App1.jpg "App1")

## Prueba en el mar

Para realizar las pruebas en el mar, todo el sistema se transportó en un kayak y se comprobó su funcionamiento a diferentes profundidades.

{:style="text-align:center;"}
![Prueba3](/assets/images/2025/Prueba_Mar/Prueba3.png "Prueba3")

El transductor se sumergió manualmente en el agua:

{:style="text-align:center;"}
![Prueba2](/assets/images/2025/Prueba_Mar/Prueba2.png "Prueba2")

El resultado de las mediciones fue el siguiente:

{:style="text-align:center;"}
![Prueba1](/assets/images/2025/Prueba_Mar/Prueba1.png "Prueba1")

Como se puede ver, el sistema es capaz de mostrar el fondo marino a profundidades de entre 0 y 15 metros, ya que la línea del fondo aparece en la posición correcta. Sin embargo, se observó que el algoritmo de detección de profundidad no funciona de forma óptima en este rango, ya que está ajustado para detectar distancias más cortas.

## Video

Aquí puedes ver un vídeo completo mostrando el funcionamiento del sistema:

{% include video id="1098302244" provider="vimeo" %}

## Conclusiones y próximos pasos

La prueba en el mar ha sido satisfactoria, ya que ha demostrado que el sistema puede medir la profundidad hasta unos 15 metros utilizando un transductor pequeño y de bajo coste.

De cara al futuro, los siguientes pasos serán:

- Mejorar el algoritmo de cálculo de profundidad, ya que actualmente está optimizado para distancias cortas y altas velocidades de muestreo.
- Diseñar un transductor de mayor tamaño para aumentar la sensibilidad y la potencia acústica radiada. La electrónica actual, gracias al uso de un transformador y MOSFETs de alta intensidad, permite utilizar transductores más potentes sin problemas.

¡Seguiremos compartiendo avances y mejoras!