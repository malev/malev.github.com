--- 
layout: post
title: Show rake tasks
wordpress_id: 466
wordpress_url: http://blog.malev.com.ar/?p=466
comments: true
categories: 
- title: rails
  slug: rails
  autoslug: rails
tags: 
- title: rails
  slug: rails
  autoslug: rails
- title: rubyonrails
  slug: rubyonrails
  autoslug: rubyonrails
- title: rake
  slug: rake
  autoslug: rake
---
{{ page.title }}
================
Para ver todas las tareas rake disponibles en un proyecto rails solo es necesario:
<blockquote>rake -T</blockquote>
Para ver todas las rutas disponibles: (además de revisar routes.rb)
<blockquote>rake routes</blockquote>
Ya se que es un post muy pobre, pero busque mucho esta info :)
Cómo mostrar las queries que genera ActiveRecord en la consola:
<blockquote>ActiveRecord::Base.logger = Logger.new(STDOUT)</blockquote>
Otra opción: en otra terminal escribir:
<blockquote>tail -f log/development.log</blockquote>
Cuando cambiamos un modelo, para no tener que reiniciar la consola, escribimos en la misma: reload!


