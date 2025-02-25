---
title: "Diseño de un sistema Ecosonar - Introducción"
language: español

toc: true
toc: true
categories: 
  - Ecosonda  

  
full-width: true
tags:
  - Piezoeléctrico
  - Transductor
  - Ultrasonidos
  - Medidas

excerpt: En este artículo se introduce el diseño de un sistema de econsonda de alta precisión que incluye el diseño del transductor construido a través de impresión 3d, el diseño del controlador, como la programación del software para poder visualizar los datos.

header:
    teaser: /assets/images/2024/Intro_Sonda/Teaser.png
---

[English]({% post_url 2024-11-01-Transductor-Piezoelectrico-para-Sonar-Eng %})

# Introducción
Una ecosonda de ultrasonidos es un equipo que es capaz de poder detectar objetos en función de los ecos que se producen a partir de un generador de ondas de presión ultrasónicas.

En el mercado existen gran número de sondas comerciales que sirven tanto para analizar el fondo oceánico, como sondas de pesca o sistemas de localización para ROVs. Aquí se va ha presentar una primera versión de un sistema ecosonda como una prueba de concepto que podrá servir como base para poder desarrollar un producto especifico en el futuro.   

El equipo consta de las siguientes partes:

1. El transductor piezoeléctrico, es el elemento que convierte un señal eléctrica en una onda de presión y viceversa.
2. El generador de la señal ultrasónica, es el que produce la señal que necesita el transductor para generar las ondas de presión adecuadas.  
3. El medidor de la señal,que detecta la señales producidas por los ecos. 
4. Software de interpretación. El software nos permite interpretar las señales recibidas. 

{:style="text-align:center;"}
![assembly](/assets/images/2024/Intro_Sonda/Img_1.jpg "img1") 
 
# Diseño del transductor piezoeléctrico para el sonar.
El transductor es elemento principal del sistema de ecosonda, en nuestro caso se utiliza un cristal piezoeléctrico que tanto genera las ondas de presión de ultrasonidos como convierte los ecos en una señal eléctrica.

El transductor diseñando  esta compuesto de una carcasa impresa en 3d con filamento PETG pintada con resina epoxy, dentro se encuentran los elementos que constituyen el transductor:
1. El cristal piezoeléctrico: En este diseño se ha elegido el material PZT5, en formato disco de 4mm de grosor por 20mm de diámetro.
2. La matching layer: Es la capa que está entre el cristal piezoeléctrico y el agua, debe tener una impedancia acústica intermedia entre el material del cristal piezoeléctrico y el agua, el PETG tiene está característica. El grosor de esta capa se ha definido como 1/4 de la longitud de onda de presión. 
3. El backing layer. Es la capa que recubre el cristal, tiene que tener un alto grado de absorción de las ondas acústicas para evitar el ringing. En nuestro caso se ha elegido corcho.  

El disco piezoeléctrico de bajo coste elegido tiene un espesor de 4mm, lo cual provoca que su frecuencia de resonancia en modo axial sea de 500 kHz, el disco utilizado es el siguiente: 
<!-- {:style="text-align:center;"}
<img src="/assets/images/2024/Intro_Sonda/Piezo.jpg" title="Piezo" width="300" height="auto"> -->

{:style="text-align:center;"}
![Piezo](/assets/images/2024/Intro_Sonda/Piezo.jpg "Piezo") 

El diseño ha sido realizado con Freecad y en esta primer versión se ha elegido realizarlo de forma desmontable mediante una rosca que permite separar las dos partes y analizar el interior. Para asegurar la estanquidad se ha utilizado una junta tórica.
El diseño ha quedado de la siguiente forma:

{:style="text-align:center;"}
![Freecad](/assets/images/2024/Intro_Sonda/Freecad_2.png "Freecad") 

Para dar  más estanqueidad y resistencia a la pieza se ha postprocesado, pintándola de resina epoxy.
El resultado final es el siguiente:

{:style="text-align:center;"}
![Trasnducer](/assets/images/2024/Intro_Sonda/Transducer.jpg "Transducer") 

 En el diseño definitivo el interior se encapsulará totalmente con resina epoxy para asegurar la estanqueidad completamente. 

# Diseño del controlador 
El controlador diseñado tiene el siguiente diagrama de bloques.

{:style="text-align:center;"}
![Block](/assets/images/2024/Intro_Sonda/Sonar.jpg "Block") 

El controlador esta basado en una placa "black pill" con el microcontrolador stm32F401 y consta de tres partes:
1. El generador de ultrasonidos, utiliza le modulo PWM del microcontrolador para generar los pulsos de 500kHz. Esta señal, a través de unos Mosfets de potencia y un trasformador para aumentar la señal, se inyecta en el transductor piezoeléctrico para generar la onda de presión. Para tener la máxima precisión se ha implementado como señal generadora un tren de 5 pulsos a una frecuencia de 500 kHz.
2. El medidor de potencia ultrasónica. Se utiliza un detector de pico y un amplificador basado en OPAMPS para pasar la señal recibida por el transductor al ADC del microcontrolador.  
3. La interface de comunicación. Se utiliza el módulo comercial HC-06 para poder enviar la información recibida por bluetooth, de esta manera se puede enviar a un ordenador o a un teléfono inteligente. 

