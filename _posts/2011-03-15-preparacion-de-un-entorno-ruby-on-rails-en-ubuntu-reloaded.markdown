--- 
layout: post
title: "Preparaci\xC3\xB3n de un entorno Ruby on Rails en Ubuntu - Reloaded"
wordpress_id: 750
wordpress_url: http://blog.malev.com.ar/?p=750
comments: true
categories: 
- title: Programacion
  slug: programacion
  autoslug: programacion
- title: rails
  slug: rails
  autoslug: rails
- title: Ruby
  slug: ruby
  autoslug: ruby
- title: Ubuntu
  slug: ubuntu
  autoslug: ubuntu
tags: 
- title: rails
  slug: rails
  autoslug: rails
- title: Ruby
  slug: ruby
  autoslug: ruby
- title: Ubuntu
  slug: ubuntu
  autoslug: ubuntu
- title: instalacion
  slug: instalacion
  autoslug: instalacion
- title: rvm
  slug: rvm
  autoslug: rvm
---
{{ page.title }}
================
**Aclaración:** Por alguna razón wordpress no me deja poner dos guiones consecutivos y los reemplaza por un único guión un poquito más largo. Cada vez que vean este guión extraño recomiendo cambiarlo por dos guiones medios para que funcione.

Hace un tiempo prepare un [tutorial](http://blog.malev.com.ar/2010/08/19/armando-un-entorno-rails-en-ubuntu-10-04/) sobre como instalar Ruby en Ubuntu 10.04, anduvo bien, pero hoy se quedó un poco viejito. Hoy en día en la comunidad **se desaconseja instalar ruby desde los repositorios de nuestras distros**. Según Ryan Bigg, es: _"is out-dated and leads to major headaches"_ [1]. La posta! es instalarlo desde RVM.
Por supuesto seguimos necesitando de varias librerías y cosillas para que nuestro Ruby ande a la perfección:
<blockquote>sudo apt-get install build-essential git-core curl</blockquote>
_Notese que también estamos instalando GIT_

Una vez hecho esto tiramos en nuestra terminal lo siguiente:
<blockquote>
bash < <(curl -s https://rvm.beginrescueend.com/install/rvm)
echo '[[ -s "$HOME/.rvm/scripts/rvm" ]] && source "$HOME/.rvm/scripts/rvm"' >> ~/.bashrc 
</blockquote>

Ahí mismo, RVM nos recomien instalar más cosas:
<blockquote>
sudo apt-get install build-essential bison openssl libreadline6 libreadline6-dev curl git-core zlib1g
zlib1g-dev libssl-dev libyaml-dev libsqlite3-0 libsqlite3-dev sqlite3 libxml2-dev libxslt-dev autoconf
</blockquote>
Listo, **tenemos RVM instalado perfectamente**, cerramos y volvemos a abrir la terminal (para recargar el .bashrc) y vamos por Ruby:
<blockquote>
rvm install 1.9.2


rvm install 1.8.7


# si eso no funciona:


rvm package install zlib


rvm remove 1.9.2


# tener cuidado con los guiones aquí abajo antes del with


rvm install 1.9.2 -C --with-zlib-dir=$rvm_path/usr
</blockquote>

Y elegimos uno para que funcione por defecto:

<blockquote>
# cuidado con los guiones aquí también

rvm --default use 1.9.2

</blockquote>
**Solo si usas versiones de Rails menores a 3:**
Hay un pequeño problema entre Ruby Gems y rails 2.3.x. Una de las soluciones es hacer downgrade de ruby gems:
En la consola:
<blockquote>
gem install rubygems-update -v 1.4.2 

update_rubygems _1.4.2_ 

</blockquote>
Y listo!

Volviendo con nuestro entorno, vamos a **instalar sqlite y MySQL**:
<blockquote>
sudo apt-get install mysql-server sqlite3 

sudo apt-get install libmysqlclient-dev libmysqlclient-dev
</blockquote>
Le damos un poco de color a nuestra consola:
<blockquote>
gem install wirble hirb
</blockquote>

y añadimos esto a nuestro ~/.ircrb  (corregido gracias a Pedro)

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
{% endhighlight %}


Por último instalamos rails y estamos listos:
<blockquote>
gem install rails
</blockquote>
Espero sea útil!
**Resources**
[1] [Ryan Bigg](http://ryanbigg.com/)
[2] [Peter Cooper video tutorial](http://www.rubyinside.com/how-to-install-ruby-1-9-2-and-rails-3-0-on-ubuntu-10-10-4148.html)
