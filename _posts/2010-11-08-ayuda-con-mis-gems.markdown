--- 
layout: post
title: Ayuda con mis gems!!!
wordpress_id: 607
wordpress_url: http://blog.malev.com.ar/?p=607
comments: true
categories: 
- title: gems
  slug: gems
  autoslug: gems
- title: Programacion
  slug: programacion
  autoslug: programacion
- title: Ruby
  slug: ruby
  autoslug: ruby
tags: 
- title: Ruby
  slug: ruby
  autoslug: ruby
- title: gem
  slug: gem
  autoslug: gem
- title: help
  slug: help
  autoslug: help
- title: rdoc
  slug: rdoc
  autoslug: rdoc
---
{{ page.title }}
================
¿Cómo obtenemos ayuda con nuestras gemas? Como primer paso podemos correr en nuestra consola: **gem server**. Esto nos acciona un servidor en nuestro máquina local: http://localhost:8808 Al entrar ahí desde nuestro browser preferido veremos un listado de las gemas instaladas. Solo tenemos que buscar nuestra gema e ir al link: RDoc [1].
Una segunda opción es usar una gem: OpenGem. Que nos habilita 2 comandos nuevos muy interesantes:
<blockquote>gem install open_gem</blockquote>


 * **gem open rails** Abre nuestro editor por defecto apuntando al código fuente de la gem

 * **gem read rails** Abre nuestro browser con la documentación



Toda esta información fue extraída de RubyQuickTips [2]
[1] [http://en.wikipedia.org/wiki/RDoc](http://en.wikipedia.org/wiki/RDoc)
[2] [http://rubyquicktips.tumblr.com/post/437086745/easy-access-to-the-api-docs-for-gems](http://rubyquicktips.tumblr.com/post/437086745/easy-access-to-the-api-docs-for-gems)
