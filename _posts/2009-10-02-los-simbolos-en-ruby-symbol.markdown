--- 
layout: post
title: "Los s\xC3\xADmbolos en ruby (symbols)"
wordpress_id: 42
wordpress_url: http://blog.malev.com.ar/?p=42
comments: true
categories: 
- title: Ruby
  slug: ruby
  autoslug: ruby
- title: ruby-core
  slug: ruby-core
  autoslug: ruby-core
tags: 
- title: metaprogramacion
  slug: metaprogramacion
  autoslug: metaprogramacion
- title: metaprogramming
  slug: metaprogramming
  autoslug: metaprogramming
- title: Ruby
  slug: ruby
  autoslug: ruby
- title: simbolos
  slug: simbolos
  autoslug: simbolos
- title: symbol
  slug: symbol
  autoslug: symbol
---
{{ page.title }}
================
Los símbolos representan nombre y algunas cadenas dentro del intérprete de Ruby. Se pueden generar haciendo :nombre, :”cadena” o con el conversor **to_sym**. Se creará un único objeto para cada nombre o cadena y este dura toda la ejecución del programa.

**Métodos de la clase Symbol:**
1.- all_symbols – Devuelve la tabla de símbolos de Ruby.
2.- id2name – devuelve una cadena – :name.id2name no da “name”.
3.- inspect – Returns the symbol literal #no se que será eso :S
4.- to_i – Un entero, propio del símbolo
5.- to_s - igual que id2name
6.- to_sym – Convierte un símbolo en un símbolo! devuelve self :X

<blockquote>
Symbol.all_symbols.size    # 903
Symbol.all_symbols[1,20]   # [:floor, :ARGV, :Binding, …]
:hola
:hola.to_s      # “hola”
:hola.to_i      # 28201
:hola.object_id # 282018
:fred.inspect   # ":fred"</blockquote>

**Diferencia con las contantes**
Una constante “Fred” puede ser una constantes en un contexto dado, un método en otro o hasta una referencia a una clase. En cambio :Fred será el mismo objeto en los tres contextos. [1]

Cómo cada vez que llamo la cadena “string” genero un objeto nuevo, en cambio, el símbolo :symbol es siempre el mismo, por lo que puedo aprovechar esta característica para ahorrar un poco de memoria.

El ejemplo clásico es comparar el object_id de un símbolo y el de una cadena para ver que... [2]
<blockquote>puts "string".object_id
puts "string".object_id
puts :symbol.object_id
puts :symbol.object_id</blockquote>
Ojo! El **garbage collector** nunca los elimina de memoria. Por lo que si creas símbolos que no vas a usar más en toda la ejecución del programa, pierdes toda esa memoria. [3]

**Cuándo usamos un símbolo y cuándo una cadena** [4]
Por ejemplo, una cadena que no necesitaremos cambiar más en todo el programa. O que no necesitemos hacer uso de los métodos de la clase String.
Si el contenido (secuencia de caracteres) del objeto es importante, entonces usar cadenas. Si por el contrario, lo importante es la identidad del objeto, entonces usar símbolos.
Un ejemplo clásico de este dilema se da a la hora de definir un hash:
<blockquote>usuario = {“nombre” =&gt; “Marcos”, “apodo” =&gt; “malev”}
usuario = {:nombre =&gt; “Marcos”, :apodo =&gt; “malev”}</blockquote>
Claramente conviene usar símbolos, ya que “nombre” o :nombre no es tan importante, si no a lo que apunta. Por otro lado, cada vez que llamemos a este hash y hagamos uso de la clave “nombre” estaremos creando un nuevo objeto, con su consiguiente gasto de memoria.

**Otros usos**
También se usan para referenciar métodos y los argumentos de los mismos.

Por último y para complicarnos la existencia, en [5] hay una discusión en la que Ryan propone no usar símbolos como claves de los hashes. Vean Uds. por que...

[1] [ruby-doc.org](http://ruby-doc.org)
[2] [rubylearning.com](http://rubylearning.com/satishtalim/ruby_symbols.html)
[3] [www.randomhacks.net](http://www.randomhacks.net/articles/2007/01/20/13-ways-of-looking-at-a-ruby-symbol)
[4][glu.ttono.us](http://glu.ttono.us/articles/2005/08/19/understanding-ruby-symbols)
[5] [www.oreillynet.com](http://www.oreillynet.com/ruby/blog/2005/12/yet_another_blog_about_ruby_sy.html)
