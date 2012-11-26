--- 
layout: post
title: Sort a hash / dictionarie
wordpress_id: 431
wordpress_url: http://blog.malev.com.ar/?p=431
comments: true
categories: 
- title: Programacion
  slug: programacion
  autoslug: programacion
- title: Ruby
  slug: ruby
  autoslug: ruby
tags: 
- title: Ruby
  slug: ruby
  autoslug: ruby
- title: programming
  slug: programming
  autoslug: programming
- title: hash
  slug: hash
  autoslug: hash
- title: ennumerable
  slug: ennumerable
  autoslug: ennumerable
---
{{ page.title }}
================
Hace bastante tiempo escuchaba una charla donde alguien había preguntado cómo ordenar un **hash**. La respuesta era: no se cómo hacerlo, pero ... para qué queres ordenar un hash? Honestamente me sigo preguntando: ¿Cuál es la necesidad de ordenar un hash? **(_Actualizado 3/11/10_ - Hay un comentario de Roberto Alsina que nos brinda una explicación de por qué ordenaríamos un Hash)**. Pero al menos ya se como ordenarlos. Igual, antes una mini definición obtenida de la ayuda de Ruby: [1]
<blockquote>A Hash is a collection of key-value pairs. It is similar to an Array, except that indexing is done via arbitrary keys of any object type, not an integer index.</blockquote>
_Un hash es una colección de parse llave-valor. Es similar a un arrelo, excepto que el indexado se hace mediante llaves arbitrarias de cualquier tipo de objeto, no un índice entero._ (mis traducciones son siempre un poco flojas).
En ruby la clase Hash incluye al módulo Enumerable [2] que provee entre otras cosas la habilidad de realizar un ordenamiento (**sort**).
**Ejemplo fallido:**

{% highlight ruby %}
malev@malev-dell:~/code/temp$ irb
>> h = {:primero => "primero", :segundo => "segundo", :tercero => "tercero"}
=> {:primero=>"primero", :segundo=>"segundo", :tercero=>"tercero"}
>> h.sort
h.sort     h.sort_by  
>> h.sort
NoMethodError: undefined method `<=>' for :primero:Symbol
from (irb):2:in `<=>'
from (irb):2:in `sort'
from (irb):2
{% endhighlight %}

Está claro que no podemos realizar el ordenamiento tan simple como con los arreglos comunes. Además, el ejemplo es malo pues ya está ordenado. Vamos a probar el método **sort_by**:

{% highlight ruby %}
>> # Opción 1
>> h = {:z => "z", :h => "h", :b => "b", :t => "t" }
=> {:b=>"b", :t=>"t", :h=>"h", :z=>"z"}
>> h.sort_by { |key| key.to_s }
=> [[:b, "b"], [:h, "h"], [:t, "t"], [:z, "z"]]
>> # Opción 2
>> h
=> {:b=>"b", :t=>"t", :h=>"h", :z=>"z"}
>> h.sort {|a,b| a[1] <=> b[1]}
=> [[:b, "b"], [:h, "h"], [:t, "t"], [:z, "z"]]
{% endhighlight %}

Estamos cerca, pero todavía no tenemos un hash ordenado como resultado, si no un arreglo extraño. Pero claro, los métodos sort y sort_by devuelven un arreglo siempre :)
Convirtiendo el extraño arreglo en un hash ahora sí ordenado:

{% highlight ruby %}
>> j = Hash.new
=> {}
>> a = [[:b, "b"], [:h, "h"], [:t, "t"], [:z, "z"]] 
=> [[:b, "b"], [:h, "h"], [:t, "t"], [:z, "z"]]
>> a.each { |i| j[i[0]] = i[1] }
=> [[:b, "b"], [:h, "h"], [:t, "t"], [:z, "z"]]
>> j
=> {:b=>"b", :t=>"t", :h=>"h", :z=>"z"}
{% endhighlight %}

**_Actualizado 03/11/10_ - Matías Flores recomienda en un comentario:**

{% highlight ruby %}
>> h = {:z => "z", :h => "h", :b => "b", :t => "t" }
=> {:b=>"b", :t=>"t", :h=>"h", :z=>"z"}
>> a = h.sort_by { |key| key.to_s }
=> [[:b, "b"], [:h, "h"], [:t, "t"], [:z, "z"]]
>> Hash[a]
=> {:b=>"b", :t=>"t", :h=>"h", :z=>"z"}
{% endhighlight %}

Bueno, así ordenaría un Hash en Ruby. Es bastante complicado, pero ... seguramente no lo voy a usar nunca :)
Nota: uno de los ejemplos fue extraído de [3].
[1] [http://ruby-doc.org/core/classes/Hash.html](http://ruby-doc.org/core/classes/Hash.html)
[2] [http://ruby-doc.org/core/classes/Enumerable.html](http://ruby-doc.org/core/classes/Enumerable.html)
[3] [http://nhw.pl/wp/2007/06/11/sorting-hash-by-values](http://nhw.pl/wp/2007/06/11/sorting-hash-by-values)
