--- 
layout: post
title: Using mercurial
wordpress_id: 240
wordpress_url: http://blog.malev.com.ar/?p=240
comments: true
categories: 
- title: cvs
  slug: cvs
  autoslug: cvs
tags: 
- title: hg
  slug: hg
  autoslug: hg
- title: mercurial
  slug: mercurial
  autoslug: mercurial
- title: svc
  slug: svc
  autoslug: svc
---
{{ page.title }}
================
En el pycamp2010 [1], participe de un proyecto muy interesante, un juego basado en tower defense usando cocos2d. En el desarrollo participamos varios "jugadores" y como sistema de control de versiones usamos Mercurial. Fue muy divertido y dado que todos modificabamos un solo archivos casi en simultaneo, rompimos el repositorio varias veces.
[
![Mercurial SCM](/images/posts/2010/04/mercurial1.png "Mercurial SCM")
](http://mercurial.selenic.com/)
**Un mini tutorial:**
<blockquote>Install Mercuria
sudo apt-get install mercurial
malev@lucid:~$ hg
Mercurial Distributed SCM
basic commands:
 add        add the specified files on the next commit
 annotate   show changeset information by line for each file
...
use "hg help" for the full list of commands or "hg -v" for details
</blockquote>
**Configurando Hg en nuestro sistema:**
**Una** de las opciones es crear un archivo .hgrc en nuestra home con el siguiente contenido:
<blockquote>
[ui]
username = Marcos Vanetta <marcosvanettaq@example.net>
verbose = True #No siempre es una buena opción
</blockquote>
Los comandos no son para nada locos, tenemos como siempre:
<blockquote>
hg init
hg add
hg remove
hg commit
hg rollback
hg log
hg revert --all #elimina los cambios hasta el último commit
hg status
hg push
hg diff
hg clone
hg pull
hg update
hg merge
</blockquote>
**Cosas Interesantes**
<blockquote>
hg serve #Servidor interno que nos permite navegar por nuestro código y commits de manera visual en un browser.
hg outgoing # Muestra los cambios del repositorio actual que no han sido "pusheados"</blockquote>
**Bibliografía:**
Por qué usar Mercurial? [http://mercurial.selenic.com/about/](http://mercurial.selenic.com/about/)
Screencast (pago) [http://peepcode.com/products/meet-mercurial](http://peepcode.com/products/meet-mercurial)
a mercurial tutorial: [http://hginit.com/index.html](http://hginit.com/index.html)
Sitio oficial: [http://mercurial.selenic.com/](http://mercurial.selenic.com/)
Presentación de Pycon: [http://blip.tv/file/3359642](http://blip.tv/file/3359642)
