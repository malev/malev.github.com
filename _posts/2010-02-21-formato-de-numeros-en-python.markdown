--- 
layout: post
title: "Formato de n\xC3\xBAmeros en python"
wordpress_id: 145
wordpress_url: http://blog.malev.com.ar/?p=145
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
- title: float
  slug: float
  autoslug: float
- title: format
  slug: format
  autoslug: format
- title: integer
  slug: integer
  autoslug: integer
- title: Python
  slug: python
  autoslug: python
- title: string
  slug: string
  autoslug: string
---
{{ page.title }}
================
Una breve explicación de como formatear números en python (plagado de ejemplos).
Básicamente hay dos formas de formatear números:
**El formato viejo** que funciona en base al caracter "%". Presenta los valores de la derecha en base al formato establecido por los argumentos de la izquierda (muy similar a printf del C). Nota: será removido tarde o temprano de Python.

{% highlight python %}
import math
print 'The value of PI is approximately %5.3f.' % math.pi
{% endhighlight %}

**El método format() de la clase str: [1]** El objeto str puede contener texto o campos a reemplazar '{}'. Cada campo a reemplazar contiene un índice, un argumento de posición o el nombre del valor a asignar. [2]

{% highlight python %}
print "La suma de 1 + 2 is {0}".format(1+2)
# La suma de 1 + 2 is 3
print('esto va {primero} y esto {segundo}'.format(segundo= "2do", primero= "1ro"))
# esto va 1ro y esto 2do
{% endhighlight %}

<blockquote>**Campos a reemplazar || replacement field **
replacement_field ::=  "{" nombre del campo ["!" conversión] [":" formato] "}"
field_name          ::=  (identificador | entero) ("." nombre del atributo | "[" índice "]")*
nombre del atributo    ::=  identificador (una palabra)
índice  ::=  entero
conversión          ::=  "r" | "s" # repr o str</blockquote>
**Formato - Mini-Language**[3]
<blockquote>format_spec ::=  [[rellenar con]alineacion][signo][#][0][ancho][.precisión][tipo]
rellenar con ::=  un caracter distinto de '}'
alineacion  ::=  "<" | ">" | "=" | "^" # izq, der, centrado, otro centrado
signo ::=  "+" | "-" | " "
ancho ::=  entero
precisión ::=  entero
tipo ::=  "b" | "c" | "d" | "e" | "E" | "f" | "F" | "g" | "G" | "n" | "o" | "x" | "X" | "%"
</blockquote>
Ejemplos:

{% highlight python %}
In [32]: 'un numero {0:%^+9}'.format(2010)
Out[32]: 'un numero %%+2010%%'
In [43]: 'un valor: {0:0.4f}'.format(0.43543534)
Out[43]: 'un valor: 0.4354'
In [45]: 'un valor: {0:^+10.4f}'.format(0.43543534)
Out[45]: 'un valor:  +0.4354  '   #notense los espacios
In [46]: 'un valor: {0:#^+10.4f}'.format(0.43543534)
Out[46]: 'un valor: #+0.4354##'
In [47]: 'un valor: {0:#^+10.4f}'.format(0.0000000043543534)
Out[47]: 'un valor: #+0.0000##'
In [48]: 'un valor: {0:#^+10.4e}'.format(0.0000000043543534)
Out[48]: 'un valor: +4.3544e-09'
In [49]: 'un valor: {0:#^+15.4e}'.format(0.0000000043543534)
Out[49]: 'un valor: ##+4.3544e-09##'
{% endhighlight %}

Espero les sea útil.
[1] http://docs.python.org/library/stdtypes.html#str.format
[2] http://docs.python.org/library/string.html#format-string-syntax
[3] http://docs.python.org/library/string.html#format-specification-mini-language
