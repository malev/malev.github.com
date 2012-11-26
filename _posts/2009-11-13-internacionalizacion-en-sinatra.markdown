--- 
layout: post
title: "Internacionalizaci\xC3\xB3n en Sinatra"
wordpress_id: 61
wordpress_url: http://blog.malev.com.ar/?p=61
comments: true
categories: 
- title: gems
  slug: gems
  autoslug: gems
- title: Ruby
  slug: ruby
  autoslug: ruby
- title: sinatra
  slug: sinatra
  autoslug: sinatra
tags: []

---
{{ page.title }}
================
Gracias a la gema: [r18n-sinatra](http://r18n.rubyforge.org/#sinatra) ya podemos internacionalizar nuestras pequeñas aplicaciones. Esta gema ya tiene más de un año, pero yo recién la descubrí ahora ;)
¿Cómo funciona? Necesitamos crear el directorio i18n en la raiz de nuestra aplicación y allí almacenar los archivos .yml
Ejemplo:
<blockquote>
require 'rubygems'
require 'sinatra'
require 'sinatra/r18n'

before do
  session[:locale] = params[:locale] if params[:locale]
end

get '/' do
  @decimal = 4.0/3
  erb :index
end

__END__
@@ index
<%= i18n.l Time.now %>
<%= (i18n.l @decimal).to_s %>
<%= i18n.post.friends %></blockquote>

**es.yml**
<blockquote>post:
..friends: Solo los amigos
</blockquote>

**en.yml**
<blockquote>post:
..friends: Only friends
</blockquote>
Una salvedad, como sabrán los archivos YML necesitan indentación. Aquí la estoy especificando con ".."
Por supuesto, esto es una mera introducción, queda en Uds. seguir investigando sobre el tema.
[http://github.com/ai/r18n](http://github.com/ai/r18n)
