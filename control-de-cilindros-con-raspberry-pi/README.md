---
title: Gobierno a distancia de dos cilindros neumáticos con Raspberry Pi
description: Programación en Python de dos botones para el control del avance y retroceso de dos cilibndros nuemáticos vía wifi.
tags: Raspberry Pi, Python, Neumática
level: Facil
authors:
  - { name: Sergio Lino Ferrández Gil, github: tecnosergio8 }
---

# Gobierno a distancia de dos cilindros neumáticos con Raspberry Pi

Programación en Python de dos botones para el control del avance y retroceso de dos cilibndros nuemáticos vía wifi.

![](preview.gif)

## Materiales

- 1 Raspberry Pi
- 1 módulo de dos relés de 5 V.
- Cables de conexión Dupont hembra-hembra para conectar los relé a la Raspberry Pi.
- 2 Cilindros neumáticos de doble efecto.
- 2 electroválvulas neumáticas 3/2 de 24 V.
- 1 Fuente de alimentación de 24 V para alimentación de las electroválvulas.
- 1 Compresor neumático con unidad de mantenimiento.
- Conductos neumáticos para distribución de aire a presión.
- Cables de conexión para las electroválvulas.

## Programación

Se muestran los progamas en Python y el HTML para hacer los botones.

```python
# Programa en python
from flask import *
app = Flask(__name__)

import RPi.GPIO as GPIO
GPIO.setwarnings(False)
GPIO.setmode(GPIO.BCM)

# leds
amarillo = 17
verde = 18
GPIO.setup(amarillo, GPIO.OUT)
GPIO.output(amarillo, GPIO.LOW)
GPIO.setup(verde, GPIO.OUT)
GPIO.output(verde, GPIO.LOW)

@app.route('/')
def home():
   templateData = {
      'amarillo' : GPIO.input(amarillo),
      'verde' : GPIO.input(verde),
   }
   return render_template('home.html', **templateData)

@app.route('/<led>/<action>')
def led(led, action):
   GPIO.output(int(led), int(action))
   templateData = {
      'amarillo' : GPIO.input(amarillo),
      'verde' : GPIO.input(verde),
   }
   return render_template('home.html', **templateData)

if __name__ == '__main__':
   app.run(host='0.0.0.0', port=8080, debug=True)
```

```HTML
<html>
<head>
    <div style="display: block; width:750px ; height: 120px;font-size: 100px; margin: 100px; padding: 10px 10px ;text-align: center; color: white; background: blue;">CILINDROS</div>
    <div style="width:300px ; height: 120px;position:relative ; top: 70;font-size: 100px; margin: 100px ; text-align: center; color: red;font-weight: bold; background: yellow;">1</div>
    <div style="width:300px ; height: 120px; position:relative ; top: -150; left: 565; font-size: 100px ;text-align: center; color: red;font-weight: bold; background: yellow;">2</div>    
        <style>
      .btn { 
         width:300px;
         height:300px;
         position: relative;
         top: -300;
         border-radius:1000px;
         margin: 100px;
         padding: 12px 50px; 
         text-align: center;
         border: 1px solid #000;
         background: #20ca00;
         text-decoration: none;
         font-size: 350px;
         line-height: 3;
         color: #ffffff;
      }
      .btn.amarillo {
         background: #ff0000;
      }
      </style>
      <style>
      .btn1 { 
         width:300px;
         height:300px;
         position: relative;
         top: -300;
         border-radius:1000px;
         margin: 0px;
         padding: 12px 50px; 
         text-align: center;
         border: 1px solid #000;
         background: #20ca00;
         text-decoration: none;
         font-size: 350px;
         line-height: 3;
         color: #ffffff;
      }
      
      .btn1.verde {
         background: #ff0000;
      }
   </style>
</head>
<body>
   {% if amarillo == 0 %}
      <a class="btn" href="/17/1">A</a>
   {% else %}
      <a class="btn amarillo" href="/17/0">R</a>
   {% endif %}

   {% if verde == 0 %}
      <a class="btn1" href="/18/1">A</a>
   {% else %}
      <a class="btn1 verde" href="/18/0">R</a>
   {% endif %}
</body>
</html>
```
