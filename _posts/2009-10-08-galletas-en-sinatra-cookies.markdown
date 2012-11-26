--- 
layout: post
title: Galletas en Sinatra (Cookies)
wordpress_id: 50
wordpress_url: http://blog.malev.com.ar/?p=50
comments: true
categories: 
- title: Ruby
  slug: ruby
  autoslug: ruby
- title: sinatra
  slug: sinatra
  autoslug: sinatra
tags: 
- title: cookies
  slug: cookies
  autoslug: cookies
- title: Ruby
  slug: ruby
  autoslug: ruby
- title: sessions
  slug: sessions
  autoslug: sessions
- title: sinatra
  slug: sinatra
  autoslug: sinatra
---
{{ page.title }}
================
¿Qué son estas galletas? Una cookie es un fragmento de información que se almacena en el disco duro del visitante de una página web a través de su navegador (**limitadas a 4k**). Esta información puede ser luego recuperada por el servidor en posteriores visitas.
Se usan por ejemplo para llevar el control de usuarios y así evitar que el usuario tenga que validar sus datos cada vez que accede a una determinada página. Un dato a tener en cuenta es que las cookies no identifican a una persona, sino a una combinación de una computadora y un navegador.
**
Atributos:**
Par nombre / valor: nombre=valor. Caducidad: Una vez que las cookies caducan, dejan de ser enviadas al navegador, es decir, dejan de ser usadas (si no se especifica, el default es cuando se cierra la sesión). Las cookies pueden ser renovadas. Y por último el path y dominio. Estos últimos son los utilizados para reenviar la cookie al servidor.
**Ejemplo1:**
Set-Cookie: name=newvalue; expires=date; path=/; domain= .example.org.
**Ejemplo 2:**
Set-Cookie: RMID=732423sdfs73242; expires=Fri, 31-Dec-2010 23:59:59 GMT; path=/; domain=.example.net

**Casos en que dejan de ser usadas:**
Finaliza la sesión (cerrar el navegador, a menos que sea declarada persistente). Ha expirado. La cookie ha sido borrada.
**Nota:** las cookies con la fecha de expiración especificada son las denomiadas persistentes.

**Cookies en sinatra**
<blockquote>response.set_cookie("foo", "bar")
request.set_cookie("foo", {:value => "bar", :expiration => Time.today + 94608000}
set_cookie("thing", { :value => "thing2",
                      :domain => “blog.malev.com.ar”,
                      :path => “/”,
                      :expires => Time.today,
                      :secure => true,
                      :httponly => true } )</blockquote>
**Leyendo las cookies: **
<blockquote>cookies["foo"] # => "bar"</blockquote>
**Actualizado**

**Otra forma:**
<blockquote>enable :sessions
get '/' do
  session["user"] ||= “malev”
  haml :index
end</blockquote>

Actualmente Sinatra no tiene otra forma de implentar sesiones o sessions. Pero según nos dice Julio [3], en un comentario en este mismo post, _"pero, como Sinatra es basicamente una capa de abstracción que se encuentra por sobre Rack, ademas podes usar directamente los middlewares existentes (como el soporte para MemCache, Pool, Cookies) en Rack o, inclusive, crear uno propio"_. Christopher, en una respuesta a una consulta mía en la lista de correos de Sinatra me comentó que escribió [este](http://www.gittr.com/index.php/archive/using-alternate-session-stores-with-sinatra/) artículo, donde detalla como hacer uso de Rack para implementar formas alternativas de sessions.

**Referencias:**
[1] [gittr](http://www.gittr.com/index.php/archive/sinatra-cookie-handling-in-0-9-4/)
[2] [Sinatra API](http://www.sinatrarb.com/api/index.html)
[3]  [Julio Javier Cicchelli](http://rubylearning.com/blog/2009/09/30/cookie-based-sessions-in-sinatra/comment-page-1/#comment-119528)
