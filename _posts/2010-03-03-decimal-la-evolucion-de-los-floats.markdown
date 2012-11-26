--- 
layout: post
title: "Decimal, la evoluci\xC3\xB3n de los floats"
wordpress_id: 188
wordpress_url: http://blog.malev.com.ar/?p=188
comments: true
categories: 
- title: Programacion
  slug: programacion
  autoslug: programacion
- title: Python
  slug: python
  autoslug: python
tags: 
- title: calculo
  slug: calculo
  autoslug: calculo
- title: decimal
  slug: decimal
  autoslug: decimal
- title: floats
  slug: floats
  autoslug: floats
- title: Python
  slug: python
  autoslug: python
---
{{ page.title }}
================
Desde **Python** 2.4 se nos ha unido un módulo con un nuevo tipo de datos: **Decimal**, que permite manejar las matemáticas de punto flotante con algunas ventajas respecto a **float**:
<li>Contiene números como 1.1 que no tienen representación binaria</li>
<li>Más exacto: 0.1 + 0.1 + 0.1 - 0.3 = 0 pero cero en serio! y no 5.5511151231257827e-017 como en los floats</li>
<li>Tiene precisión regulable (28 lugares por defecto), pero extensible.</li>

El **módulo** se centra en 3 conceptos: el número decimal, el contexto aritmético y las señales.
**Número decimal:** inmutable, tiene un signo, coeficientes y un exponente. No trunca ceros a la derecha y posee los valores especiales: infinito, - infinito y **NaN** (no es un número). También diferencia entre -0 y +0.
**El contexto aritmético** es el ambiente donde se especifica la precisión, las reglas de redondeo, los límites en los exponentes, banderas que indican los resultados de las operaciones y "trampas" que permiten que ciertas señales sean tratadas como excepciones. La opciones de redondeo incluyen: ROUND_CEILING, ROUND_DOWN, ROUND_FLOOR, ROUND_HALF_DOWN, ROUND_HALF_EVEN, ROUND_HALF_UP, ROUND_UP, and ROUND_05UP.
**Las señales** son un conjunto de condiciones que surgen durante el proceso de cálculo. Y dependiendo de las necesidades de la aplicación, pueden ser ignoradas, consideradas información o tratadas como excepciones. Las señales en el módulo decimal son: Clamped, InvalidOperation, DivisionByZero, Inexact, Rounded, Subnormal, Overflow, and Underflow.
Para cada señal hay una **flag** (bandera) y una **trap** (trampa¿?). Cuando se encuentra una señal, la flag se pone a uno, luego si la habilitador de la trap está puesto en uno, entonces se envía una excepción (exception). Las flags quedan en uno, por lo que es necesario reiniciarlas antes de monitorear el cálculo.

Probando:

{% highlight python %}
from decimal import *
getcontext()
# Context(prec=28, rounding=ROUND_HALF_EVEN, Emin=-999999999, Emax=999999999, capitals=1, flags=[], traps=[Overflow, DivisionByZero, InvalidOperation])
getcontext().prec = 7       # Set a new precision
{% endhighlight %}

Los objetos decimal se instancian a partir de integers, string o tuplas. Para convertir una desde un float primero es recomendable convertilo primero a string.
Ya se, este post parece una traducción de [1] y medio que lo es, pero lo hice para obligarme a leer toda la documentación ;) También les dejo un tutorial más que interesante en [2].
**El contra de decimal:** No se pueden realizar operaciones entre números Decimal y floats. Antes los floats deben ser convertidos a string y luego a decimal. Bastante molesto cuando cargamos constantes, o algunos valores numéricos de una ecuación. Seguro hay alguna forma de hacer esto simple, pero todavía no la he encontrado :(
[1] [http://docs.python.org/library/decimal.html](http://docs.python.org/library/decimal.html)
[2] [http://broadcast.oreilly.com/2009/08/pymotw-decimal---fixed-and-flo.html](http://broadcast.oreilly.com/2009/08/pymotw-decimal---fixed-and-flo.html)
