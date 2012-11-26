--- 
layout: post
title: GIT - Me olvide de cambiar de branch
wordpress_id: 374
wordpress_url: http://blog.malev.com.ar/?p=374
comments: true
categories: 
- title: tools
  slug: tools
  autoslug: tools
- title: buenas practicas
  slug: buenas-practicas
  autoslug: buenas-practicas
tags: 
- title: git
  slug: git
  autoslug: git
- title: stash
  slug: stash
  autoslug: stash
- title: branch
  slug: branch
  autoslug: branch
- title: master
  slug: master
  autoslug: master
- title: production
  slug: production
  autoslug: production
- title: vcs
  slug: vcs
  autoslug: vcs
---
{{ page.title }}
================
No es la primera vez que me pasa. Estoy trabajando en un proyecto, por alguna razón tengo que cambiarme a production y una vez finalizada la tarea en production. Comienzo a modificar archivos sin antes haberme cambiado a master. Luego, a la hora de ejecutar un commit caigo que estoy en production y yo pensaba que estaba en master. 
**¿Cómo soluciono este problema?**
Opción 1: Hago commit en production, me cambio a master, traigo ese commit con cherrypick y luego en production deshago el último commit. Una solución problemática
Opción 2: **git stash**[1]: en production hago:
<blockquote>git stash clear
git stash
git checkout master
git stash pop</blockquote>
y listo! Todos mis avances se movieron a master, **INCREIBLE!**
Ya les había hablado de [git stash](http://blog.malev.com.ar/2010/06/18/pausar-un-desarrollo-git-stash/) y la verdad, esta herramienta no deja de sorprenderme. Me pregunto si otros SCV tienen opciones similares ¿?

[1] [http://www.kernel.org/pub/software/scm/git/docs/git-stash.html](http://www.kernel.org/pub/software/scm/git/docs/git-stash.html)
