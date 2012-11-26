--- 
layout: post
title: Preparando mi entorno Python en Ubuntu 10.04
wordpress_id: 485
wordpress_url: http://blog.malev.com.ar/?p=485
comments: true
categories: 
- title: Programacion
  slug: programacion
  autoslug: programacion
- title: Python
  slug: python
  autoslug: python
- title: Ubuntu
  slug: ubuntu
  autoslug: ubuntu
tags: 
- title: Python
  slug: python
  autoslug: python
- title: virtualenv
  slug: virtualenv
  autoslug: virtualenv
- title: environment
  slug: environment
  autoslug: environment
---
{{ page.title }}
================
Ubuntu trae instalado python por defecto:
<blockquote>
malev@dell:~$ python -V
Python 2.6.5
</blockquote>

**Primero instalamos pip**, pero para esto necesitamos python-setuptools:
<blockquote>sudo apt-get install python-setuptools</blockquote>
Bajamos pip de aquí [1], descomprimimos y hacemos:
<blockquote>sudo python setup.py install
sudo ln -s /usr/local/bin/pip-2.6 /usr/bin/pip
</blockquote>

**Algunos intérpretes "bonitos":**
<blockquote>sudo apt-get install ipython bpython</blockquote>

**Bazaar**
Ya he hablado de este fantástico CVS [2] y hoy en día se ha convertido en una herramienta fundamental en mi trabajo con python, por lo que obligadamente tengo que hacer:
<blockquote>malev@dell:~/code$ sudo apt-get install bzr</blockquote>

**Me le animo a virtualenv:**
<blockquote>sudo pip install virtualenv
sudo pip install virtualenvwrapper</blockquote>

Creo un directorio para guardar mis proyectos:
<blockquote>mkdir /home/malev/dev</blockquote>

Agrego lo siguiente a ~/.bashrc:

{% highlight bash %}
export WORKON_HOME=~/dev
source /usr/local/bin/virtualenvwrapper.sh
{% endhighlight %}

Cierro y abro otra terminal.
Virtualenv ya está listo y a la espera :)

**Usando virtualenv** Basado en [3]
<blockquote>malev@dell:~/code$mkvirtualenv --distribute --no-site-packages stipple
(stipple)malev@dell:~/code$ cdvirtualenv
(stipple)malev@dell:~/dev/stipple$ </blockquote>

Compruebo que no tengo ninguna librería de python instalada, salvo por distribute y wsgiref:
<blockquote>(stipple)malev@dell:~/dev/stipple$ pip freeze</blockquote>

Vuelvo a la normalidad:
<blockquote>(stipple)malev@dell:~/code$  deactivate</blockquote>

Vuelvo a trabajar con stipple:
<blockquote>malev@dell:~/code$ workon stipple</blockquote>

**¿Cómo compartir mi entorno?**
Cuando trabajamos en un proyecto con varias personas es necesario que todxs tengamos instaladas las mismas librerías. Virtualenv y pip nos ayudan a compartir nuestro entorno con nuestros amigxs:
<blockquote>malev@dell:~/dev/stipple$ workon stipple
(stipple)malev@dell:~/dev/stipple$ pip freeze > ~/stipple_env.txt</blockquote>

Enviamos este archivo a nuestrxs amigos y ellxs tendrán que hacer desde sus computadoras:
<blockquote>
# nota: antes de distribute y no-site... se usa doble guión y no uno solo :)
mkvirtualenv --distribute --no-site-packages stipple_en_otro_lugar
pip install -r ~/stipple_env.txt
</blockquote>
Y así tendrán las mismas librerías que nosotros :)

**Haciendo andar pygtk en nuestro entorno, ¿Cómo?**
Este punto es bastante raro, por que pip install pygtk no funciona, para solucionarlo, yo instale pygtk2 usando apt-get y luego seguí las instrucciones de esta pregunta de stackoverflow:
[http://stackoverflow.com/questions/249283/virtualenv-on-ubuntu-with-no-site-packages](http://stackoverflow.com/questions/249283/virtualenv-on-ubuntu-with-no-site-packages)



**Links:**
[1] http://pypi.python.org/pypi/pip#downloads
[2] http://blog.malev.com.ar/2009/12/16/probando-bazaar
[3] http://www.youtube.com/watch?v=jI8VBP1wEZU&feature=related
