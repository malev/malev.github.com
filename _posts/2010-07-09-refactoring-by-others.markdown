--- 
layout: post
title: Refactoring by others
wordpress_id: 348
wordpress_url: http://blog.malev.com.ar/?p=348
comments: true
categories: 
- title: comunidad
  slug: comunidad
  autoslug: comunidad
- title: Programacion
  slug: programacion
  autoslug: programacion
- title: tools
  slug: tools
  autoslug: tools
tags: 
- title: Ruby
  slug: ruby
  autoslug: ruby
- title: community
  slug: community
  autoslug: community
- title: programming
  slug: programming
  autoslug: programming
- title: asking
  slug: asking
  autoslug: asking
- title: question
  slug: question
  autoslug: question
- title: refactoring
  slug: refactoring
  autoslug: refactoring
---
{{ page.title }}
================
Mientras buscaba información para un post, encontré [esta red social](http://refactormycode.com/) llamada refactor my code :D  ¡Me pareció alucinante!. Posteas un poco de código y otros usuarios postean como comentarios como lo refactorizaría para que quede más "bonito".

¿De que estás hablando Willy?
**Código original**

{% highlight ruby %}
def create_files
  files = []
  sizes = ['10K', '100K', '500K', '1M', '2M', '5M', '10M', '20M']
  
  sizes.each do |size|
    100.times do
      files << create_file(size)
    end
  end
  files
end
{% endhighlight %}


**Código refactorizado** (hace lo mismo, mejor escrito y de ser posible mejor)

{% highlight ruby %}
def create_files
  %w{ 10K 100K 500K 1M 2M 5M 10M 20M }.map do |size|
    100.times.map {|index| create_file(size)}
  end.flatten
end
{% endhighlight %}

Por supuesto, no es buena idea fiarse 100% del refactoring de personas desconocidas, pero esta bueno para ir aprendiendo. Sobre todo cuando uno empieza con un lenguaje nuevo.
También les cuento que desde hace varios años existe [Stackoverflow](http://stackoverflow.com/), un sitio muy conocido dentro de la comunidad, donde postear preguntas sobre programación principalmente y por qué no, dejar alguna respuesta de vez en cuando ;)
