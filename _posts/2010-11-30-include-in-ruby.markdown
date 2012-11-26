--- 
layout: post
title: Include in Ruby
wordpress_id: 652
wordpress_url: http://blog.malev.com.ar/?p=652
comments: true
categories: 
- title: objetos
  slug: objetos
  autoslug: objetos
- title: Programacion
  slug: programacion
  autoslug: programacion
- title: Ruby
  slug: ruby
  autoslug: ruby
- title: ruby-core
  slug: ruby-core
  autoslug: ruby-core
tags: 
- title: include
  slug: include
  autoslug: include
- title: Ruby
  slug: ruby
  autoslug: ruby
- title: programming
  slug: programming
  autoslug: programming
- title: class
  slug: class
  autoslug: class
- title: methods
  slug: methods
  autoslug: methods
---
{{ page.title }}
================
Hace un tiempo, cuando recién empezaba con Ruby escribí un post sobre la diferencia entre **include** y **require** en ruby[1]. Hoy estaba discutiendo acerca de ese tema con un compañero de la oficina y se me ocurrió repasarlo y de paso, hacerlo un poquito más claro con un ejemplo:

{% highlight ruby %}
module MyModule
  def module_method
    puts "esto deberia aparecer"
  end
  
  def module_method_two
    puts "esto no deberia aparecer"
  end
end

class Clase

  include MyModule

  def method_from_a_class
    puts "desde la clase"
  end
  
  def module_method_two
    puts "este metodo sobreescribe al del modulo"
  end
end

objeto = Clase.new
objeto.method_from_a_class
objeto.module_method_two
objeto.module_method
#malev@SERVER ~$ ruby experi.rb
#desde la clase
#este metodo sobreescribe al del modulo
#esto deberia aparecer
{% endhighlight %}

Esto es lo mismo que hace la clase **Array**[2] al incluir **Ennumerable**[3]. Y nos permite acceder a métodos como first o find que **no **están definidos en la clase Array pero sí en el módulo Ennumerable. Por otro lado, nos permite incluir el módulo ennumerable y hacer usos de muchos métodos en alguna clase nuestra en la que lo podamos necesitar.
**Resources**
[1] [http://blog.malev.com.ar/2009/08/26/%C2%BFcual-es-la-diferencia-entre-include-y-require-in-ruby/](http://blog.malev.com.ar/2009/08/26/%C2%BFcual-es-la-diferencia-entre-include-y-require-in-ruby/)
[2] [http://ruby-doc.org/ruby-1.9/classes/Array.html](http://ruby-doc.org/ruby-1.9/classes/Array.html)
[3] [ http://ruby-doc.org/ruby-1.9/classes/Enumerable.html](http://ruby-doc.org/ruby-1.9/classes/Enumerable.html)
