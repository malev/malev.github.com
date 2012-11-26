--- 
layout: post
title: Sinatra app in multiple files
wordpress_id: 16
wordpress_url: http://blog.malev.com.ar/?p=16
comments: true
categories: 
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
Ya se, el título está en inglés, pero bueno. Este artículo fue inspirado por un hilo abierto en la lista de correo de Sinatra. Por lo tanto la idea original es de Alex Chaffee.
¿Qué pasa cuando tu aplicación crece demasiado y es difícil para la lectura contener todo el código en un solo archivo?
En el sitio de Sinatra recomiendan para las extensiones esta distribución:
<blockquote>sinatra-fu
|-- README
|-- LICENSE
|-- Rakefile
|-- app.rb
|-- lib
|   `-- sinatra
|       `-- fu.rb
|-- test
|   `-- spec_sinatra_fu.rb
`-- sinatra-fu.gemspec</blockquote>

Quizás se podría aplicar la misma para una aplicación Sinatrera. Otra opción muy interesante es usar la estructura de directorios que se recomienda para la construcción de gemas:
<blockquote>gem_name/
|-- History.txt
|-- README.txt
|-- Rakefile
|    `-- bin/
|    |-- app.rb
|-- lib/
|    `-- lib.rb
|-- test/
|    `-- test_app.rb
`-- views
</blockquote>
Por supuesto, a las dos estructuras propuestas hay que agregarles el directorio “views”. Los modelos pueden ir en el directorio “lib”.
Por último yo recomiendo crear un archivo “_requires.rb_” en la raiz de nuestra aplicación con un listado de todos las librerías a incluir:
<blockquote>
require 'rubygems'
require 'dm-core'
require 'lib/models.rb'</blockquote>
Para poder llamarlo desde la consola en el caso de necesitar hacer alguna prueba.
