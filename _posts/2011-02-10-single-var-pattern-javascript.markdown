--- 
layout: post
title: Single var pattern - Javascript
wordpress_id: 735
wordpress_url: http://blog.malev.com.ar/?p=735
comments: true
categories: 
- title: buenas practicas
  slug: buenas-practicas
  autoslug: buenas-practicas
- title: javascript
  slug: javascript
  autoslug: javascript
- title: Patterns
  slug: patterns-2
  autoslug: patterns
tags: 
- title: javascript
  slug: javascript
  autoslug: javascript
- title: patterns
  slug: patterns
  autoslug: patterns
- title: js
  slug: js
  autoslug: js
- title: hoising
  slug: hoising
  autoslug: hoising
- title: var
  slug: var
  autoslug: var
---
{{ page.title }}
================
Javascript es un lenguaje muy particular, cualquier variable que no declaremos se convierte en propiedad del global object (objeto global), es decir se convierte en una **variable global**. Que en el caso de los browsers, es el mismísimo window. Aquí un ejemplo extraído de [1]:

{% highlight javascript %}
myglobal = "hello";  // antipattern  
   console.log(myglobal);    // "hello"  
   console.log(window.myglobal); // "hello"
{% endhighlight %}

Ahora mostramos como una variable no declarada (resultado) es visible aún fuera de la función donde apareció por primera vez:

{% highlight javascript %}
>>> function cuadrado(x) { resultado = x*x; return resultado }
undefined
>>> cuadrado(9)
81
>>> resultado
81
{% endhighlight %}

Nota: Podemos probar los ejemplos en la consola de firebug en firefox.

**Las variables globales son evil**(malas)
Al menos eso me han dicho. Lo que sí hacen o pueden hacer es interferir con alguna librería que estemos usando y generarnos comportamientos inesperados en nuestros propios scripts. Lo ideal es tratar de minimizarlas. ¿Cómo?

**Usando variables locales**
Una variable local es aquella que se declara dentro de una función, siendo local y existiendo únicamente en esa función:

{% highlight javascript %}
function cuadrado(x)
  var resultado = x*x;
  return resultado;
}
{% endhighlight %}

Demo:

{% highlight javascript %}
js> function cuadrado(x){ var resultado = x*x; return resultado }
js> cuadrado(9)
81
js> resultado
js: "", line 4: uncaught JavaScript runtime exception: ReferenceError: "resultado" is not defined.
at :4

js>
{% endhighlight %}


**Listo! entonces usemos solo variables locales! pero y Dónde está el single var pattern?**
En javascript podemos declarar varias variables **locales** en una sola línea: var a, b; (No hace falta que le asignemos un valor). Usando el mismo criterio, podemos escribir un único **var** arriba dentro de la función y de ahí separar todo con comas.

{% highlight javascript %}
function func() {
  var a = 1,
    b = 2,
    sum = a + b,
    myobject = {},
    j;
// function body...
}
{% endhighlight %}

No parece nada del otro mundo, pero trae algunas ventajas y desventajas: tenemos todas las variables ubicadas en un solo lugar, evita que nos olvidemos el **var** y previene **hoisting**. Desventajas: queda feo.

**Hoising**
Javascript considera que todas las declaraciones de variables se realizan en la parte de arriba de la función. No importa si la declaramos luego en el medio del código, aún usando **var**. En ese caso, se declararía arriba con un valor indefinido. El ejemplo a continuación ilustra el fenomeno.

{% highlight javascript %}
myname = "global"; // global variable  
function func() {  
  alert(myname); // "undefined"  
  var myname = "local";  
  alert(myname); // "local"  
  }
func();
{% endhighlight %}

**Resources**
[1] [http://net.tutsplus.com/tutorials/javascript-ajax/the-essentials-of-writing-high-quality-javascript/](http://net.tutsplus.com/tutorials/javascript-ajax/the-essentials-of-writing-high-quality-javascript/)
[2] [http://snook.ca/archives/javascript/global_variable](http://snook.ca/archives/javascript/global_variable)
[3] [http://2007-2010.lovemikeg.com/2010/01/19/responsible-javascript-globals/](http://2007-2010.lovemikeg.com/2010/01/19/responsible-javascript-globals/)
[4] [http://www.keyframesandcode.com/resources/javascript/deconstructed/jquery/](http://www.keyframesandcode.com/resources/javascript/deconstructed/jquery/)
