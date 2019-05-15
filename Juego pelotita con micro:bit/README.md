---
title: Juego Pelotita
description: Desarrollando la programación. Uso de procedimientos y funciones. Práctica: uso de los acelerómetros en la placa Micro:bit.
tags: Micro:bit, microbit, juego, pelotita, pelota, acelerómetro
level: Medio, Avanzado
authors:
  - { name: Rafael Rubio Baeza, github: rafael-rubio }
---


title: 
description: 
tags: Micro:bit, microbit, juego, pelotita, pelota, acelerómetro
level: Medio
authors:
  - { name: Rafael Rubio Baeza, github: rafael-rubio }




# Juego de la pelotita

Basándome en una simple idea surgida cuando estaba descubriendo las posibilidades de la placa Micro:bit. Se me ocurrió poder implementar sobre ella un pequeño juego que fuese atractivo y motivador para los chavales, sería utilizado para dar un paso de gigante en el desarrollo de sus habilidades en programación, la introducción de los beneficios de trabajar con procedimientos y funciones. 

Así surge esta idea. Es un juego en el que utilizando los acelerómetros que posee esta placa, los chavales pueden probar sus habilidades y no solo con el juego en si, es solo una excusa, sino que lo que estamos desarrollando son sus hablidades en programación. Además, hay algún problema matemático que se ha resuelto en la implementación del programa que será utilizado como ejercicio de profundización.

Empecemos con el primer prototipo, una pelotita que se mantiene en la fila central y que utilizando el acelerómetro del eje x hay que mantener en el centro.





![](practica.gif)

## Materiales

- 1 Raspberry Pi
- 1 Cable USB - microUSB
- 1 Micro:bit

## Esquema eléctrico

Descripción y fórmulas del esquema eléctrico si las hubiese

```
V = 5V - 2.1V = 2.9V
I = 20mA

Fórmulas
```

![](fritzing.png)

## Programación

Empecemos con el primer prototipo, una pelotita que se mantiene en la fila central y que utilizando el acelerómetro del eje x hay que mantener en el centro.

```python
# Programa en python
from microbit import *
posicion=('10000','01000',"00100","00010","00001") # Array con las distintas posiciones en las que puede estar la pelotita.

bola_x=2 # Posición donde comienza la pelotita
while True:
    reading = accelerometer.get_x()
    
    if reading > 20:
        if bola_x ==4:
            display.show(Image.SAD) # Nos hemos salido del tablero de juego por la derecha
            sleep(1000)
            bola_x = 4
        else:
            bola_x += 1
            display.show(Image("00000:00000:"+posicion[bola_x] + ":00000:00000"))
            sleep(500)
            
    if reading < -20:
        if bola_x ==0:
            display.show(Image.SAD) # Nos hemos salido del tablero de juego por la izquierda
            sleep(1000)
            bola_x=0
        else:
            bola_x -= 1
            display.show(Image("00000:00000:"+posicion[bola_x] + ":00000:00000"))
            sleep(500)
```

```arduino
// Programa en arduino
```

![](mblock.png)
