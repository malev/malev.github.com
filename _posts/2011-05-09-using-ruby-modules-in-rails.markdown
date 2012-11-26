--- 
layout: post
title: Using Ruby modules in Rails
wordpress_id: 811
wordpress_url: http://blog.malev.com.ar/?p=811
comments: true
categories: 
- title: rails
  slug: rails
  autoslug: rails
- title: ActiveRecord
  slug: activerecord
  autoslug: activerecord
- title: Ruby
  slug: ruby
  autoslug: ruby
tags: 
- title: modules
  slug: modules
  autoslug: modules
- title: patterns
  slug: patterns
  autoslug: patterns
- title: models
  slug: models
  autoslug: models
- title: Ruby
  slug: ruby
  autoslug: ruby
- title: rails
  slug: rails
  autoslug: rails
- title: include
  slug: include
  autoslug: include
- title: extend
  slug: extend
  autoslug: extend
---
{{ page.title }}
================
El título quizás no sea muy feliz, porque la verdad que es que todo el tiempo estamos usando módulos en Rails. De hecho en Rails 3 podríamos decir que el génesis de cada aplicación rails está contenida en un módulo:

{% highlight ruby %}
# config/application.rb
module Grouppet
  class Application < Rails::Application
    # ...
  end
end
{% endhighlight %}

Volviendo a los módulos, ya he hablado de ellos en [1], y en otro post, que ya fue corregido por el [1], así que no vale la pena citarlo aquí.

**¿Qué es un módulo en Ruby?**
Son modos de agrupamiento de métodos, clases y constantes. Tienen dos beneficios: proveen un namespace previniendo choques de nombres y soportan mezclado o mixins de una manera fácil [2]. En contraste con las clases, los módulos no pueden: ser instanciados o tener subclases[3]. Por último y aprovechando al lenguaje, un módulo es un objeto. 
Aquí, un ejemplo robado de [4]:

{% highlight ruby %}
module Trig
  PI = 3.1416
  # class methods
  def Trig.sin(x)
    puts "sin(#{x})"
  end
  def Trig.cos(x)
    # también se podría definir como self.cos(x)
    puts "cos(#{x})"
  end
end
Trig::PI # => 3.1416
Trig.cos(90) #=> cos(90)
{% endhighlight %}

**Modules mixin (mezcla de módulos)**
Los módulos pueden ser incluidos (usando **include**) dentro de las clases y con esto los mezclamos (o al menos eso dice Dave Thomas). Ya he hablado sobre esto en [1], el ejemplo posterior sería como un repaso.

{% highlight ruby %}
module Trig
  def sin(x)
    puts "sin(#{x})"
  end
end

class Matematica
  include Trig
end
mat = Matematica.new
mat.sin(90) # => sin(90)
{% endhighlight %}

Podemos hacer algo similar usando la palabra clave **extend** pero para agregar métodos de clase:

{% highlight ruby %}
class Matematica2
  extend Trig
end
Matematica.sin(90) # => sin(90)
{% endhighlight %}

**In a nutshell**
Por un lado lo módulos nos permiten agregar funcionalidad a nuestras clases, algunos los clasifican como la forma que tiene Ruby de soportar herencia múltiple (yo no me siento calificado para hacer esa aseguración). Pero hay un poco más, el código de los módulos que incluimos puede interactuar con el código de nuestra clase. Eso permite hacer cosas fantásticas como: (el ejemplo es de [5])

{% highlight ruby %}
class SizeMatters
  include Comparable
  attr :str
  def <=>(anOther)
    str.size <=> anOther.str.size
  end
  def initialize(str)
    @str = str
  end
  def inspect
    @str
  end
end

s1 = SizeMatters.new("Z")
s2 = SizeMatters.new("YY")
s3 = SizeMatters.new("XXX")
s4 = SizeMatters.new("WWWW")
s5 = SizeMatters.new("VVVVV")

