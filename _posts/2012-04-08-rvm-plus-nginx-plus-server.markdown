--- 
layout: post
title: RVM plus Nginx plus Server
comments: true
categories: 
- title: server
  slug: server
  autoslug: server
- title: rails
  slug: rails
  autoslug: rails
- title: Ruby
  slug: ruby
  autoslug: ruby
tags:
- title: server
  slug: server
  autoslug: server
- title: deploy
  slug: deploy
  autoslug: deploy
- title: nginx
  slug: nginx
  autoslug: nginx
- title: rvm
  slug: rvm
  autoslug: rvm
---
{{ page.title }}
================
Al fin pude hacer andar mi servidor con RVM y Nginx! Aquí voy a dejar una receta por si tengo que volver a hacerlo (espero que no en el mediano plazo al menos).
Vamos a necesitar algunas cosas antes de empezar: **curl**, **git**, muchos paquetes de desarrollo y un usuario que pueda usar sudo.

## Empezamos con el usuario

> sudo adduser malev

> sudo passwd malev

> sudo visudo

y agregamos al final:

> malev ALL=(ALL) ALL

y ahora nos transformamos en nuestro nuevo usuario:

> su malev

## Preparandonos para instalar
Primero necesitamos instalar RVM en modo multiusuario [1], para esto necesitamos **curl** y **git**:

> sudo yum install git-core curl

e instalamos RVM:

> sudo bash -s stable < <(curl -s https://raw.github.com/wayneeseguin/rvm/master/binscripts/rvm-installer)

Esto instalara RVM en */usr/local/lib/rvm* y nos generará archivos en: */etc/rvmrc* y */usr/local/rvm*. También necesitamos configurar nuestros *.bashrc* y el esqueleto para los futuros usuarios, así que en los archivos: */root/.bashrc* y */etc/skel/.bashrc* agregamos:

{% highlight bash %}
if groups | grep -q rvm ; then
  source "/usr/local/lib/rvm"
fi
{% endhighlight %}

Ahora también tenemos un grupo llamado **RVM** y necesitamos agregar nuestro usuario al mismo:

> adduser malev rvm

Otra buena práctica es definir el **environment** para todas nuestras aplicaciones Rails. En el archivo */etc/environment* agregamos:

> RAILS_ENV=production

Aquí yo me deslogueo y vuelvo a entrar, pero esta vez como malev y al tipear:

> type rvm | head -n1

debería ver: **rvm is a function**, lo cual me indica que RVM está correctamente instalado.

## Avanzando

Yo estoy usando una distro basada en CentOS, así que para instalar las librerías de desarrollo tengo que hacer:

> sudo yum groupinstall "Development Tools"

> sudo yum install kernel-devel kernel-headers

> sudo yum install openssl-devel libcurl-devel ImageMagick httpd-devel ruby-libs zlib-devel libjpeg-devel giflib-devel readline-devel

> sudo yum -y install libpcap libpcap-devel libnet libnet-devel pcre pcre-devel gcc automake autoconf libtool make gcc-c++ libyaml libyaml-devel zlib zlib-devel pkgconfig ruby-devel libxml2 libxml2-devel libxslt libxslt-devel

**Nota:** Si estas usando Ubuntu, hay MUCHA info sobre que instalar en este paso, pero en ppio bastaría con esto:

> sudo apt-get install build-essential ruby-full libmagickcore-dev imagemagick libxml2-dev libxslt1-dev git-core ruby-devel libxml2 libxml2-devel libxslt libxslt-devel

## Instalando Ruby

> rvm pkg install readline 

> rvm pkg install zlib

> rvm pkg install openssl

> rvm install 1.9.2

> rvm use --default 1.9.2

## Instalando Passenger

> gem install passenger

> rvmsudo passenger-install-nginx-module

Si lo instalaste en el directorio por defecto (*/opt/nginx*), clonate este [gist](https://gist.github.com/1918419) y copia el archivo **nginx** en */etc/init.d* con permisos de ejecución.

## Configurar Nginx

Buscar el archivo */opt/nginx/conf/nginx.conf* que debería verse más o menos como este [gist](https://gist.github.com/1918436). Las líneas más importantes son la 1 donde digo con qué usuario ejecutar nginx y la 22 donde establezco dónde ubicaré el resto de los archivos de configuración.

## Instalar nuestra app

En mi caso voy a instalar una app llamada infofund. Y voy a deployar con *capistrano*, pero las explicaciones de cómo usar *capistrano* vendrán en otro post. De momento les dejo el archivo */opt/nginx/conf/conf.d/infofund.conf* cuyo contenido está en este [gist](https://gist.github.com/2339504).

En principio y como para probar pueden bajarse el contenido del proyecto en el directorio */home/malev/sites/infofund/current*, crear el Gemset e instalar todas las gemas con nuestro amigo **bundle** para luego iniciar **nginx**:

> sudo /etc/init.d/nginx start

y ya debería estar todo andando!

Resources
---------

1. https://rvm.beginrescueend.com/rvm/install/#explained
2. http://thekindofme.wordpress.com/2010/10/24/rails-3-on-ubuntu-10-10-with-rvm-passenger-and-nginx/
3. https://rvm.beginrescueend.com/rvm/install/#explained
4. http://ikennaokpala.wordpress.com/2011/12/27/deployment-strategy-for-rails-passenger-and-nginx-server-with-multiple-virtual-hosts/
5. http://blog.ninjahideout.com/posts/a-guide-to-a-nginx-passenger-and-rvm-server
6. http://kris.me.uk/2010/08/30/rails3-hosting-all-in-one.html

