---
title: Juego de habilidad
description: Avanzando en la programación. Uso de procedimientos y funciones. Práctica con los acelerómetros en la placa Micro:bit.
tags: Micro:bit, microbit, juego, pelotita, pelota, acelerometro, habilidad
level: Medio, Avanzado
authors:
  - { name: Rafael Rubio Baeza, github: rafael-rubio }
---




# Juego de habilidad

Basándome en una simple idea surgida cuando estaba descubriendo las posibilidades de la placa Micro:bit. Se me ocurrió poder implementar sobre ella un pequeño juego que fuese atractivo y motivador para los chavales, sería utilizado para dar un paso de gigante en el desarrollo de sus habilidades en programación, la introducción de los beneficios de trabajar con procedimientos y funciones. 

Así surge esta idea. Es un juego en el que utilizando los acelerómetros que posee esta placa, los chavales pueden probar sus habilidades. El juego es solo una excusa, ya que lo que estamos en el fondo persiguiendo es desarrollar sus hablidades en programación. Aparte de las disciplinas básicas, hay algún problema matemático que se ha resuelto en la implementación del programa que será utilizado como ejercicio de profundización.







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

Empecemos con el primer juego, una pelotita que hay que mantener en la fila central sin que se salga por los laterales utilizando el acelerómetro del eje x para que se mantenga en las posiciones centrales.

```python
# Programa en python
from microbit import *
posicion=('50000','05000',"00500","00050","00005") # Array con las distintas posiciones en las que puede estar la pelotita.

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
El juego quedaría así:

![](habilidad_lineal.gif)

El siguiente paso parece evidente, extenderlo a todos los leds y trabajar en dos dimensiones, aquí surgen al gunos pequeños problemas que hay que resolver pero con un poco de esfuerzo nada es imposible.

```
# Programa en python
from microbit import *
posicion=('50000','05000',"00500","00050","00005")

# Posiciones iniciales de la pelotita
bola_x=2
bola_y=2

sensibilidad = 20 # Disminuir el número para aumentar la sensibilidad
dificultad = 200 # Poner un número más pequeño para aumentar la dificultad del juego


while True:
    read_x = accelerometer.get_x()
    read_y = accelerometer.get_y()
    if read_y > sensibilidad:
        if bola_y == 4:
            display.show(Image.SAD)
            sleep(1000)
            bola_y = 4
        else:
            bola_y += 1
            cadena=''
            for i in range(5):
                if i != bola_y:
                    cadena += '00000:'
                else:
                    cadena = cadena + posicion[bola_x] + ':'
            cadena = cadena[:29]
            display.show(Image(cadena))
            sleep(dificultad)

    if read_y < -1 * sensibilidad:
        if bola_y == 0:
            display.show(Image.SAD)
            sleep(1000)
            bola_y = 0
        else:
            bola_y -= 1
            cadena=''
            for i in range(5):
                if i != bola_y:
                    cadena += '00000:'
                else:
                    cadena = cadena + posicion[bola_x] + ':'
            cadena = cadena[:29]
            display.show(Image(cadena))
            sleep(dificultad)



    if read_x > sensibilidad:
        if bola_x ==4:
            display.show(Image.SAD)
            sleep(1000)
            bola_x = 4
        else:
            bola_x += 1
            cadena=''
            for i in range(5):
                if i != bola_y:
                    cadena += '00000:'
                else:
                    cadena = cadena + posicion[bola_x] + ':'
            cadena = cadena[:29]
            display.show(Image(cadena))
            sleep(dificultad)
    if read_x < -1*sensibilidad:
        if bola_x ==0:
            display.show(Image.SAD)
            sleep(1000)
            bola_x=0
        else:
            bola_x -= 1
            cadena=''
            for i in range(5):
                if i != bola_y:
                    cadena += '00000:'
                else:
                    cadena = cadena + posicion[bola_x] + ':'
            cadena = cadena[:29]
            display.show(Image(cadena))
            sleep(dificultad)

```

![](habilidad_plano.gif)

Este es el punto donde se puede hacer enfasis entre los alumnos para que se den cuenta que el programa tiene trozos que se repiten en varias partes del código, justo lo que necesitamos para reforzar la idea de _procedimiento_ y de _función_.
El codigo utilizando funciones y procedimientos queda:
  ```
  # Programa en python
  # Escribe tu código aquí :-)
from microbit import *



posicion=('90000','09000',"00900","00090","00009")



sensibilidad = 20 # Sensibilidad del juego
dificultad = 200


def mostrar(x,y): # Presenta en el display la bolita
    cadena=''
    for i in range(5):
        if i != y:
            cadena += '00000:'
        else:
            cadena = cadena + posicion[bola_x] + ':'
        cadena = cadena[:29]
    display.show(Image(cadena))
    sleep(dificultad)


def fuera():  # El jugador ha fallado
    display.show(Image.SAD)
    sleep(1000)


# Principio del programa



# Posiciones iniciales de la pelotita
bola_x=2
bola_y=2







while True:
    read_x = accelerometer.get_x()
    read_y = accelerometer.get_y()
    if read_y > sensibilidad:
        if bola_y == 4:
            fuera()
            bola_y = 4
        else:
            bola_y += 1
            mostrar(bola_x,bola_y)

    if read_y < -1 * sensibilidad:
        if bola_y == 0:
            fuera()
            bola_y = 0
        else:
            bola_y -= 1
            mostrar(bola_x,bola_y)


    if read_x > sensibilidad:
        if bola_x ==4:
            fuera()
            bola_x = 4
        else:
            bola_x += 1
            mostrar(bola_x,bola_y)

    if read_x < -1*sensibilidad:
        if bola_x ==0:
            fuera()
            bola_x=0
        else:
            bola_x -= 1
            mostrar(bola_x,bola_y)
  
  ```
  
  Se ha utilizado una función: 
