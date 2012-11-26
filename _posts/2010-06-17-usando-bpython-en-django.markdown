--- 
layout: post
title: Usando bpython en django
wordpress_id: 294
wordpress_url: http://blog.malev.com.ar/?p=294
comments: true
categories: 
- title: Programacion
  slug: programacion
  autoslug: programacion
- title: Python
  slug: python
  autoslug: python
- title: django
  slug: django
  autoslug: django
tags: 
- title: Python
  slug: python
  autoslug: python
- title: bpython
  slug: bpython
  autoslug: bpython
- title: django
  slug: django
  autoslug: django
- title: shell
  slug: shell
  autoslug: shell
- title: console
  slug: console
  autoslug: console
---
{{ page.title }}
================
Desde hace unos días que vengo jugando con django. Y hasta ahora me ha parecido muy interesante, pero había algo que no me convencía del todo. Cada vez que tenía que experimentar algo, tenía que usar [ipython](http://ipython.scipy.org/moin/), el cual es muy bueno, pero para mi, ya ha sido reemplazado por [bpython](http://bpython-interpreter.org/).
Bpython es según su propio sitio una linda interfase (_fancy interface_). Liberado con licencia MIT y con algunas cosillas interesantes: Resaltado de sintaxis en línea, autocompletado y sugerencias en pantalla, publicaciones en pastebin y permite guardar el historial del código escrito en un archivo.
Volviendo a Django. Cuando ejecutamos: python manage.py shell, en general (si está instalado) cargamos ipython con algunas librerías de Django ya importadas en el. Para usar Bpython con las mismas librerías importadas tenemos que:
1) Agregar al .bashrc la siguiente línea:
<blockquote>export PYTHONSTARTUP=~/.pythonrc</blockquote>
2) Crear un archivo ~/.pythonrc y en el introducir el siguiente código:

{% highlight python %}
#.pythonrc
try:
    from django.core.management import setup_environ
    import settings
    setup_environ(settings)
    print 'imported django settings'
except:
    pass
{% endhighlight %}

Y listo! ya podremos usa Bpython. Per esto no termina aquí! En este [link](http://uswaretech.com/blog/2009/12/using-bpython-shell-with-django-and-some-ipython-features-you-should-know/), podemos encontrar mucha más información sobre como mejorar nuestro shell.
