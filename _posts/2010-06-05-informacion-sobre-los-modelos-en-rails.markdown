--- 
layout: post
title: "Informaci\xC3\xB3n sobre los modelos en rails"
wordpress_id: 302
wordpress_url: http://blog.malev.com.ar/?p=302
comments: true
categories: 
- title: rails
  slug: rails
  autoslug: rails
- title: Ruby
  slug: ruby
  autoslug: ruby
- title: plugin
  slug: plugin
  autoslug: plugin
- title: ActiveRecord
  slug: activerecord
  autoslug: activerecord
tags: 
- title: rails
  slug: rails
  autoslug: rails
- title: Ruby
  slug: ruby
  autoslug: ruby
- title: git
  slug: git
  autoslug: git
- title: models
  slug: models
  autoslug: models
- title: plugin
  slug: plugin
  autoslug: plugin
---
{{ page.title }}
================
Por alguna extraña razón, cuando trabajamos con un modelo en Rails, no tenemos ninguna información sobre las tablas de la base de datos a las que el modelo representa. Para solucionar esto, yo siempre revisaba el archivo schema.rb o simplemente copiaba el contenido relevante como comentario en mi modelo. Pero alguien ya había pensado en algo mejor, Dave Thomas [1], un gran programador, creo en el 2006 el plugin: annotate_models [2] que le agrega a los modelos la info sobre la base de datos de la siguiente manera:

{% highlight ruby %}
# == Schema Information
# Schema version: 20100529015450
#
# Table name: users
#
#  id         :integer         not null, primary key
#  name       :string(255)
#  email      :string(255)
#  created_at :datetime
#  updated_at :datetime
#

class User < ActiveRecord::Base

end
{% endhighlight %}

Claro, para usar este plugin, primero hay que instalarlo con: 
<blockquote>script/plugin install http://repo.pragprog.com/svn/Public/plugins/annotate_models</blockquote>
y luego llamarlo con:
<blockquote>rake annotate_models</blockquote>
Pero, cada vez que modifiquemos la base de datos, tendremos que llamar de nuevo al plugin. Para evitar esto, Michael Hartl [3] nos propone llamar a esta tarea rake cada vez que hacemos commit. Para esto, tenemos que crear un archivo .git/hooks/pre-commit y agregarle al mismo: 
<blockquote>#!/bin/sh
rake annotate_models > /dev/null</blockquote>
Por último es necesario brindarle permisos de ejecución con: **chmod +x .git/hooks/pre-commit** y listo!

[1] http://pragdave.pragprog.com/
[2] http://pragdave.pragprog.com/pragdave/2006/02/annotate_models.html
[3] http://www.railstutorial.org
