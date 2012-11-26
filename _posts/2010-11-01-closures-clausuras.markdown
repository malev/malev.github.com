--- 
layout: post
title: Closures (clausuras)
wordpress_id: 585
wordpress_url: http://blog.malev.com.ar/?p=585
comments: true
categories: 
- title: objetos
  slug: objetos
  autoslug: objetos
- title: Programacion
  slug: programacion
  autoslug: programacion
- title: Python
  slug: python
  autoslug: python
- title: Ruby
  slug: ruby
  autoslug: ruby
- title: ruby-core
  slug: ruby-core
  autoslug: ruby-core
tags: 
- title: metaprogramming
  slug: metaprogramming
  autoslug: metaprogramming
- title: Python
  slug: python
  autoslug: python
- title: Ruby
  slug: ruby
  autoslug: ruby
- title: closures
  slug: closures
  autoslug: closures
---
{{ page.title }}
================
**Definición**
Un **closure** (clausura) es una funcioń evaluada en un entorno contenido con una o más variables dependientes de otro entorno. Las variables dentro del closure mantienen su vida útil hasta que el closure muere. (un poco trágico). Se usan mucho en programación funcional, en la construcción de objetos y en la construcción de estructuras de control. [1]
[
![](/images/posts/2010/10/closure-300x288.png "closure")
](http://blog.malev.com.ar/wp-content/uploads/2010/10/closure.png)
**Definición académica**
"In computer science, a closure is a first-class function with free variables that are bound in the lexical environment." WTF!! [1]

**Definición iluminadora**
Los objetos son datos y métodos, los closures son funciones con datos adjuntos.
_"Objects are data with methods attached, closures are functions with data attached."_[2]

**Historia (para los fanáticos de smalltalk)**
El concepto de closure de desarrolló en los 60 y se implementó en un lenguaje llamado Scheme (Así es, no vienen de smalltalk).

**Explicación más simple**
Un closure es basicamente una función o método que tiene las siguientes propiedades:
 - Puede pasar como un objeto, inclusive ser llamado después.
 - Recuerda los valores de las variables en su **scope** (alcance) al momento de ser declarado. Puede acceder a estas variables luego, inclusive si ya no se encuentran dentro de su scope. [3]
Entonces un closure puede ser definido en un scope y luego llamado desde otro completamente diferente manteniendo los estados en que fue definido.

**Ejemplo - Ruby** [5]

{% highlight ruby %}
class SomeClass
  def initialize(value1)
    @value1 = value1
  end

  def value_printer(value2)
    lambda {puts "Value1: #{@value1}, Value2: #{value2}"}
  end
end

def caller(some_closure)
  some_closure.call
end

some_class = SomeClass.new(5)
printer = some_class.value_printer("some value")

caller(printer) # => Value1: 5, Value2: some value
{% endhighlight %}

**¿Cómo hace el lenguaje para retener el valor de las variables?** [3]
Hay dos opciones (capaz hay más, pero yo encontré estas).
 - El closure crea una copia de todas las variables que necesita cuando es definido.
 - El closure extiende la vida úlil de las variables. No las copia pero mantienen referencia a ellas así no se las lleva el Garbage collector.
**Moraleja**: Si el lenguaje usa la primer forma, entonces muchos closures tendrán su propia copia de las variables; en el 2do caso, los closures solo guardan referencia a la variable. (Python y Ruby usa la 2da forma). [5]

{% highlight ruby %}
class SomeClass
  def initialize(value1)
    @value1 = value1
  end
  def value_incrementer
    lambda {@value1 += 1}
  end
  def value_printer
    lambda {puts "value1: #{@value1}"}
  end
end

some_class = SomeClass.new(2)

incrementer = some_class.value_incrementer
printer = some_class.value_printer

(1..3).each do
  incrementer.call
  printer.call
end

This produces the following output:

alan@alan-ubuntu-vm:~/tmp$ ruby closures.rb
value1: 3
value1: 4
value1: 5
{% endhighlight %}

**python**
Este lenguaje implementa los closures mediante la función lambda y mediante funciones dentro de funciones (named closures). Aquí solo vemos el 2do método.

{% highlight python %}
def makeInc(x):
  def inc(y):
     # x is "closed" in the definition of inc
     return y + x

 return inc

inc5 = makeInc(5)
inc10 = makeInc(10)

inc5 (5) # returns 10
inc10(5) # returns 15
{% endhighlight %}

**Python 3000 - closures**
No funciona en versiones anteriores pues usa la nueva keyword: **nonlocal**

{% highlight python %}
def outer():
    y = 0
    def inner():
        nonlocal y
        y += 1
        return y
    return inner

f = outer()

print(f(), f(), f()) #prints 1 2 3
{% endhighlight %}

La función anidada inner() declara un closure y lo llama inner.
nota: la función lambda no acepta if o for en su interior.
**Javascript**

{% highlight javascript %}
var printMessage = function (s) {
  var f = function () {
    console.log(s + ' was pressed.');
  }
  return f;
}
buttonA.onClick = printMessage('A');
buttonB.onClick = printMessage('B');
buttonC.onClick = printMessage('C');
{% endhighlight %}

**Ventajas de los closures**
 - Reemplazan las constantes hard coded
 - Eliminan globals
 - Útiles en los callbacks the systemas orientados a eventos
En [7] hay un listado de usos prácticos de closures en refactoring.
**Bibliografía que leí**
[1] [http://en.wikipedia.org/wiki/Closure_%28computer_science%29](http://en.wikipedia.org/wiki/Closure_%28computer_science%29)
[2] [http://stackoverflow.com/questions/13857/can-you-explain-closures-as-they-relate-to-python](http://stackoverflow.com/questions/13857/can-you-explain-closures-as-they-relate-to-python)
[3] [http://www.skorks.com/2010/05/closures-a-simple-explanation-using-ruby/](http://www.skorks.com/2010/05/closures-a-simple-explanation-using-ruby/)
[4] [http://stackoverflow.com/questions/3145893/how-are-closures-implemented-in-python](http://stackoverflow.com/questions/3145893/how-are-closures-implemented-in-python)
[5] [http://samdanielson.com/2007/9/6/an-introduction-to-closures-in-ruby](http://samdanielson.com/2007/9/6/an-introduction-to-closures-in-ruby)
[6] [http://ynniv.com/blog/2007/08/closures-in-python.html](http://ynniv.com/blog/2007/08/closures-in-python.html)
[7] [http://www.ibm.com/developerworks/java/library/j-cb01097.html](http://www.ibm.com/developerworks/java/library/j-cb01097.html)
**Bibliografía que no leí**, pero seguro lo haré pronto
[http://weblog.raganwald.com/2007/01/closures-and-higher-order-functions.html](http://weblog.raganwald.com/2007/01/closures-and-higher-order-functions.html)
[http://www.randomhacks.net/articles/2007/02/01/some-useful-closures-in-ruby](http://www.randomhacks.net/articles/2007/02/01/some-useful-closures-in-ruby)
[http://www.robertsosinski.com/2008/12/21/understanding-ruby-blocks-procs-and-lambdas/](http://www.robertsosinski.com/2008/12/21/understanding-ruby-blocks-procs-and-lambdas/)
[http://www.artima.com/intv/closures.html](http://www.artima.com/intv/closures.html)
