--- 
layout: post
title: "M\xC3\xBAltiples submit en un form"
wordpress_id: 315
wordpress_url: http://blog.malev.com.ar/?p=315
comments: true
categories: 
- title: Programacion
  slug: programacion
  autoslug: programacion
- title: jQuery
  slug: jquery
  autoslug: jquery
- title: rails
  slug: rails
  autoslug: rails
tags: 
- title: buttons
  slug: buttons
  autoslug: buttons
- title: form
  slug: form
  autoslug: form
- title: submit
  slug: submit
  autoslug: submit
- title: rubyonrails
  slug: rubyonrails
  autoslug: rubyonrails
- title: jQuery
  slug: jquery
  autoslug: jquery
- title: rails
  slug: rails
  autoslug: rails
- title: hidden
  slug: hidden
  autoslug: hidden
---
{{ page.title }}
================
Hoy me enfrente a este problema, necesitaba poner 2 submit buttons en un solo form. Una forma bastante grosera de hacerlo era leyendo desde el controller el título del submit presionado:

{% highlight ruby %}
#view
<% form_for(@object) do |f| %>
  

<%= f.label :title %>

    <%= f.text_field :title %>

  

<%= f.submit 'submit_1'%>

  

<%= f.submit 'submit_2' %>

<% end %>
{% endhighlight %}


{% highlight ruby %}
#Controller
  def create #O el método que sea
    if params["commit"] == "submit_1"
      #Ejecutar submit_1
    else
      #Ejecutar submit_2
    end
  end
{% endhighlight %}

Pero aquí hay algo grave! Hay una gran dependencia entre la vista y el controlador y NO! queremos eso.
Solución alternativa y muy interesante: Crear un campo oculto y usar jQuery para rellenarlo según el submit presionado:

{% highlight ruby %}
#view
  

<%= f.label :title %>

    <%= f.text_field :title %>
{% endhighlight %}

Ahora, desde el controlador, solo necesitamos revisar el contenido de params[:object][:hidden] para determinar que submit button fue el presionado. También es necesario incluir jQuery, esto se puede hacer desde el layout usando un helper:

{% highlight ruby %}
#layout
<%= javascript_include_tag 'jquery', 'application' %>
{% endhighlight %}

No se olviden de instalar jQuery en el directorio /public y con toodooo eso ya estaría listo!
[1] [jQuery](http://jquery.com/)
[2] [Railscasts de jQuery en Rails](http://railscasts.com/episodes/136-jquery)
