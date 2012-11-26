--- 
layout: post
title: "M\xC3\xA9todos de instancias y atributos"
wordpress_id: 96
wordpress_url: http://blog.malev.com.ar/?p=96
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
- title: atributos
  slug: atributos
  autoslug: atributos
- title: clase
  slug: clase
  autoslug: clase
- title: instancia
  slug: instancia
  autoslug: instancia
- title: Programacion
  slug: programacion
  autoslug: programacion
- title: Python
  slug: python
  autoslug: python
---
{{ page.title }}
================
He decidido hablar de estos temas por que difieren bastante de ruby y me costó un poco acostumbrarme al principio.
En un [post anterior](http://blog.malev.com.ar/2009/12/metodos-y-variables-de-clase-en-python/) hablamos de los métodos de clase. Ahora pasamos a los métodos de instancias [1]:

{% highlight python %}
#!/usr/bin/env python

class Clase:
    def __init__(self, argumento):
        self.atributo = argumento
        #aqui no puede haber return

    def metodo(self, argumento):
        salida = self.atributo + argumento + "!"
        return salida

if __name__ == "__main__":
    objeto = Clase("hola ")
    print objeto.metodo("mundo")
{% endhighlight %}

Los métodos de clase se definen como cualquier función y pueden devolver o no un valor (no es necesario el **return**). Un punto importante, es que siempre necesitan por lo menos un argumento, que es una referencia al objeto creado, es decir a la instancia creada, esto se hace a través de **self**. Otro punto interesante es el método **__init__** que viene a ser una suerte de constructor. Este lleva como argumentos el objeto (mediante self) y los argumentos que se consideren necesarios (en general de configuración). Por último cabe destacar que __init__ no puede devolver ningún valor (aquí no vale return).
Los atributos, son a mi entender como las variables de instancia y necesitan saber a que objeto hacen referencia y como puede ser esto .... SI con **self**. _self.atributo = argumento_. En ejemplo se setea un atributo y luego se lo llama.
[1] [http://docs.python.org/tutorial/classes.html](http://docs.python.org/tutorial/classes.html)
