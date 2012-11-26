--- 
layout: post
title: Method arguments in Ruby
wordpress_id: 628
wordpress_url: http://blog.malev.com.ar/?p=628
comments: true
categories: 
- title: Python
  slug: python
  autoslug: python
- title: rails
  slug: rails
  autoslug: rails
- title: Ruby
  slug: ruby
  autoslug: ruby
tags: 
- title: Python
  slug: python
  autoslug: python
- title: Ruby
  slug: ruby
  autoslug: ruby
- title: methods
  slug: methods
  autoslug: methods
- title: arguments
  slug: arguments
  autoslug: arguments
- title: helpers
  slug: helpers
  autoslug: helpers
---
{{ page.title }}
================
Hay algo que extraño mucho de otros lenguajes y no entiendo bien por que no lo implementarion en Ruby. Los argumentos con nombre de los métodos o funciones. ¿De qué estoy hablando? Miren como se declaran los métodos en python:

{% highlight python %}
def func(spam, eggs, toast=0, ham=0): # First 2 required
    print (spam, eggs, toast, ham)
func(1, 2)
func(1, ham=1, eggs=0)
func(spam=1, eggs=0)
func(toast=1, eggs=2, spam=3)
{% endhighlight %}

Lindo, lindo, muy lindo!
Lo interesante es poder pasar los objetos con nombre, entonces tenemos una lectura simple y no necesitamos atarnos al orden en que fue definida la función. Como les decía Ruby no cuenta con esta posibilidad :(
**Tipos de métodos en ruby:**
- Argumentos fijos
- Argumentos con valores por defecto
- Argumentos opcionales

{% highlight ruby %}
def metodo1(a, b)
end
def metodo2(a, b=2)
end
def metodo3(*p)
end
metodo1(1,2)
metodo2(4) # b valdría 2
medoto3(4,5,6) # p = [4,5,6]
{% endhighlight %}

El más interesante es el 3er caso en el que todos los argumentos se convierten en elementos de un arreglo (splat arguments). Nota: para esto se necesita el asterisco (*).
**Recibiendo un hash como argumento**

{% highlight ruby %}
def metodo(a, my_hash)
end
metodo 2, { :something => "shomething"}
# o podría ser:
metodo 2, :something => "shomething"
{% endhighlight %}

Cuando el último parámetro es un hash, Ruby no necesita de las llaves, lo cual los convierte en cuasi named arguments.
Ejemplos claro de esto último son los helpers de rails. Aquí un ejemplo:

{% highlight ruby %}
def link_to(*args, &block)
  if block_given?
    # ...
  else
    name         = args.first
    options      = args.second || {}
    html_options = args.third
    #...
  end
end
{% endhighlight %}

Un punto a destacar es como este método divide los argumentos en categorías. No es el único, de hecho, **link_to** y la mayoría de los _helpers_ dividen en 3 categorías, el primer y obligatorio (name), luego un diccionario para options y por último los html_options.

{% highlight ruby %}
link_to(name, options = {}, html_options = nil)
{% endhighlight %}
