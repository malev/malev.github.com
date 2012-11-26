--- 
layout: post
title: Hash, hashie y struct
wordpress_id: 691
wordpress_url: http://blog.malev.com.ar/?p=691
comments: true
categories: 
- title: gems
  slug: gems
  autoslug: gems
- title: objetos
  slug: objetos
  autoslug: objetos
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
- title: hashie
  slug: hashie
  autoslug: hashie
---
{{ page.title }}
================
Bueno, los **Hash** o diccionarios casi no necesitan presentación, son colecciones de pares key-value, como arreglos, pero en vez de acceder a ellos por un posición, se puede acceder por un objeto, cualquiera sea su tipo [1]:

{% highlight ruby %}
>> dic = {"saludo" => "hola", :despedida => "adios", 2 => 5}
=> {:despedida=>"adios", "saludo"=>"hola", 2=>5}
{% endhighlight %}

Una cosa importante, devuelven **nil** cuando intentamos acceder a una key que no existe:

{% highlight ruby %}
>> dic["no_existo"]
=> nil
{% endhighlight %}

A nivel código, incluyen el módulo **Enumerable**, por lo que comparten muchos métodos con sus primos los arreglos, por ejemplo se puede interarlos:

{% highlight ruby %}
>> dic.each_pair { |k,v| puts k, v }
{% endhighlight %}

**Agregando, buscando y eliminando elementos (con condicional):**

{% highlight ruby %}
>> dic["nuevo"] = 89
=> 89
>> dic
=> {:despedida=>"adios", "nuevo"=>89, "saludo"=>"hola", 2=>5}
>> dic["saludo"]
=> "hola"
>> dic.delete "saludo"
=> "hola"
>> dic
=> {:despedida=>"adios", "nuevo"=>89, 2=>5}
>> nuevo = { :marcos => 27, :alvaro => 25, :diego => 17 }
=> {:marcos=>27, :alvaro=>25, :diego=>17}
>> nuevo.delete_if { |k,v| v <18 }
=> {:marcos=>27, :alvaro=>25}
{% endhighlight %}

**Preguntando por elementos:**

{% highlight ruby %}
=> {:despedida=>"adios", "nuevo"=>89, 2=>5}
>> dic.has_key? "nuevo"
=> true
>> dic.has_value? 2
=> false
{% endhighlight %}

**También tenemos un método fetch que nos acepta un número variable de argumentos:**
1) Un key, que buscará en el hash, de no existir, tira una exception
2) Una key más un valor por defecto, si no existe la key, nos devuelve el valor por defecto
3) Una key y un bloque

{% highlight ruby %}
>> dic.fetch :despedida
=> "adios"
>> dic.fetch "noexisto"
IndexError: key not found
from (irb):14:in `fetch'
from (irb):14
>> dic.fetch "noexisto", "default"
=> "default"
{% endhighlight %}

**¿Qué pasa cuando los Hashes ya no son suficientes?**
Ahí entra hashie, una gema desarrollada por gente de intredea [2], que nos entrega unas cuantas clases para generar objetos similares a los hash, a los objetos de ActiveRecord, etc. Como casi todas las gemas, su código se encuentra en gihub [3]. Muy interesante:
1) Mash, un hash extendido, con comportamiento de pseudo objeto:

{% highlight ruby %}
mash = Hashie::Mash.new
    mash.name? # => false
    mash.name # => nil
    mash.name = "My Mash"
    mash.name # => "My Mash"
    mash.name? # => true
    mash.inspect # =>
{% endhighlight %}

2) Dash, otro hash extendido, pero con una cantidad de propiedades discretas predefinidas. Pueden tomar valores por defecto.

{% highlight ruby %}
class Person < Hashie::Dash
      property :name
      property :email
      property :occupation, :default => 'Rubyist'
    end
    p = Person.new
    p.name # => nil
    p.email = 'abc@def.com'
    p.occupation   # => 'Rubyist'
{% endhighlight %}

Por último, tenemos una opción desde la librería estandar, los **Struct**. Una opción interesante, sobre todo cuando necesitamos hardcodear alguna vista o algo por el estilo en Rails.

{% highlight ruby %}
Customer = Struct.new(:name, :address)
Customer.new("Dave", "123 Main")
{% endhighlight %}

**Resources**
[1] http://www.ruby-doc.org/core/classes/Hash.html
[2] http://intridea.com/posts/hashie-the-hash-toolkit
[3] https://github.com/intridea/hashie
