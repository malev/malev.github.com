--- 
layout: post
title: "La funci\xC3\xB3n getattr()"
wordpress_id: 225
wordpress_url: http://blog.malev.com.ar/?p=225
comments: true
categories: 
- title: metaprogramacion
  slug: metaprogramacion
  autoslug: metaprogramacion
- title: objetos
  slug: objetos
  autoslug: objetos
- title: Programacion
  slug: programacion
  autoslug: programacion
- title: Python
  slug: python
  autoslug: python
tags: 
- title: metaprogramming
  slug: metaprogramming
  autoslug: metaprogramming
- title: Python
  slug: python
  autoslug: python
---
{{ page.title }}
================
Me encuentro haciendo un IRC bot [1] para el canal de #ubuntu-ar, el código fuente está aquí [2] y quiero extenderlo mediante plugins. Suena simple, pero no lo es para nada. Buscando como hacer semejante delirio descubrí el método getattr()[3]. Que según la documentación de python es:
<blockquote>**getattr(objeto, nombre[,default])**: Devuelve el valor de un atributo de un objeto. Si la cadena es el nombre de uno de los atributos del objeto, el resultado será el valor del atributo. Si el nombre no existe, se devuelve "default" y de no estar definido, se levanta la excepción AttributeError.</blockquote>
No entendi muy mucho, pero gracias a [4] y a [5] pude entender lo siguiente:

{% highlight python %}
# Equivalentes
from string import Formatter
Formatter #<class 'string.Formatter'>

import string
c = getattr(string, 'Formatter')
c #<class 'string.Formatter'>
{% endhighlight %}

Si son equivalentes, por qué molestarnos? Porque en el primer caso todo es estático, no se puede modificar en run time, mientras que en el segundo caso, como getattr toma como argumento cadenas, entonces se pueden modificar en tiempo de ejecición! ;)

{% highlight python %}
# Seguimos investigando!
# doc tendrá: 'len(object) -> integer\n\nReturn the number ...
doc = getattr(len, "__doc__")
#también se puede acceder a métodos:
result = "hola".find("a") # = 3
result =getattr("hola", "find")("a") # = 3
# Previendo exceptions
try:
    func = getattr(obj, "method")
except AttributeError:
    ... deal with missing method ...
else:
    result = func(args)
# Solo ejecuta si existe el método
func = getattr(obj, "method", None)
if callable(func):
    func(args)
{% endhighlight %}

Se que esto se usa para metaprogramación, pero todavía no lo entiendo bien, así que hasta ahí llego hoy.

[1] [http://en.wikipedia.org/wiki/Internet_Relay_Chat_bot](http://en.wikipedia.org/wiki/Internet_Relay_Chat_bot)
[2] [finop](https://code.launchpad.net/~marcosvanetta/+junk/finop)
[4] [http://docs.python.org/library/functions.html#getattr](http://docs.python.org/library/functions.html#getattr)
[4] [http://effbot.org/zone/python-getattr.htm](http://effbot.org/zone/python-getattr.htm)
[5] [El blog de Marcos Islas](http://islascruz.org/html/index.php?Blog/SingleView/id/Piratearle-las-propiedades-a-un-objeto-en-Python)
**Actualización!** Lectura recomendada [http://sydarex.org/repository/papers/py_dmlfd/py_dmlfd.html](http://sydarex.org/repository/papers/py_dmlfd/py_dmlfd.html)