s1 < s2                       #=> true
s4.between?(s1, s3)           #=> false
s4.between?(s3, s5)           #=> true
[ s3, s2, s5, s4, s1 ].sort   #=> [Z, YY, XXX, WWWW, VVVVV]
{% endhighlight %}

Bien, ¿qué hicimos aquí? Al incluir el módulo **Comparable**[5] en nuestra clase le dimos a nuestra clase la capacidad de ordenarse (método **sort**). El único detalle es que nuestra clase debe poder responder al método "<=>", definido en el interior de la misma. ¿Cómo debe responder ese método? Los objetos de nuestra clase van a ser mayores, menores o iguales de acuerdo a cómo queremos nosotros que se comporten. El método <=> debe devolver 1 si el primer elemento es mayor, -1 si es menor o 0 si son iguales.
También podríamos incluir Eumerable[6] y hacer que nuestros objetos sean iterables por ejemplo.

**Volviendo al mundo rails**
Mi curiosidad por los módulos viene desde que uso clearance como gema de autenticación en mis proyectos. Clearance incluye métodos en un modelo y en los controladores:

{% highlight ruby %}
class User < ActiveRecord::Base
  include Clearance::User
end
class ApplicationController < ActionController::Base
  include Clearance::Authentication
end
{% endhighlight %}


El código fuente de estos módulos se puede navegar en github [7] y [8], en ambos casos podemos ver la inclusión de métodos de clase y de instancia y cómo el buen uso de módulos nos permite (en realidad a los muchachos de thoughtbot) generar excelentes plugins para nuestras aplicaciones rails.
Ahora me pueden decir: yo no escribo gemas ni plugis. ¿Para qué quiero los módulos en rails? La gente de Stac escribió un artículo [9] sobre como organizar modelos cuando estos empiezan a crecer de manera desenfrenada y empiezan a convertirse un desastre. El artículo expone como usar **mixins** para poder organizar nuestros métodos por funcionalidad y así tener código mucho más legible y ordenado.

{% highlight ruby %}
# lib/models/user/friendship_methods.rb
module Models
  module User
    module FriendshipMethods
      extend ActiveSupport::Concern
      included do
        has_many :friendships, :foreign_key => :initiator_id
      end
      module InstanceMethods
        def befriend(other)
          self.friend << other
        end
      end
    end
  end
end

# app/models/user.rb
class User < ActiveRecord::Base
  include Models::User::FriendshipMethods
end
{% endhighlight %}

Explicar que hace **ActiveSupport::Concern** escapa la idea de este post, pero pueden buscar info en [10], aunque creo que el código y los comentarios del propio método **concern** se explican por si solo [11]. Recomendación: leer el código fuente a veces está bueno!
También podríamos usar módulos para compartir código entre modelos. Un buen ejemplo sería un sitio que admite comentarios en varios modelos (post, page, etc). Por un lado podríamos usar herencia polimórfica, pero ahí tendríamos que guardar todo en la misma tabla, por otro lado podríamos definir los métodos comunes en un módulo e importarlo en los modelos en donde los necesitamos.

**Resources**

* [1] http://blog.malev.com.ar/2010/11/30/include-in-ruby/
* [2] Programming Ruby - Dave Thomas
* [3] http://www.rubyist.net/~slagell/ruby/modules.html
* [4] http://rubylearning.com/satishtalim/modules_mixins.html
* [5] http://www.ruby-doc.org/core/classes/Comparable.html
* [6] http://www.ruby-doc.org/core/classes/Enumerable.html
* [7] https://github.com/thoughtbot/clearance/blob/master/lib/clearance/authentication.rb
* [8] https://github.com/thoughtbot/clearance/blob/master/lib/clearance/user.rb
* [9] http://wearestac.com/blog/2011/05/organise_your_models
* [10] http://en.ihower.tw/post/454873995/rails3-activesupport-concern
* [11] https://github.com/rails/rails/blob/master/activesupport/lib/active_support/concern.rb
