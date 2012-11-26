--- 
layout: post
title: Objects in Javascript
wordpress_id: 657
wordpress_url: http://blog.malev.com.ar/?p=657
comments: true
categories: 
- title: objetos
  slug: objetos
  autoslug: objetos
- title: Programacion
  slug: programacion
  autoslug: programacion
- title: javascript
  slug: javascript
  autoslug: javascript
tags: 
- title: objects
  slug: objects
  autoslug: objects
- title: javascript
  slug: javascript
  autoslug: javascript
- title: prototype
  slug: prototype
  autoslug: prototype
---
{{ page.title }}
================
En una entrevista cuando buscaba trabajo me hicieron algunas preguntas teóricas acerca de **objetos** (no pude responder casi ninguna), pero recuerdo una que me dejo pensando mucho tiempo: ¿Todos los lenguajes orientados a objetos tienen **clases**? Yo dije que sí, pero resultó que no :( . **Javascript** es justamente un lenguaje orientado a objetos que no tiene clases. Vamos a ver un poco:
**Objetos en Javascript**

{% highlight javascript %}
[] instanceof Object; // true
(function(){}) instanceof Object; // true
{% endhighlight %}

Wow todo es un objeto entonces!! (bueno, casi) También  vemos en la última línea del ejemplo que javascript cuenta con funciones anónimas.

**¿Y cómo armamos nuestros objetos?**
 Simple, lo obtenemos a partir del objeto "padre" **Objetc**.

{% highlight javascript %}
obj = new Object;
// Atributos:
obj.x = 9;
obj.y = 8;
// Un método
obj.multiplication = function(){ return this.x * this.y; };
{% endhighlight %}


**Otra forma es declarar un objeto literal**
Aquí vemos un ejemplo donde no solo creamos un objetos, si no que además le añadimos atributos y hasta un método.

{% highlight javascript %}
marcosObject = {
  name : "Marcos",
  nickname : "malev",
  age : 27,
  favoriteSongs: ['yesterday', 'help'],
  talks : function(){alert("hi there, my na8me is " + this.name)}
};
{% endhighlight %}

**Object constructor**
También podemos crear un objeto a partir de otro objeto constructor. Nota: por convención los objetos constructores se nombran con el primer carácter en mayúsculas.

{% highlight javascript %}
var Person = function (name, age) {
  this.name = name;
  this.age = age;
  this.talk = function() { alert("hi, my name is " + this.name) }
}
// Volvemos a usar a keyword new
ana = new Person("Ana", 25);
ana.talk();
{% endhighlight %}

Esta forma de declarar objetos funciona, pero es muy costosa en cuando a memoria. Por cada objeto que hacemos desde el constructor Person, declaramos el método talk, es decir que tenemos varios métodos talk en memoria y todos hacen lo mismo. Una mejor aproximación es añadir el método talk al prototipo de Person:

{% highlight javascript %}
Person.prototype.jump = function(){
    alert("I am jumping");
}
{% endhighlight %}

Ahora, este nuevo método "jump" estará habilitado en todos los objetos que se hayan construido a partir del constructor Person y los objetos que se construirán en el futuro. Aquí una salvedad, NO podemos agregar un método o atributo a un objeto literal de la misma manera:

{% highlight ruby %}
Rhino 1.7 release 2 2010 09 15
js> persona = {};
[object Object]
js> persona.prototype.age = 0;
js: "", line 3: uncaught JavaScript runtime exception: TypeError: Cannot set property "age" of undefined to "0"
at :3
{% endhighlight %}

**Agregando un método o atributo a un objeto literal:**
Justamente lo interesante de la programación con objetos prototipados es que podemos tener objetos que se comportan de manera particular, es decir que tienen un comportamiento extra. Una forma de manejar ese tema es justamente añadiendo métodos a un objeto ya existente.

{% highlight javascript %}
ana.work = "Something";
ana.go = function(){ "moving" }
{% endhighlight %}

**Closures**
En javascript solo las funciones pueden generar nuevos **namescopes**, estas también permiten recordar los objetos que almacenan, de esta manera podemos hacer:

{% highlight javascript %}
funciton getCounter() {
var i = 0;
return function() { console.log(++i); }
}
var counter = getCounter();
counter() // 1
counter() // 2
{% endhighlight %}

En el ejemplo vemos que estamos retornando una función. Esto es así por que en javascript las funciones son funciones de primer clase (**first-class functions**), es decir son objetos con los que podemos trabajar.

**¿Cómo probar los ejemplos?**
En lo personal los estoy probando con la consola de javascript de Firebug en Firefox o en Rhino.

**Resources**

* http://mckoss.com/jscript/object.htm
* http://docs.jquery.com/Types#Options
* http://www.codeproject.com/kb/aspnet/JsOOP1.aspx
* http://www.crockford.com/javascript/inheritance.html
* http://eloquentjavascript.net/contents.html
* http://fingernailsinoatmeal.com/post/292301859/metaprogramming-ruby-vs-javascript
* http://www.youtube.com/watch?v=seX7jYI96GE