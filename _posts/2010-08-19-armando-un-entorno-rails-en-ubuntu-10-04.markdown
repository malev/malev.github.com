--- 
layout: post
title: Armando un entorno Rails en Ubuntu 10.04
wordpress_id: 479
wordpress_url: http://blog.malev.com.ar/?p=479
comments: true
categories: 
- title: rails
  slug: rails
  autoslug: rails
- title: Ruby
  slug: ruby
  autoslug: ruby
- title: ruby-core
  slug: ruby-core
  autoslug: ruby-core
tags: 
- title: rails
  slug: rails
  autoslug: rails
- title: Ruby
  slug: ruby
  autoslug: ruby
- title: instalacion
  slug: instalacion
  autoslug: instalacion
- title: entorno
  slug: entorno
  autoslug: entorno
---
{{ page.title }}
================
Este por no es para nada original, pero como tuve que reinstalar todo mi sistema, se me ocurrió aprovechar y sacar un post :) Mañana o cuando pueda voy a hacer uno de mi entorno python.

**Instalar Ruby 1.8.7**
Como Ubuntu te recomienda la versión 1.9.1 y esta todavía no es compatible con muchas gemas, prefiero dejarla de lado hasta más adelante.
Baje el .deb de ruby 1.8.7 de aquí [1].
No se por que el binario luego de la instalación se llama ruby1.8 y está ubicado aquí:
<blockquote>malev@dell:~$ which ruby1.8
/usr/bin/ruby1.8
</blockquote>

Para evitar escribir ruby1.8 cada vez que lo necesito, voy a crear un link simbólico:
<blockquote>malev@dell:~$ sudo ln -s /usr/bin/ruby1.8 /usr/bin/ruby
malev@dell:~$ ruby -v
ruby 1.8.7 (2010-01-10 patchlevel 249) [i486-linux]
</blockquote>

**También instalo irb y rdoc:**
<blockquote>sudo apt-get install irb
sudo apt-get install rdoc1.8
sudo apt-get install ruby1.8-dev
apt-get install libopenssl-ruby
</blockquote>

**Ahora a instalar rubygems**
Bajo el .tgz de [2] lo descomprimimos y corremos:
<blockquote>malev@dell:~/Descargas/rubygems-1.3.7$ sudo ruby setup.rb install
RubyGems 1.3.7 installed

=== 1.3.7 / 2010-05-13
...
------------------------------------------------------------------------------

RubyGems installed the following executables:
/usr/bin/gem1.8
</blockquote>
Otra vez, el binario se guarda con el 1.8 al final. Muy molesto, por lo que también le voy a crear un link simbólico para simplificarme la vida:
<blockquote>
malev@dell:~/Descargas/rubygems-1.3.7$ sudo ln -s /usr/bin/gem1.8 /usr/bin/gem
</blockquote>
Una vez que tenemos gem instalado, podemos instalar todas nuestras gemas favoritas, para ser un poco original, aquí voy a instalar Sinatra y wirble.
<blockquote>
sudo gem install sinatra wirble hirb
</blockquote>
Ya que estoy voy a probar algunas sugerencias de [3] para mejorar un poco la consola IRB. Por lo que todo esto va directo al ~/.irbrc:

{% highlight ruby %}
require 'rubygems'
require 'wirble'
require 'hirb'
Wirble.init
Wirble.colorize
# hirb (active record output format in table)
Hirb::View.enable

# IRB Options
IRB.conf[:AUTO_INDENT] = true
IRB.conf[:SAVE_HISTORY] = 1000
IRB.conf[:EVAL_HISTORY] = 200

# Log to STDOUT if in Rails
if ENV.include?('RAILS_ENV') && !Object.const_defined?('RAILS_DEFAULT_LOGGER')
  require 'logger'
  RAILS_DEFAULT_LOGGER = Logger.new(STDOUT)
  #IRB.conf[:USE_READLINE] = true

  # Display the RAILS ENV in the prompt
  # ie : [Development]>>
  IRB.conf[:PROMPT][:CUSTOM] = {
   :PROMPT_N => "[#{ENV["RAILS_ENV"].capitalize}]>> ",
   :PROMPT_I => "[#{ENV["RAILS_ENV"].capitalize}]>> ",
   :PROMPT_S => nil,
   :PROMPT_C => "?> ",
   :RETURN => "=> %s\n"
   }
  # Set default prompt
  IRB.conf[:PROMPT_MODE] = :CUSTOM
end

# We can also define convenient methods (credits go to thoughtbot)
# def me
#  User.find_by_email("me@gmail.com")
# end
{% endhighlight %}


**MySQL y SQLite:**


<blockquote>sudo apt-get install mysql-server  sqlite3 libmysql-ruby
sudo apt-get install build-essential  libmysqlclient-dev libmysql-ruby libsqlite3-ruby libsqlite3-dev
apt-get install ruby-dev libmysql-ruby libmysqlclient-dev
sudo gem install rails  mysql sqlite3-ruby mongrel
</blockquote>


**RVM: Ruby Version Manager [5]**
Estoy leyendo un libro sobre ruby 1.9 y quiero ir probando algunos ejemplos. RVM es la mejor ( y creo que única ) alternativa para poder instalar varias versiones de Ruby en un solo entorno. Aquí solo sigo las instrucciones de la página. Y una vez terminado, pues:
<blockquote>
rvm install 1.9.2
</blockquote>

**Links:**
[1] http://ns2.canonical.com/es/lucid/i386/ruby1.8/download
[2] http://rubygems.org/pages/download
[3] http://www.tech-angels.fr/post/963080350/improve-irb-and-fix-it-on-mac-os-x
[4] http://tagaholic.me/2009/03/13/hirb-irb-on-the-good-stuff.html
[5] http://rvm.beginrescueend.com/
