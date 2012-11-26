--- 
layout: post
title: "El patr\xC3\xB3n method_missing"
wordpress_id: 726
wordpress_url: http://blog.malev.com.ar/?p=726
comments: true
categories: 
- title: metaprogramacion
  slug: metaprogramacion
  autoslug: metaprogramacion
- title: objetos
  slug: objetos
  autoslug: objetos
- title: Ruby
  slug: ruby
  autoslug: ruby
- title: ActiveRecord
  slug: activerecord
  autoslug: activerecord
- title: Patterns
  slug: patterns-2
  autoslug: patterns
tags: 
- title: Ruby
  slug: ruby
  autoslug: ruby
- title: method_missing
  slug: method_missing
  autoslug: method_missing
- title: respond_to
  slug: respond_to
  autoslug: respond_to
- title: patterns
  slug: patterns
  autoslug: patterns
---
{{ page.title }}
================
¿Alguna vez tuviste que escribir varios métodos muy parecidos en una única clase? A mi me paso hace poco, tenía que escribir métodos que entreguen precios de una manera muy similar:

{% highlight ruby %}
class AlgoConPrecios
  def basic_price
    ...
  end
  def offer_price
    ...
  end
  def suggested_price
    ...
  end
  ...
end
{% endhighlight %}

ABURRIDOOO!!!! y para peor, ESTÁTICO!! Escribir muchos métodos similares que hacen casi lo mismo y hasta tienen un nombre parecido no tiene onda. Investigando encontré el **patrón**: **method_missing**. He aquí, cada vez que llamamos a un método que no existe en una clase, ruby busca si está definido el método: method_missing, si está hace lo que el dice y si no, nos tira una **exception** [1]:

{% highlight ruby %}
>> 9.no_existe
NoMethodError: undefined method `no_existe' for 9:Fixnum
from (irb):1
>>
class Fixnum
  def method_missing(sym, *args, &block)
    puts "Hola, soy method missing"
  end
end
>> 9.no_existe
Hola, soy method missing
=> nil
>>
{% endhighlight %}

La pregunta es: ¿Cómo aprovechamos esto? Muy simple! declaramos **method_missing** en nuestra clase, investigamos el contenido de "sym" que no es más que un **símbolo** que tiene el método que intentamos llamar (y no existe), lo procesamos y entregamos el resultado. Pero! antes tendríamos que hacer algunas validaciones. ¿Qué pasa si en algún lugar de nuestra aplicación llamamos a un método que de verdad no existe? Ahí puede que si queramos que salte la exception.

{% highlight ruby %}
def method_missing(sym, *args, &block)
  if sym.to_s =~ /^[a-z]+_price+$/
    case sym.to_s.split("_")[0]
    when "suggested" then ...
    when "basic" then ...
    end
  else
    super # si el método no tiene nada que ver tiramos la exception
  end
end
{% endhighlight %}

Esto haría que nuestro código responda a llamadas como: suggested_price, basic_price, etc. Pero no a cualquier otro tipo de llamada. ¿Cómo lo dinamizamos un poco más? Nuestro case-when-end podría venir de una tabla o algo por el estilo y ahí sí tendríamos código super dinámico. Pero ahora, qué pasa si alguien escribe unos test para nuestro código, e intenta testear la existencia de los métodos: (por ejemplo con rspec)

{% highlight ruby %}
it "should has a suggested price" do
    @objeto = AlgoConPrecios.new
    @objeto.should respond_to(:suggester_price)
  end
{% endhighlight %}

¿Cómo fingimos que nuestro método existe? ¿Recuerdan el método **respond_to?**? ¿No? Bueno, este método responde true cuando el método (entregado en el argumento) está definido: [2]

{% highlight ruby %}
>> "cadena".respond_to? "downcase"
=> true
>> "cadena".respond_to? "no_soy_un_metodo"
=> false
>>
{% endhighlight %}

Hora de redefinirlo:

{% highlight ruby %}
class AlgoConPrecios
  def respond_to?(sym, include_private=false)
    if sym.to_s =~ /^[a-z]+_price+$/
      true
    else
      super
    end
end
{% endhighlight %}

Y listo! hasta hicimos que nuestro código pase los test. Por supuesto y cada vez que jugamos con código que casi se escribe solo hay que tener mucho cuidado, sobre todo por que los errores después son difíciles de ver. También estamos generando código difícil de leer para programadores inexpertos (como el que escribe). Pero al menos hace divertido escribir métodos aburridos.
Por último, dejo un artículo sobre como funciona el find_by_... de ActiveRecord [3]. Y una propuesta bastante interesante, que como adivinaron hace algo bastante parecido.
**Resources**
[1] http://rubylearning.com/satishtalim/ruby_method_missing.html
[2] http://www.ruby-doc.org/core/classes/Object.html#M001005
[3] http://technicalpickles.com/posts/using-method_missing-and-respond_to-to-create-dynamic-methods/
[4] http://kconrails.com/2010/12/21/dynamic-methods-in-ruby-with-method_missing/
