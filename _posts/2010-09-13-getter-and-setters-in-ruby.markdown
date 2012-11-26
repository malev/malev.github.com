--- 
layout: post
title: getter and setters in ruby
wordpress_id: 308
wordpress_url: http://blog.malev.com.ar/?p=308
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
- title: Ruby
  slug: ruby
  autoslug: ruby
- title: attr_reader
  slug: attr_reader
  autoslug: attr_reader
- title: attr_accessor
  slug: attr_accessor
  autoslug: attr_accessor
- title: attr_writer
  slug: attr_writer
  autoslug: attr_writer
- title: class
  slug: class
  autoslug: class
---
{{ page.title }}
================
Una de las primeras definiciones de que es un objeto que escuche fue:
Un objeto se define como la unidad que en tiempo de ejecución realiza las tareas de un programa. Cada objeto es capaz de recibir mensajes, procesar datos y enviar mensajes. Los objetos tienen 3 características: identidad, comportamiento y estado.
No voy a discutir sobre la definición de los objetos este post. **Me voy a enfocar en la última característica: el estado, que viene dado por los atributos de un objeto**. Vamos a definir una clase con algunos atributos _"old school"_:

{% highlight ruby %}
class Coordinates
  def set_x(x=0)
    @x = x
  end
  
  def set_y(y=0)
    @y = y
  end
  
  def get_x
    return @x
  end
  
  def get_y
   return @y
  end
end

coord = Coordinates.new()
x = coord.set_x(9)
y = coord.set_y(10)
puts "#{x}, #{y}"
{% endhighlight %}

El código funciona, pero no se ve muy "lindo", como diría Roberto en [1], _eso es C++ disfrazado de Ruby_ (el en realidad hablaría de python, pero no creo que se enoje). Usando un poquito más las ventajas que nos ofrece ruby, podríamos, mejorar los métodos "set_x" y "set_y" usando la característica de Ruby de dejarnos usar el signo "=" en el nombre del método, más o menos así: **"get_x="** y **"get_y="**, como para tener un código mucho más legible. Pero aprovechando un poco más la magia de ruby, vamos a omitir los métodos set_x y set_y:

{% highlight ruby %}
class Coordinates2
  def initialize(x=0, y=0)
    @x, @y = x, y
  end

  def get_x
    return @x
  end
  
  def get_y
   return @y
  end
end  

coord2 = Coordinates2.new(4, 6)
puts "#{coord2.get_x}, #{coord2.get_y}"
{% endhighlight %}

Mejoró un poco, pero todavía le falta bastante. Por supuesto, podríamos omitir la keyword "**return**" y dejar solo el "@x" o "@y" ya que ruby devuelve por defecto el última código ejecutado en cada método. Igual sigue siendo mucho código para tan poco.
Ruby nos ofrece un lindo atajo: "**attr_reader**":

{% highlight ruby %}
class Coordinates3
  attr_reader :y, :x
  def initialize(x=0, y=0)
    @x, @y = x, y
  end
end

coord3 = Coordinates3.new(2,8)
puts "#{coord3.x}, #{coord3.y}"
{% endhighlight %}

Como vemos estamos usando símbolos [2] para indicar que atributos pueden ser leidos desde el exterior. Ahora que pasa si en algún momento de la ejecución también queremos cambiar alguno de estos atributos?. Una solución es definir un método de escritura como en nuestro primer ejemplo o podemos usar el hermano de **attr_reader**: **attr_accessor**:

{% highlight ruby %}
class Coordinates4
  attr_accessor :y, :x
  def initialize(x=0, y=0)
    @x, @y = x, y
  end
end

coord4 = Coordinates4.new(2, 3)
puts "#{coord4.x}, #{coord4.y}"
coord4.y = 9
coord4.x = 1
puts "#{coord4.x}, #{coord4.y}"
{% endhighlight %}


Y por último vemos: **attr_writer**, que nos deja escribir un atributo pero no leerlo:

{% highlight ruby %}
class Coordinates5
  attr_accessor :x
  attr_writer :y
  def initialize(x=0, y=0)
    @x, @y = x, y
  end
end
coord5 = Coordinates5.new(8, 9)
coord5.x = 5
coord5.y = 10
# this last command fails
puts coord5.y
{% endhighlight %}

Listo por ahora. En otro post voy a cubrir: **virtual attributes**, **attr_accessible** y **attr_protected**. Los últimos 2 exclusivos de Rails.

[1] http://nomuerde.netmanagers.com.ar/1.html
[2] http://blog.malev.com.ar/2009/10/02/los-simbolos-en-ruby-symbol/
