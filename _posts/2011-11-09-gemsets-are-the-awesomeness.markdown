--- 
layout: post
title: gemsets are the awesomeness
wordpress_id: 939
wordpress_url: http://blog.malev.com.ar/?p=939
comments: true
categories: 
- title: gems
  slug: gems
  autoslug: gems
- title: rails
  slug: rails
  autoslug: rails
- title: Ruby
  slug: ruby
  autoslug: ruby
tags: 
- title: gems
  slug: gems
  autoslug: gems
- title: rails
  slug: rails
  autoslug: rails
- title: Ruby
  slug: ruby
  autoslug: ruby
- title: rvm
  slug: rvm
  autoslug: rvm
- title: gemset
  slug: gemset
  autoslug: gemset
---
{{ page.title }}
================
Cada vez que tenía que usar un proyecto con python, empezaba creando un ambiente particular con virtualenv. En ruby, en principio no podía hacer eso. Después conocí RVM y trabajaba con varias versiones de Ruby en mi PC, sobre todo para trabajar algunos proyectos legacy. Sin embargo, tenía un popurrí de gemas instaladas que ya empezaban a conflictuarse entre sí. Apareció bundler y su magia de **bundle exec** para solucionar muchos problemas (nota: en futuras versiones de bundler esto ya no va a hacer falta). En fin, la ensalada de gemas seguí ahí, hasta que por fin conocí **gemset**.

[![](/images/posts/2011/11/logo.png "rvm")](http://blog.malev.com.ar/wp-content/uploads/2011/11/logo.png)

Mi workflow es más o menos así:

{% highlight ruby %}
➜  ~  cd code 
➜  code  mkdir proyecto_nuevo
➜  code  cd proyecto_nuevo 
➜  proyecto_nuevo  rvm use malev@proyecto_nuevo --rvmrc --create
Using /home/malev/.rvm/gems/ruby-1.9.2-p290 with gemset proyecto_nuevo
WARN: .rvmrc is not empty, moving aside to preserve.
➜  proyecto_nuevo  rvm gemset list

gemsets for ruby-1.9.2-p290 (found in /home/malev/.rvm/gems/ruby-1.9.2-p290)
   global
   mobilenews
=> proyecto_nuevo
   webui

➜  proyecto_nuevo  
{% endhighlight %}

Es decir, voy a mi directorio donde guardo mis proyectos / experimentos, creo un directorio, creo un nuevo gemset y listo! Todas las **gems** que instale en ese **gemset** van a estar aisladas del resto.
Lo interesante es que gracias a la magia de **rvm**, cada vez que entre en el directorio de mi proyecto nuevo, **rvm** me va a cambiar al **gemset** ahí usado:

{% highlight ruby %}
➜  code  pwd
/home/malev/code
➜  code  rvm gemset list

gemsets for ruby-1.9.2-p290 (found in /home/malev/.rvm/gems/ruby-1.9.2-p290)
=> global
   mobilenews
   proyecto_nuevo
   webui

➜  code  cd proyecto_nuevo 
=========================================================
= NOTICE                                                                     =
=========================================================
= RVM has encountered a new or modified .rvmrc file in the current directory =
= This is a shell script and therefore may contain any shell commands.       =
=                                                                            =
= Examine the contents of this file carefully to be sure the contents are    =
= safe before trusting it! ( Choose v[iew] below to view the contents )      =
=========================================================
Do you wish to trust this .rvmrc file? (/home/malev/code/proyecto_nuevo/.rvmrc)
y[es], n[o], v[iew], c[ancel]> yes
➜  proyecto_nuevo  rvm gemset list

gemsets for ruby-1.9.2-p290 (found in /home/malev/.rvm/gems/ruby-1.9.2-p290)
   global
   mobilenews
=> proyecto_nuevo
   webui

➜  proyecto_nuevo  
{% endhighlight %}

_Si no tienes rvm instalado, corred e instaladlo por favor!_
