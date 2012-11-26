--- 
layout: post
title: "e\xC3\xB1e &quot;\xC3\x91&quot; y acentos en python"
wordpress_id: 101
wordpress_url: http://blog.malev.com.ar/?p=101
comments: true
categories: 
- title: Programacion
  slug: programacion
  autoslug: programacion
- title: Python
  slug: python
  autoslug: python
tags: 
- title: ascii
  slug: ascii
  autoslug: ascii
- title: Programacion
  slug: programacion
  autoslug: programacion
- title: Python
  slug: python
  autoslug: python
- title: utf8
  slug: utf8
  autoslug: utf8
---
{{ page.title }}
================
Un rato luego de empezar a usar python me di cuenta de que este no me aceptaba los acentos ni las eñes.. TREMENDO! Qué le pasa? **Año** no es lo mismo que **Ano**. Pero claro, como python parece que solo habla inglés...
Pero alguien [1] se tomó la molestia de averiguar que estaba pasando y descubrió que python por defecto considera que los caracteres con los que trabajamos son formato ASCII de 7 bits (ni siquiera el extendido). Por supuesto, en estos tristes 7 bits no entran ni la ñ ni los acentos, ni un montón de caracteres más. Es por esto que nosotros los hispano hablantes necesitamos decirle a Python que hay más caracteres y que los queremos interpretar :D
Para esto hacemos:

{% highlight python %}
#!/usr/bin/env python
# -*- coding: utf-8 -*-

print "¡Hola mundo con ñ y acentos papá!"
{% endhighlight %}

En la primera línea va la dirección del interprete y en la segunda línea especificamos que usaremos UTF8 en vez de ASCII.
[1] [http://www.ubuntu-es.org/?q=node/7474](http://www.ubuntu-es.org/?q=node/7474)
