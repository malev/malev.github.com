--- 
layout: post
title: HTML - Zen coding
wordpress_id: 413
wordpress_url: http://blog.malev.com.ar/?p=413
comments: true
categories: 
- title: tools
  slug: tools
  autoslug: tools
- title: plugin
  slug: plugin
  autoslug: plugin
tags: 
- title: plugins
  slug: plugins
  autoslug: plugins
- title: zen
  slug: zen
  autoslug: zen
- title: editor
  slug: editor
  autoslug: editor
- title: html
  slug: html
  autoslug: html
- title: css
  slug: css
  autoslug: css
---
{{ page.title }}
================
Como habrán notado por varios de mis posts, soy bastante reacio al uso de IDEs para programar y aunque Robert Martin nos recomiende lo contrario [1], todavía no ha logrado convencerme jeje.
Bien, partiendo de que uso un editor bastante básico para codear, no hay nada más molesto que escribir HTML en el mismo. Este lenguaje tiene tags muy molestos que hay que abrirlos y cerrarlos usando los símbolos de mayor y menor. La verdad bastante doloroso. Al menos así era hasta que encontré ZenCoding [2].

Zencoding es un plugin para editores que nos permite escribir HTML de una manera fantástica, por ejemplo, en nuestro editor tiramos:

**\#content** y luego presionamos Ctrl + E y obtenemos:

{% highlight html %}
<div id="content"></div>
{% endhighlight %}

Hasta aquí nada nuevo, nada que un editor con un poquito de pilas no pueda hacer, lo interesante es cuando queremos armar estructuras más complejas, por ejemplo:

**ul.list>li*4** y luego presionamos Ctrl + E y obtenemos:

{% highlight html %}
<ul class="list">
  <li></li>
  <li></li>
  <li></li>
  <li></li>
</ul>
{% endhighlight %}

Guau!! y lo mejor es que esto no es todo, hay muchos muchos ejemplos en [2] y en [3]. También hay se lo puede usar en CSS y anda en un montón de editores, si quieren ver cuántos, solo visiten [4].

**Resources**

* [1] [http://www.youtube.com/watch?v=mslMLp5bQD0](http://www.youtube.com/watch?v=mslMLp5bQD0)
* [2] [http://code.google.com/p/zen-coding/](http://code.google.com/p/zen-coding/)
* [3] [http://www.sitepoint.com/blogs/2010/05/11/how-to-code-like-a-zen-master/](http://www.sitepoint.com/blogs/2010/05/11/how-to-code-like-a-zen-master/)
* [4] [http://code.google.com/p/zen-coding/downloads/list](http://code.google.com/p/zen-coding/downloads/list)
