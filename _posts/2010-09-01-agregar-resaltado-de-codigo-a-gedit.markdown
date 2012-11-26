--- 
layout: post
title: "Agregar resaltado de c\xC3\xB3digo a gedit"
wordpress_id: 330
wordpress_url: http://blog.malev.com.ar/?p=330
comments: true
categories: 
- title: tools
  slug: tools
  autoslug: tools
- title: Ubuntu
  slug: ubuntu
  autoslug: ubuntu
- title: plugin
  slug: plugin
  autoslug: plugin
tags: 
- title: gedit
  slug: gedit
  autoslug: gedit
- title: rails
  slug: rails
  autoslug: rails
- title: syntax
  slug: syntax
  autoslug: syntax
- title: hightligh
  slug: hightligh
  autoslug: hightligh
- title: mime
  slug: mime
  autoslug: mime
- title: linux
  slug: linux
  autoslug: linux
---
{{ page.title }}
================
En un [post](http://blog.malev.com.ar/2010/02/23/gedit-un-editor-que-lo-tiene-todo/) anterior les comente sobre gedit, un excelente editor y potencialmente un gran IDE. Hace unos días atrás volví a usarlo para unos experimentos en Ruby on Rails. Pero había archivos en los que no funcionaba bien el resaltado de código (hight light code). Principalmente en los archivos de vistas ERB. Buscando encontré estos highlightcodes y aquí les cuento como instalarlos en Linux.
**1]** Busquen sus propios highlightcodes. [Aquí](http://blog.malev.com.ar/wp-content/uploads/mime.tar.gz) hay algunos para: haml, sass, erb y otros archivos que generalmente encontramos en un proyecto rails.
**2]** Copiar los archivos mime y lang-specs a sus correspondientes directorios:
<blockquote>
sudo cp ./mime/* /usr/share/mime/packages/
sudo cp ./lang-specs/* /usr/share/gtksourceview-2.0/language-specs/
</blockquote>
**3]** Agregar los mime types a la base de datos:
<blockquote>sudo update-mime-database /usr/share/mime</blockquote>
y listo!

**Créditos**
Los syntax hightlight los extraje de [aquí](http://github.com/gmate/gmate)
Las instrucciones de como instalarlos desde [aquí](http://grigio.org/pimp_my_gedit_was_textmate_linux)

