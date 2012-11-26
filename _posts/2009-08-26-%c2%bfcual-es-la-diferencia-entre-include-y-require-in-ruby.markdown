--- 
layout: post
title: "\xC2\xBFCu\xC3\xA1l es la diferencia entre include y require in Ruby?"
wordpress_id: 19
wordpress_url: http://blog.malev.com.ar/?p=19
comments: true
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
- title: core
  slug: core
  autoslug: core
- title: include
  slug: include
  autoslug: include
- title: load
  slug: load
  autoslug: load
- title: require
  slug: require
  autoslug: require
- title: Ruby
  slug: ruby
  autoslug: ruby
---
{{ page.title }}
================
Me lo pregunte varias veces. :D. La respuesta fue traducida de este blog [[1]](http://www.codeweblog.com/the-difference-between-ruby-s-require-load-and-include/).
El método **require** hace lo que hace include en casi todos los otros lenguajes: ejecuta otro archivo. También lleva una cuenta de todos lo que has requerido en el pasado, para no ejecutarlo dos veces. Para evitar esta funcionalidad, puedes usar el método **load**.

{% highlight ruby %}
require 'rubygems'
require 'sinatra'
get '/' do
  "requiriendo sinatra"
end
{% endhighlight %}

El método **include** toma todos los métodos desde otro módulo y los incluye en el módulo actual. Esto está en el nivel de lenguaje, a diferencia del require que se encuentra a nivel de archivo. El método **include** es la primer vía para expandir clases con otros módulos (mezclar módulos). Por ejemplo, si tu  clase define el método “each”, puedes incluir un método Enumerable, mezclarlos y hacerlo comportarse como una colección. Esto puede ser confuso ya que el verbo include es muy usado en otros lenguajes, pero de manera distinta. Include se usa por ejemplo a la hora de definir los modelos en Datamapper.

{% highlight ruby %}
class User
  include DataMapper::Resource
  property :id, Serial
  property :username, String
  property :email, String
end
{% endhighlight %}
