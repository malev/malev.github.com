--- 
layout: post
title: "M\xC3\xA9todos y variables de clase en Python"
wordpress_id: 89
wordpress_url: http://blog.malev.com.ar/?p=89
comments: true
categories: 
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
- title: clase
  slug: clase
  autoslug: clase
- title: instancia
  slug: instancia
  autoslug: instancia
- title: metodos
  slug: metodos
  autoslug: metodos
- title: objetos
  slug: objetos
  autoslug: objetos
- title: Python
  slug: python
  autoslug: python
---
{{ page.title }}
================
Los métodos de clase, son aquellos que podemos llamar y que no necesitan ser instanciados. Mientras que las variables de clase, son comunes a todas las instancias creadas, se pueden usar para establecer parámetros de configuración comunes a todos los objetos creados y a crear.

{% highlight python %}
#!/usr/bin/env python

class Clase:
    variable = "hola"
    @classmethod
    def metodo(cls):
        print "hola metodo de clase"

if __name__ == "__main__":
    print "Clase.variable"
    print Clase.variable
    Clase.metodo()
{% endhighlight %}

Habría que ver si hay alguna ventaja en definir métodos de clases o simplemente crear funciones y añadirlas a un módulo y luego importarlas.
