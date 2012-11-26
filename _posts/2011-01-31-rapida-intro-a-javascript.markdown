--- 
layout: post
title: "R\xC3\xA1pida intro a javascript"
wordpress_id: 707
wordpress_url: http://blog.malev.com.ar/?p=707
comments: true
categories: 
- title: Programacion
  slug: programacion
  autoslug: programacion
- title: jQuery
  slug: jquery
  autoslug: jquery
- title: javascript
  slug: javascript
  autoslug: javascript
tags: 
- title: jQuery
  slug: jquery
  autoslug: jquery
- title: programming
  slug: programming
  autoslug: programming
- title: javascript
  slug: javascript
  autoslug: javascript
---
{{ page.title }}
================
Hace bastante que vengo usando **javascript**, el lenguaje más subestimado de la historia, en la oficina. Al principio solo lo usaba para "efectitos" con **jQuery** y cosillas así, por supuesto el grado de complejidad fue aumentando y me empecé a repetir. Con la idea de empezar a reutilizar código, con lo cual tengo que escribir mejor código y más fiable, arme un pequeño resumen para luego presentar algo de objetos en Javascript.

**¿Cómo ejecutarlo en una página?**
Embebiendo javascript en HTML

{% highlight javascript %}
// source code goes here
{% endhighlight %}

Separando el código javascript del HTML

{% highlight javascript %}

Esta última es la <strong>recomendable siempre</strong>, pues ayuda a aislar la presentación, estructura y el contenido del comportamiento. Jason Seifer explica en un divertidísimo podcast por que recomienda usar Unobtrusive Javascript. [1][2]

<strong>¿Cómo ejecutarlo fuera de una página?</strong>
Usando una cosola externa como <strong>Rhino</strong>, que es Javascript implementado en Java. Buenísimo para hacer pruebas rápidas. (http://www.mozilla.org/rhino/).
En una consola de javascript como por ejemplo <strong>Firebug</strong> (http://getfirebug.com/). Si no lo conoces no existís!
<strong>JSLint</strong>, una herramienta para verifical la calidad del código. Honestamente la descubrí recién, pero está bueno tenerla presente para investigarla un poco más. (http://jslint.com)

<strong>Javascript para programadores</strong>
El título no es mío, es de un RE artículo de una empresa llamada Wooji Juice [3]. Esta gente se tomo la molestia de hacer una breve guía de Javascript para gente que ya sabe programar y no como las millones de guías orientadas diseñadores que en lo personal me tienen cansado. (Encontre este manual en el blog de Juanjo Conti).

<strong>Super síntesis (robada del artículo anterior):</strong>


 * comments (both /* C */ style and // C++ style )

 * if / then / else

 * switch / case / break / default

 * while / do / break / continue

 * for / break / continue

 * punto y coma al final de cada declaración

 * typeof ... Nos dice con que objeto estamos interactuando



<em>No es joda, para empezar no hace falta más que esto.</em>

<strong>Variables</strong>
En js las variables no tienen tipo y por defecto son globales (asociadas al <strong>global object</strong>). Esto último puede generar muchos dolores de cabeza, por lo tanto es muy bueno tenerlo presente y más aún, saber como se declaran las variables locales [4]:
<pre lang="javascript">
var a = 9; // local variable
var b;  // local variable, su valor es "undefined"
myglobal = "hello"; // global variable
</pre>
<strong>Funciones</strong>
En javascript podemos definir funciones con nombre (de las comunes), anónimas e inclusive podemos asignarlas a variables:
<pre lang="javascript">
function sumar(a, b){
  return a + b;
}
var multiplicar = function(a, b){ return a * b; }
function(a){ return a*a; }
</pre>
Quizás el último caso sea el más extraño, pero si alguna vez usaste jQuery, segurísimo esto te suena familiar [5]:
<pre lang="javascript">
$('#target').click(function() {
  alert('Handler for .click() called.');
});
</pre>
En este caso se está pasando como parámetro una función anónima a la función click. <i>Algún día entendere como funciona jQuery.</i>

<strong>Arrays y hashes</strong>
<pre lang="javascript">
var a = [];
var b = [1,2,3,4,5];
var c = [1, "t", function(g){ return g; } ];
// Sí, también le podemos guardar una función dentro de un Array.
a.length // 0
b.slice(1,3) // 2,3
// HASHES
var d = {}
var e = {"key": "value", "key2": "value2"}
e["key2"] // "value2"
</pre>
<strong>Un for copado y un each aún más copado</strong>
El each forma parte del framework jQuery. No te creo si me dices que no lo conoces. [6]
for (<variable> in <variable of container type>) <statements>
Ojo! no devuelve la variable interna, obtenemos un índice hacia ella:
<pre lang="javascript">
js> a = [1,2,3,4,5]
js> for(i in a) print(a[i]);
1
2
3
4
5
</pre><pre lang="javascript">
$("button").click(function () {
  $("div").each(function (index, domEle) {
    // domEle == this
    $(domEle).css("backgroundColor", "yellow");
    if ($(this).is("#stop")) {
      $("span").text("Stopped at div index #" + index);
      return false;
    }
  });
});
</pre>
Y eso es todo por ahora, la idea es profundizar en objetos en un próximo post y en el futuro, si me animo como extender jQuery.

<strong>Referencias</strong>
[1] http://jasonseifer.com/2008/03/13/unobtrusive-javascript
[2] http://en.wikipedia.org/wiki/Unobtrusive_JavaScript
[3] http://www.wooji-juice.com/blog/javascript-article.html
[4] http://net.tutsplus.com/tutorials/javascript-ajax/the-essentials-of-writing-high-quality-javascript/
[5] http://api.jquery.com/click
[6] http://api.jquery.com/each/
{% endhighlight %}
</statements></variable></variable>