Se ha diseñado una PCB con KICAD, el resultado final tiene la siguiente forma: 

{:style="text-align:center;"}
![PCB](/assets/images/2024/Intro_Sonda/PCB.jpg "PCB") 

Se ha desarrollado el firmware mediante el framework de Arduino pero utilizando las librerías HAL de ST-Microelectronics para poder usar el DMA y las funciones avanzadas de los timers para el PWM y ADC.

Para realizar las primeras pruebas se ha utilizado el transductor en un recipiente cerrado donde se mantiene una distancia de 10 cm con el fondo,  poder así analizar la forma de onda.       

{:style="text-align:center;"}
<!-- ![10cm"](/assets/images/2024/Intro_Sonda/10cm.jpg "10cm") -->
<img src="/assets/images/2024/Intro_Sonda//10cm.jpg" title="10cm" width="300" height="auto"> 

Mediante el osciloscopio se puede analizar,tanto las señal generada para el transductor (Canal 1) como la respuesta medida del eco (Canal 2). 

{:style="text-align:center;"}
!["Osciloscope"](/assets/images/2024/Intro_Sonda/SN_1.4_2_e.png "Osciloscope")

Se observa que la señal generada es un tren de 5 pulsos de 170 V p-p, y que el eco tarda en llegar 125 uS. El resto de la señal son rebotes de las ondas de presión en el recipiente. 

Para calcular la distancia se utiliza el tiempo que tarda en volver un echo según la siguiente formula ![F1](/assets/images/2024/Intro_Sonda/F1.png "F1") donde c es la velocidad del sonido en el agua, y la t es el tiempo en que aparece un echo. En nuestro caso  ![F2](/assets/images/2024/Intro_Sonda/F2.png "F2")


# Software de control.
Se ha programado una interface gráfica en Pyhton con el framework KIVY para representar la señal  medida. Se utiliza la interfaz clásica de las sondas representando en el eje Y la distancia y representando con escala de colores la intensidad de la señal.        

La presentación es la siguiente:

{:style="text-align:center;"}
!["Inter3"](/assets/images/2024/Intro_Sonda/Interface3.png "Inter3")

La aplicación se comunica con la placa mediante bluetooth con el perfil de puerto serie y permite realizar una medida cada 1 segundo. Además se pude configurar la distancia de medida y diversos parámetros de visualización.
Se ha programado con el framework KIVY que permite compilar la aplicación para Android para, que en un futuro, poder hacer una sistema portátil y poder visualizar los datos con un teléfono inteligente o una tablet.
 
# Test
Para probar todo el sistema de ecosonda se utiliza un recipiente de plástico, se aplica el transductor sobre la pared y se comprueba que es capaz de detectar un destornillador de menos de 1 cm de diámetro a una distancia de aproximadamente 0.5 metros sin problemas.    

{% include video id="1023944174" provider="vimeo" %}

Se observa que el sistema es capaz de detectar los rebotes de la señal en los extremos del recipiente hasta varios metros de distancia. 

Cabe destacar que el cristal es de reducidas dimensiones, la frecuencia es relativamente alta (500kHz) y se ha usado un número reducido de pulsos, esto permite una detección con mucha precisión. Pero por otro lado, la frecuencia alta aumenta las perdidas de absorción de las ondas ultrasónicas en el agua, la poca sección del transductor reduce su sensibilidad y utilizar solo 5 pulsos limita la energía de los ecos, por eso la distancia de detección es relativamente reducida.   

# Conclusiones y trabajos futuros.
Esta  primera version se ha comprobado que es totalmente funcional y seria útil para detectar a distancias pequeñas con mucha precisión. 

Así mismo, este diseño da pie a muchas mejoras y variaciones que se pueden estudiar, como son:

- Estudiar diferentes tipos de transductores para diferentes necesidades. Se podría aumentar el diámetro del cristal piezoeléctrico para aumentar la sensibilidad o variar el espesor para variar la frecuencia de trabajo.  
- Variar el numero de pulsos. Para aumentar la distancia de detección se podría aumentar el numero de pulsos, pero esto reduciría la precisión de las medidas. 
- Hacer un sistema para poder determinar la impedancia de los transductores.
- Hacer un transductor compacto con la electronica integrada en la carcasa. 
- Mejorar la sensibilidad del detector de ultrasonidos.
- Desarrollar un app en android para hacer la sonda portátil.

En un futuro se intentará desarrollar estas mejoras.