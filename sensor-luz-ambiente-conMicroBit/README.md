---
title: Sensor de luz ambiente con micro:bit
description: Medir y representar con micro:bit la cantidad de luz ambiente usando la pantalla LED matricial.
tags: micro:bit, sensor luz, medir luz
level: Facil
authors:
  - { name: Javier Solana Campoy, github: jviHub }
---

# Sensor de luz ambiente con micro:bit

Aunque la placa BBC micro:bit no tiene instalado un dispositivo especial como sensor de luz, podemos usar los diodos LED integrados para estimar la cantidad de luz.

Los LEDs son unos de los componentes opto-electrónicos más comunes utilizados para indicar la activación de una salida, sin embargo, pese a que no son dispositivos optimizados para la detección de luz, son muy efectivos como fotodiodo si los configuramos adecuadamente, por lo que pueden utilizarse como elementos de entrada en un sistema microprogramable.

Bajo condiciones de polarización inversa, un diodo LED se modela como un capacitor en paralelo con una fuente de corriente cuyo valor es proporcional a la luz incidente en el LED.



![](modeloFotodiodo.png)

## Materiales

- 1 Módulo micro:bit
- 1 Cable micro USB

## Esquema eléctrico

Descripción y fórmulas del esquema eléctrico si las hubiese

```
V = 5V - 2.1V = 2.9V
I = 20mA

Fórmulas
```

![](fritzing.png)

## Programación

Descripción interesante sobre la programación

```python
# Programa en python
```

```arduino
// Programa en arduino
```

![](mblock.png)
