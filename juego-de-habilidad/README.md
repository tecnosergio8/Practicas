---
title: Juego de habilidad
description: Avanzando en la programación. Uso de procedimientos y funciones. Práctica con los acelerómetros en la placa Micro:bit.
tags: Micro:bit, microbit, juego, pelotita, pelota, acelerometro, habilidad
level: Medio, Avanzado
authors:
  - { name: Rafael Rubio Baeza, github: rafael-rubio }
---

# Juego de habilidad

![](habilidad_reto.gif) 

Basándome en una simple idea surgida cuando estaba descubriendo las posibilidades de la placa Micro:bit. Se me ocurrió poder implementar sobre ella un pequeño juego que fuese atractivo y motivador para los chavales, sería utilizado para dar un paso de gigante en el desarrollo de sus habilidades en programación, la introducción de los beneficios de trabajar con procedimientos y funciones. 

Así surge esta idea. Es un juego en el que utilizando los acelerómetros que posee esta placa, los chavales pueden probar sus habilidades. El juego es solo una excusa, ya que lo que estamos en el fondo persiguiendo es desarrollar sus hablidades en programación. Aparte de las disciplinas básicas, hay algún problema matemático que se ha resuelto en la implementación del programa que será utilizado como ejercicio de profundización.


## Materiales

- 1 Raspberry Pi
- 1 Cable USB - microUSB
- 1 Micro:bit


## Programación

Empecemos con el primer juego, una pelotita que hay que mantener en la fila central sin que se salga por los laterales utilizando el acelerómetro del eje x para que se mantenga en las posiciones centrales.

```python
# Programa en python
from microbit import *

# Array con las distintas posiciones en las que puede estar la pelotita.
posicion=('50000','05000',"00500","00050","00005") 

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
  
  Se ha utilizado una __función__: mostrar(x,y) y un __procedimiento__: fuera(), lo que hace q  ue el programa sea más sencillo de interpretar y de entender.
  
  Llegamos así a la versión final, me pareció que sería un juego más motivador si le incorporaba un reto. Definir al comienzo del juego una puerta de salida entre los 16 leds que forman el exterior. Si logramos salir por esa _puerta de escape_ se mostrará una cara sonriente y si nos salimos por otra, la clásica cara triste.

Quedando el juego así:

![](habilidad_reto.gif)  
  
  
El progrma lo podéis obtener copiando el siguiente código:
  
  ```
  # Programa en python
  # Escribe tu código aquí :-)
from microbit import *
from random import randrange


posicion=('90000','09000',"00900","00090","00009")



sensibilidad = 20 # Sensibilidad del juego
dificultad = 200


def mostrar(x,y): # Presenta en el display la bolita
    cadena=''
    for i in range(5):
        if i != y:
            cadena += '00000:'
        else:
            cadena = cadena + posicion[x] + ':'
        cadena = cadena[:29]
    display.show(Image(cadena))
    sleep(dificultad)


def fuera(out_x,out_y,x,y):  # El jugador ha fallado

    if x == out_x and y == out_y: # Comprueba si la bola ha salido por el sitio correcto
        display.show(Image.HAPPY)
        sleep(1000)
        out_x, out_y = empezar()
        x=2
        y=2
        mostrar(x,y)
        sleep(1000)

    else:
        display.show(Image.SAD)
        sleep(1000)
    return out_x,out_y, x, y

def empezar():  # Establece la casilla de escape
    salida = randrange(0,16)
    if salida < 5:
        out_x = salida
        out_y = 0
    elif salida > 10:
        out_x= salida - 11
        out_y = 4
    elif salida%2 == 0:
        out_x = 4
        out_y = ((salida-5)//2)+1
    else:
        out_x=0
        out_y = ((salida-5)//2)+1
    mostrar(out_x,out_y)
    sleep(1000)
    return out_x, out_y

# Principio del programa

salida_x, salida_y = empezar()
# Posiciones iniciales de la pelotita
bola_x=2
bola_y=2

while True:
    read_x = accelerometer.get_x()
    read_y = accelerometer.get_y()
    if read_y > sensibilidad:
        if bola_y == 4:
            salida_x, salida_y, bola_x,bola_y = fuera(salida_x,salida_y,bola_x,bola_y)
        else:
            bola_y += 1
            mostrar(bola_x,bola_y)

    if read_y < -1 * sensibilidad:
        if bola_y == 0:
            salida_x, salida_y, bola_x,bola_y = fuera(salida_x,salida_y,bola_x,bola_y)
        else:
            bola_y -= 1
            mostrar(bola_x,bola_y)


    if read_x > sensibilidad:
        if bola_x ==4:
            salida_x, salida_y, bola_x,bola_y = fuera(salida_x,salida_y,bola_x,bola_y)
        else:
            bola_x += 1
            mostrar(bola_x,bola_y)

    if read_x < -1*sensibilidad:
        if bola_x ==0:
            salida_x, salida_y, bola_x,bola_y = fuera(salida_x,salida_y,bola_x,bola_y)
        else:
            bola_x -= 1
            mostrar(bola_x,bola_y)
  ```
En este programa merece la pena estudiar la función __empezar()__ y como se consigue tras obtener un número aleatorio entre 0 y 15 asignarle un led de escape, sería interesante repasar las operaciones: divisiones enteras (//) y los restos de una división (%).

```
if salida < 5:
        out_x = salida
        out_y = 0
    elif salida > 10:
        out_x= salida - 11
        out_y = 4
    elif salida%2 == 0: # % - Resto de la división
        out_x = 4
        out_y = ((salida-5)//2)+1   # // - División entera
    else:
        out_x=0
        out_y = ((salida-5)//2)+1
```
