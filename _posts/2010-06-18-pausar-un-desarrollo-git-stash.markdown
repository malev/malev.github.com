--- 
layout: post
title: Pausar un desarrollo (git stash)
wordpress_id: 324
wordpress_url: http://blog.malev.com.ar/?p=324
comments: true
categories: 
- title: Programacion
  slug: programacion
  autoslug: programacion
- title: tools
  slug: tools
  autoslug: tools
tags: 
- title: git
  slug: git
  autoslug: git
- title: stash
  slug: stash
  autoslug: stash
- title: svn
  slug: svn
  autoslug: svn
- title: versiones
  slug: versiones
  autoslug: versiones
- title: scv
  slug: scv
  autoslug: scv
---
{{ page.title }}
================
Me paso que estaba en medio de una tarea específica, y apareció un pequeño problema con una refacción que acababa de subir (deploy). Para solucionarla bastaba solo con modificar 2 líneas de código bien conocidas, pero también necesitaba hacer deploy en ese momento. El problema: Cómo pausar mi trabajo en lo que venía haciendo, reparar este pequeño bug, hacer commit, push y deploy por último reiniciar mi trabajo.
**Opción 1:** hacer checkout a los archivos en que venía trabajando y perder los cambios. NO!
**Opción 2:** Bajarme de nuevo el proyecto en otro directorio, solucionar el problema ahí, hacer push y deply y después retomar a mi trabajo actual. NO!
**Opción 3:** Aprovechar GIT! =)
Existe una opción de git llamada _stash_ que: toma los cambios que vengo haciendo, los guarda en una zona temporal y me los elimina del branch, el branch queda como si estuviese recién comiteado. Entonces baje los cambios del otro desarrollador, repare el bug y luego recupere mis cambios desde el stash. Se que es muy complicado todo esto y difícil de explicar, pero el resumen de los comandos está aquí:
<blockquote>git stash
git pull
# working on changes
git add .
git commit -m "a message"
git stash pop
# continue as nothing has happened</blockquote>
También podría haber usado: git stash apply y continuar con los cambios en el temporal.

**Bibliografía**
[1] Ayuda de mi amigo Gus (creo que no tiene blog)
[2] [http://ariejan.net/2008/04/23/git-using-the-stash/](http://ariejan.net/2008/04/23/git-using-the-stash/)
[3] [http://gitref.org/](http://gitref.org/)
[4] [http://www.gitready.com/](http://www.gitready.com/)
