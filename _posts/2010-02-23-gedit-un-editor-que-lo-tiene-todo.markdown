--- 
layout: post
title: Gedit - un editor que lo tiene todo!
wordpress_id: 163
wordpress_url: http://blog.malev.com.ar/?p=163
comments: true
categories: 
- title: Programacion
  slug: programacion
  autoslug: programacion
- title: tools
  slug: tools
  autoslug: tools
tags: 
- title: gedit
  slug: gedit
  autoslug: gedit
- title: gmate
  slug: gmate
  autoslug: gmate
- title: ide
  slug: ide
  autoslug: ide
- title: Programacion
  slug: programacion
  autoslug: programacion
---
{{ page.title }}
================
Muchas veces he visto en las listas de correo de **python**, **ruby**, etc. como la gente pregunta por una **IDE** para programar en tal o cual lenguaje. Y las recomendaciones son para todos los gustos. Desde los clásicos [Vim](http://www.vim.org/) y [Emacs](www.gnu.org/software/emacs) ([chiste](http://www.tiraecol.net/modules/comic/comic.php?content_id=3)) hasta los titanes [Eclipse](http://eclipse.org) y [Netbeans](http://netbeans.org). También los hay pagos y algunos exclusivos para python (Boa constructor). También aparecen los que recomiendan usar editores como [scibes](http://scribes.sourceforge.net/) o [E-TextEditor](http://www.e-texteditor.com/). Y no falta aquel que te recomienda comprarte una Mac y usar [Text-Mate](http://macromates.com/).
Yo en lo personal, hace tiempo que vengo usando [gedit](www.gnome.org/projects/gedit/ ), así es, gedit, el que viene instalado por defecto con Ubuntu. Ojo! está recontra [tuneado](http://es.wikipedia.org/wiki/Tuneo) ;)
Y aquí les voy a presentar una lista de todos los plug-ins que utilizo a menudo:


 * [Align](http://users.tkk.fi/~otsaloma/gedit/): Permite alinear bloques de texto en columnas

 * [Bazaar](http://launchpad.net/bzr-gedit) en gedit

 * [Class browser](http://www.stambouliote.de/projects/gedit_plugins.html)

 * [Comentar código](http://projects.gnome.org/gedit/plugins.html)

 * [Gemini](http://www.garyharan.com/), permite el autocompletado de código común ' "([{}])" '

 * [LaTeX](http://sourceforge.net/projects/gedit-latex/), latex en gedit*

 * Marcadores de código, para navegar más facilmente

 * Panel examinador de archivos

 * [Pastie](http://hiler.pl/)

 * Snippets: para usar recortes de código

 * Sangrar líneas

 * Smart indent: sangra automaticamente en base al código

 * word completion, completa automáticamente nuestro código

 * [Epydoc Docstring Wizard Gedit Plugin](http://code.google.com/p/docstringwizard/)

 * [gEdit Auto Complete](http://sourceforge.net/projects/gedit-autocomp/)

 * [Gedit Developers plugin](https://code.launchpad.net/gdp)

 * [Snap Open](https://github.com/MadsBuus/gedit-snapopen-plugin/)



* Uso mucho el plugin para la edición de documentos y realmente es fantástico!
**Opciones usadas:**
Espacios en lugar de tabuladores
Ancho del tabulador: 4
[
![](/images/posts/2010/02/opciones_gedit1.png "opciones_gedit")
](http://blog.malev.com.ar/wp-content/uploads/2010/02/opciones_gedit1.png)
Para podes hacer uso de estos plug-ins primero tienen que instalar los siguientes paquetes:
<blockquote>
sudo add-apt-repository ppa:ubuntu-on-rails/ppa
sudo apt-get install gedit-plugins gedit-gmate gedit-latex-plugin
</blockquote>
Nota: gmate es desarrollado por [Alexandre da Silva](http://blog.siverti.com.br) y el repositorio se encuentra [aquí](http://github.com/lexrupy/gmate/)
Me entero, gracias a [Gonzalo](http://gonzalodelgado.com.ar), que en la [wiki](http://python.org.ar/pyar/IDEs#Editores_de_texto_avanzados) de pyar hay un listado interesante de IDEs. Por si no te convencí con gedit :)
