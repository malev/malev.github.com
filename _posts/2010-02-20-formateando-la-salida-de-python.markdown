--- 
layout: post
title: Formateando la salida de python
wordpress_id: 127
wordpress_url: http://blog.malev.com.ar/?p=127
comments: true
categories: 
- title: formato
  slug: formato
  autoslug: formato
- title: Programacion
  slug: programacion
  autoslug: programacion
- title: Python
  slug: python
  autoslug: python
tags: 
- title: consola
  slug: consola
  autoslug: consola
- title: format
  slug: format
  autoslug: format
- title: formato
  slug: formato
  autoslug: formato
- title: Python
  slug: python
  autoslug: python
- title: salida
  slug: salida
  autoslug: salida
---
{{ page.title }}
================
En estos días tuve que realizar una simulación, bastante simple, pero que implicó numerosos cálculos complejos. Las opciones era: Matlab, octave, etc. Yo tuve experiencia con el primero. Si bien, pude resolver el problema de manera rápida, apenas tuve que empezar a cambiar algunos puntos del algoritmo o extenderlo, la cosa se volvió cada vez más lenta y tediosa. Me recuerdo un día haciendo cálculos a mano, por que ya no tenía ganas de programar más en ese día.
Esta vez, me decidí por un lenguaje más sólido. Claro, elegí **Python**.
Pero... (siempre hay un pero) el problema llegó a la hora de presentar los resultados. Grandes tablas de números difíciles de leer aparecían en la pantalla y ahí me pregunte: ¿Hay alguna forma de presentar esta información de una manera más amigable y legible?
¡CLARO QUE SÍ!

**Salida elegante**
Esta la podemos encontrar en el manual de python [1]. Puede ayudarnos mucho, cuando los resultados a mostrar no son demasiados. Los primeros son _str()_ y _repr()_. La diferencia entre ellos es que el primero nos entrega un valor legible por seres humanos y el segundo por intérpretes :S  [2]

{% highlight python %}
In [1]: a = 0.000000394
In [2]: str(a)
Out[2]: '3.94e-07'
In [3]: repr(a)
Out[3]: '3.9400000000000001e-07'
{% endhighlight %}


{% highlight python %}
for x in range(1, 5):
    print repr(x).rjust(10), repr(x*x).rjust(3), repr(x*x*x).rjust(6)
#         1   1      1
#         2   4      8
#         3   9     27
#         4  16     64
{% endhighlight %}

rjust(n) : indica cuantos “n” espacios se dejarán a la izquierda
Es necesario conocer cuánto “mide” cada cadena a presentar. También están las funciones primas: _ljust()_ y _center()_

{% highlight python %}
print '{0} con {1}'.format('pizza', 'cerveza')
print 'El valor de PI es aproximadamente {0}.'.format(math.pi)
print 'El valor de PI es aproximadamente {0!s}.'.format(math.pi)
# !s es el equivalente a str()
print 'El valor de PI es aproximadamente {0:.3f}.'.format(math.pi)
# pizza con cerveza
# El valor de PI es aproximadamente 3.14159265359.
# El valor de PI es aproximadamente 3.14159265359.
# El valor de PI es aproximadamente 3.142.

table = {'Sjoerd': 4127, 'Jack': 4098, 'Dcab': 7678}
for name, phone in table.items():
    print '{0:10} ==> {1:10d}'.format(name, phone)
# Dcab       ==>       7678
# Jack       ==>       4098
# Sjoerd     ==>       4127

table = {'Sjoerd': 4127, 'Jack': 4098, 'Dcab': 8637678}
print ('Jack: {0[Jack]:d}; Sjoerd: {0[Sjoerd]:d}; '
    'Dcab: {0[Dcab]:d}'.format(table))
# Jack: 4098; Sjoerd: 4127; Dcab: 8637678

# La forma C (antigua)
print '%(language)s esta de %(#)03d ' % \
    {'language': "Python", "#": 10}
# Python esta de 010

print "floats: %4.2f %+.0e %E " % (3.1416, 3.1416, 3.1416)
print "Preceding with zeros: %010d" % (1977)
print "floats: %5.2f" % 473247.472347237492374
print "floats: %E" % 473247.472347237492374
print "floats: %2.4E" % 473247.472347237492374
# floats: 3.14 +3e+00 3.141600E+00
# Preceding with zeros: 0000001977
# floats: 473247.47
# floats: 4.732475E+05
# floats: 4.7325E+05
{% endhighlight %}


<blockquote>'d' Signed integer decimal.
'x' Signed hexadecimal (lowercase).
'X' Signed hexadecimal (uppercase).
'e' Floating point exponential format (lowercase).
'E' Floating point exponential format (uppercase).
'f' Floating point decimal format.
'F' Floating point decimal format.
'c' Single character (accepts integer or single character string).
</blockquote>

**pprint (Ojo! Es así, no lo escribí mal)**
_Data pretty printer_. Es un módulo que permite presentar estructuras de datos de una forma “pretty” (linda). En general presenta la información en una sola línea a menos que se vea limitada por el argumento “width” (por defecto 80). [3]

{% highlight python %}
import pprint
stuff = ['spam', 'eggs', 'lumberjack', 'knights', 'ni']
stuff.insert(0, stuff[:])
pp = pprint.PrettyPrinter(indent=4)
pp.pprint(stuff)
#[   ['spam', 'eggs', 'lumberjack', 'knights', 'ni'],
#    'spam',
#    'eggs',
#    'lumberjack',
#    'knights',
#    'ni']
dic = {
    "hola" : "hello",
    "adios" : "good by",
    "buen dia" : "good morning"
}
pprint.pprint(dic)
#{'adios': 'good by', 'buen dia': 'good morning', 'hola': 'hello'}
{% endhighlight %}


**HTML**
La idea es la siguiente: una vez obtenido todos los resultados, formatearlos en HTML con la herramienta que nos presenta [4] y generar un archivo “.html” que se puede abrir en un explorador cualquiera. De esa manera, si hay algún cambio en los resultados, se puede re-generar el archivo html y actualizar la ventana del explorador (con F5 en firefox por ejemplo).

{% highlight python %}
import HTML
table_data = [
        ['Smith',       'John',         30],
        ['Carpenter',   'Jack',         47],
        ['Johnson',     'Paul',         62],
    ]

htmlcode = HTML.table(table_data,
    header_row=['Last name',   'First name',   'Age'])
print htmlcode
{% endhighlight %}


**CSV (otro día)**
[1] http://docs.python.org/tutorial/inputoutput.html
[2] http://docs.python.org/library/stdtypes.html#string-formatting
[3] http://docs.python.org/library/pprint.html
[4] http://www.decalage.info/python/html
