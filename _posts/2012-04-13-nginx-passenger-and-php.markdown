---
layout: post
title: Nginx, passenger and PHP
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
- title: PHP
  slug: php
  autoslug: php
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
- title: PHP
  slug: php
  autoslug: php
- title: passenger
  slug: passenger
  autoslug: passenger
---
{{ page.title }}
================
Suele pasar, solemos llenar de cosas nuestros VPS, y bueno, tuvo que pasar, necesitaba un Wordpress en un VPS y debo admitir que extrañé Apache por unos minutos. Luego de googlear mucho encontré esta solución:

## Instalamos algunas cositas:

> sudo yum -y install mysql-server php-fpm php-mysql php-pecl-apc

Una buena práctica es ejecutar **/usr/bin/mysql_secure_installation** para dejar nuestro MySQL en perfectas condiciones.

En Ubuntu podríamos hacer:

> sudo apt-get install php5-cli php5-cgi php5-common php5-curl php5-dev php5-gd php5-imagick php5-mcrypt php5-mhash

> sudo apt-get install php5-mysql libmysqld-dev

## Instalando spawn-fcgi

**spawn-fcgi**[1] es un herramienta del proyecto Lighttp que nos ayuda a preparar el ambiente para FastCGI. Tenemos que hacerlo amigo de **nginx**.

> cd /tmp

> wget http://www.lighttpd.net/download/spawn-fcgi-1.6.3.tar.gz

> tar -xvzf spawn-fcgi-1.6.3.tar.gz

> cd spawn-fcgi-1.6.3

> ./configure --prefix=/usr

> make

> sudo make install

> sudo nano /usr/bin/php5-fcgi

Y le pegamos esto:

> /usr/bin/spawn-fcgi -f /usr/bin/php-cgi -a 127.0.0.1 -p 49232 -C 2 -P /var/run/fastcgi-php.pid

También vamos a necesitar crear un archivo de inicio para spawn-fcgi. Creamos **/etc/init.d/php5-fcgi** y le pegamos el contenido de [este gist](https://gist.github.com/2373780).

Cambiamos algunos permisos:

> sudo chmod +x /usr/bin/php5-fcgi

> sudo chmod +x /etc/init.d/php5-fcgi

> sudo update-rc.d php5-fcgi defaults

y ahora a lo más (sarcamo on) divertido (sarcasmo off), recompilamos **nginx**:

Bajamos el código fuente:

> rvmsudo passenger-install-nginx-module

Elegimos la opción 2 y le pasamos estos parámetros extras:

> --with-http_ssl_module --with-http_dav_module --with-http_gzip_static_module --http-fastcgi-temp-path=/var/lib/nginx/fastcgi --http-proxy-temp-path=/var/lib/nginx/proxy --http-client-body-temp-path=/var/lib/nginx/body --with-http_stub_status_module

## Configurando un VirtualHost

[Aquí](https://gist.github.com/2373815) les dejo mi archivo de configuración de **nginx**. Por supuesto lo deben guardar en un lugar donde nginx lo pueda encontrar. Si siguieron mi [post anterior](http://blog.malev.com.ar/rvm-plus-nginx-plus-server/), pueden colocarlo en: **/opt/nginx/conf/conf.d/mycoolblog.conf**.

## Reiniciamos y listo el pollo!

> sudo mkdir /var/lib/nginx/

> sudo /etc/init.d/php5-fcgi start

> sudo /etc/init.d/nginx restart

## Base de datos

Lo más seguro es que tengan que crear una base de datos y un usuario por ejemplo para un Wordpress, aquí los pasos:

Ejecutamos:

> mysql -u root -p
{% highlight sql %}
CREATE DATABASE database_name;
GRANT ALL PRIVILEGES ON database_name.* TO "user" "localhost" IDENTIFIED BY "password";
FLUSH PRIVILEGES;
EXIT
{% endhighlight %}

## Resources

* [1] http://cgit.stbuehler.de/gitosis/spawn-fcgi/about/
* [2] codex.wordpress.org/Installing_WordPress
* [3] http://internetmodulation.com/2011/01/11/install-wordpress-inside-passenger-app.html.html
