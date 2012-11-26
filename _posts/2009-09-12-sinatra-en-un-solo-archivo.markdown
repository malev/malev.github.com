--- 
layout: post
title: Sinatra en un solo archivo
wordpress_id: 35
wordpress_url: http://blog.malev.com.ar/?p=35
comments: true
categories: 
- title: Ruby
  slug: ruby
  autoslug: ruby
- title: ruby-core
  slug: ruby-core
  autoslug: ruby-core
- title: sinatra
  slug: sinatra
  autoslug: sinatra
tags: 
- title: one file
  slug: one-file
  autoslug: one-file
- title: Ruby
  slug: ruby
  autoslug: ruby
- title: sinatral
  slug: sinatral
  autoslug: sinatral
---
{{ page.title }}
================
¡Increíble! Podés poner todo tu código e incluso las vistas en un solo archivo. Por supuesto esto es recomendable siempre y cuando tu archivo sea pequeño! ¿Cómo hacerlo? Simplemente ponemos el código de nuestra aplicación en la parte superior del archivo y luego del bloque  __END__ ubicamos toda las vistas incluyendo el layout.

<blockquote>require 'rubygems'
require 'sinatra'

get '/hello/:name' do
@name = params[:name]
erb :hello
end

__END__
@@ layout
&lt;html&gt;
&lt;body&gt;
&lt;%= yield %&gt;
&lt;/body&gt;
&lt;/html&gt;

@@ hello
&lt;h3&gt;Hello &lt;%= @name %&gt;!&lt;/h3&gt;</blockquote>

Ahora. ¿Cómo funciona esto? Resulta que el intérprete de Ruby ejecuta todo el código hasta que encuentra la etiqueta __END__ A partir de ahí todo el contenido restante se puede rescatar usando la variable global DATA. Esta última pertenece a la clase File.
ShiftEleven presenta un ejemplo interesante de como usar esta funcionalidad para guardar datos en YML.

[http://shifteleven.com/articles/2009/02/09/useless-ruby-tricks-data-and-__end__](http://shifteleven.com/articles/2009/02/09/useless-ruby-tricks-data-and-__end__)
