--- 
layout: post
title: Bindings and Ruby
wordpress_id: 636
wordpress_url: http://blog.malev.com.ar/?p=636
comments: true
categories: 
- title: metaprogramacion
  slug: metaprogramacion
  autoslug: metaprogramacion
- title: ruby-core
  slug: ruby-core
  autoslug: ruby-core
- title: Ruby
  slug: ruby
  autoslug: ruby
- title: objetos
  slug: objetos
  autoslug: objetos
tags: 
- title: blocks
  slug: blocks
  autoslug: blocks
- title: objects
  slug: objects
  autoslug: objects
- title: binding
  slug: binding
  autoslug: binding
- title: programming
  slug: programming
  autoslug: programming
- title: Ruby
  slug: ruby
  autoslug: ruby
- title: metaprogramming
  slug: metaprogramming
  autoslug: metaprogramming
- title: eval
  slug: eval
  autoslug: eval
---
{{ page.title }}
================
**La definición:**
_Un **binding** es una “ligadura” o referencia a otro símbolo más largo y complicado, y que se usa frecuentemente._ [1]
**Volviendo a la práctica**
En Ruby hay una clase: **Binding**. Las instancias de la misma encapsulan el contexto de la ejecución en un determinado punto del código y retienen este contexto para un uso posterior. Entiéndase por contexto: self, variables locales (incluyendo variables de instancia), bloques asociados y la pila de return (return stack). [2][3]
Los bindings se crean con el método: Kernel.binding y como casi todo en Ruby, es un objeto. [4]
**¿Dónde usamos bindings?**
Podemos usar estos objetos binding como segundo argumento del método Kernel.**eval**, para establecer un ambiente de evaluación.[5]
Recordemos qué es eval: Eval toma una cadena y la ejecuta como código Ruby:

{% highlight ruby %}
eval "puts 'hola mundo!'"
{% endhighlight %}

Ahora le vamos a pasar un binding como segundo parámetro:

{% highlight ruby %}
class MiClase
  def initialize
    @vi = "variable de instancia"
  end
  def metodo(param)
    variable = "something"
    binding
  end # El método retorna el binding interno al método
end
# Generando el binding
>> mc = MiClase.new
=> #<MiClase:0xb6e5c804 @vi="variable de instancia">
>> b = mc.metodo(2)
=> #>> b = mc.metodo(2) { "un bloque" }
=> #>> eval "puts variable, param, yield, self.class.to_s, @vi", b
something
2
un bloque
MiClase
variable de instancia
=> nil
{% endhighlight %}

La clase Binding nos provee de un método: clone, cuya documentación es la siguiente:
<blockquote>MISSING: documentation</blockquote>
Personalmente no me queda muy claro :)
**Pero vamos a hacer algunos experimentos:**

{% highlight ruby %}
# Retomando de los ejemplos anteriores
>> c = b.clone
=> #>> b
=> #>> # 2 Objetos distintos según su ID,
>> # pero y las variables a las que hacen referencia?
>> eval "variable = 3", b
=> 3
>> eval "puts variable", c
3
=> nil
{% endhighlight %}

Esto está interesante, no se donde lo usaría, pero sigue siendo interesante.
**Usando bindings en algo cool**
Ruby asocia un binding a cada bloque que crea. Esto nos permite por ejemplo definir una suerte de métodos dinámicos:

{% highlight ruby %}
def n_times(n)
  lambda { |var| var * n}
end
two_times = n_times(2) # => <Proc:0xb6e3eb60@(irb):36>
# se crea el binding y guarda n = 2
puts two_times.call(3) # => 6
{% endhighlight %}

Este post vino con la idea de acompañar al del bloques y permitir un mayor entendimiento de estos temas. Espero pronto encontrar algún ejemplo concreto donde se usen estas técnicas para poder estudiarlas en más profundidad.

**Referencias**
[1] [http://en.wikipedia.org/wiki/Binding](http://en.wikipedia.org/wiki/Binding)
[2] [http://ruby-doc.org/ruby-1.9/classes/Binding.html](http://ruby-doc.org/ruby-1.9/classes/Binding.html)
[3] The Ruby Object Model - Dave Thomas
[4] [http://ruby-doc.org/ruby-1.9/classes/Kernel.html#M002636](http://ruby-doc.org/ruby-1.9/classes/Kernel.html#M002636)
[5] [http://ruby-doc.org/ruby-1.9/classes/Kernel.html#M002619](http://ruby-doc.org/ruby-1.9/classes/Kernel.html#M002619)
