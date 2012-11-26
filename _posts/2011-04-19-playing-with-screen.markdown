--- 
layout: post
title: Playing with screen
wordpress_id: 784
wordpress_url: http://blog.malev.com.ar/?p=784
comments: true
categories: 
- title: Programacion
  slug: programacion
  autoslug: programacion
- title: tools
  slug: tools
  autoslug: tools
- title: Ubuntu
  slug: ubuntu
  autoslug: ubuntu
tags: 
- title: tools
  slug: tools
  autoslug: tools
- title: linux
  slug: linux
  autoslug: linux
- title: screen
  slug: screen
  autoslug: screen
---
{{ page.title }}
================
En el pycamp [1], vi mucha gente usando **screen** para manejar su terminal. Luego de ponerme a investigar un poquito descubrí que se puede hacer cosas bastante interesantes con este nuevo programilla. Pero, antes, de qué estoy hablando? **GNU Screen** es un programa informático multiplexor de terminales desarrollado por el proyecto GNU. [2]

**Qué cosas tiene de bueno?**

* Muy configurable
* Permite dividir la pantalla
* Permite organizar muchas terminales virtuales

El último punto es el más interesante, ya que nos permite automatizar tareas que hacemos en nuestra consola. Digamos por ejemplo que tenemos una aplicación en rails 3. En la misma abrimos: Unicorn (server), una consola de rails, spork y watchr para nuestros test y por último queremos hacer un **tail -f al log**. Hacer todo esto siempre puede ser un poco molesto. La solución, obviamente: screen:

Primero una introducción: abrimos **screen** con ...

> screen

En principio no hay grandes cambios, pero si nos pasamos al modo comando: **Ctrl + A**, las cosas empiezan a mejorar. Para crear crear una segunda instancia de la terminal (Ctrl +A c) y navegamos entre ellas con n (next) o p (previous), siempre en el modo comando. También podemos dividir una ventana de manera horizontal. Para esto, entramos a modo comando (Ctrl + A) y escribimos **:split**. Vemos que se dividió nuestra pantalla. Con "Ctrl+A tab" nos paseamos entre las mitades, y presionamos "c" para crear una nueva terminal en cualquiera de las divisiones.
Hasta aquí nada del otro mundo, pero luego de ver una presentación de Tim Charper, pude ver como automatizar tareas comunes de nuestros entornos de desarrollo. Empezamos con un poco de configuración:
**/home/malev/.screerc**

{% highlight ruby %}
# Remap escape char to C-\
# escape \034\034
vbellwait 0.25
startup_message off
shell "-/bin/bash"
defscrollback 5000
chdir

caption always "%{-u wk} %L=%-Lw%45>%{+ Gk} %n%f* %t %{-}%+Lw%-0< %=%{ck} %H | %M %d %c "

bind j focus down
bind k focus up
bind t focus top
bind b focus bottom
{% endhighlight %}

Este archivo hace algunas cosas interesante: elimina el cartel de bienvenida y mapea las teclas j y k como en vim para una navegación más simple. También nos configura b y t para pasarnos entre divisiones de manera más simple. Luego, dentro del directorio de una aplicación que estemos usando agregamos:
/home/malev/code/grouppet/screen

{% highlight ruby %}
# why use stuff instead of exec? Because if the process dies, the screen
# session will die with it, leaving you unable to see the backtrace. Also,
# it's easier to restart that way. But, your choice :)
setenv ROOT "$HOME/code/grouppet"

screen
title "server"
stuff "cd $ROOT\015clear\015unicorn\015"

screen
title "logs"
stuff "cd $ROOT\015clear\015tail -f log/development.log\015"

screen
title "spork"
stuff "cd $ROOT\015clear\015spork\015"

screen
title "watchr"
stuff "cd $ROOT\015clear\015rake watchr\015"

screen
title "console"
stuff "cd $ROOT\015clear\015rails c\015"

screen
title "shell"
stuff "cd $ROOT\015clear\015"
{% endhighlight %}

La primer línea útil indica donde estamos parados. Luego crea varias screens y en cada una corre un comando: **unicorn**, los logs, **spork**, **watchr**, la consola y por último un shell limpio. Esto me permite tener mi entorno listo para empezar a trabajar con solo ejecutar:

{% highlight ruby %}
cd /home/malev/code/grouppet
screen -c screen
{% endhighlight %}

Nota: Por supuesto podemos crear un alias para este comando y así tener todo más simple todavía.
**Algunos comando útiles**

* C-a D : Sale de la terminal
* C-a :quit : Sale de screen
* C-a " : Muestra todas las screen abiertas
* C-a ? : Muestra ayuda

**Más información:**
Quick reference: http://aperiodic.net/screen/quick_reference

**Resources**

* [1] http://blog.malev.com.ar/2011/03/31/pycamp-2011/
* [2] http://es.wikipedia.org/wiki/GNU_Screen
* [3] http://www.gnu.org/software/screen/
* [4] http://www.debian-administration.org/articles/34
* [5] http://www.gnu.org/software/screen/manual/screen.html
