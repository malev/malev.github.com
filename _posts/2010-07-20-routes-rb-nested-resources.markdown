--- 
layout: post
title: routes.rb - Nested resources
wordpress_id: 386
wordpress_url: http://blog.malev.com.ar/?p=386
comments: true
categories: 
- title: Programacion
  slug: programacion
  autoslug: programacion
- title: rails
  slug: rails
  autoslug: rails
- title: Ruby
  slug: ruby
  autoslug: ruby
tags: 
- title: rails
  slug: rails
  autoslug: rails
- title: routes
  slug: routes
  autoslug: routes
- title: nested
  slug: nested
  autoslug: nested
- title: resources
  slug: resources
  autoslug: resources
---
{{ page.title }}
================
Continuando con el [post anterior](http://blog.malev.com.ar/2010/07/15/routes-en-rails-restful/). Cuando RESTful parece no alcanzar y cuando nuestros recursos empiezan a tener hijos necesitamos de los recursos anidados (nested resources). Aquí usaremos los mismos ejemplos de [1].
Suponemos que tenemos una tabla revistas (magazines) y cada revista a su vez tiene  publicidades (ads). Tendríamos los modelos:

{% highlight ruby %}
class Magazine < ActiveRecord::Base
has_many :ads
end

class Ad < ActiveRecord::Base    
  belongs_to :magazine
end
{% endhighlight %}

En routes.rb configuramos: 

{% highlight ruby %}
map.resources :magazines do |magazine|
  magazine.resources :ads  
end  
# o la versión corta: 
# map.resources :magazines, :has_many => :ads.
{% endhighlight %}


Tendremos como resultado, las siguientes rutas:
<table>
<tbody>
<tr>
<th>Verb</th>
<th>URL</th>
<th>controller</th>
<th>action</th>
<th>used for</th>
</tr>
<tr>
<td>GET</td>
<td>/magazines/1/ads</td>
<td>Ads</td>
<td>index</td>
<td>display a list of all ads for a specific magazine</td>
</tr>
<tr>
<td>GET</td>
<td>/magazines/1/ads/new</td>
<td>Ads</td>
<td>new</td>
<td>return an HTML form for creating a new  ad belonging to a specific magazine</td>
</tr>
<tr>
<td>POST</td>
<td>/magazines/1/ads</td>
<td>Ads</td>
<td>create</td>
<td>create a new ad belonging to a specific magazine</td>
</tr>
<tr>
<td>GET</td>
<td>/magazines/1/ads/1</td>
<td>Ads</td>
<td>show</td>
<td>display a specific ad belonging to a specific magazine</td>
</tr>
<tr>
<td>GET</td>
<td>/magazines/1/ads/1/edit</td>
<td>Ads</td>
<td>edit</td>
<td>return an HTML form for editing an ad  belonging to a specific magazine</td>
</tr>
<tr>
<td>PUT</td>
<td>/magazines/1/ads/1</td>
<td>Ads</td>
<td>update</td>
<td>update a specific ad belonging to a specific magazine</td>
</tr>
<tr>
<td>DELETE</td>
<td>/magazines/1/ads/1</td>
<td>Ads</td>
<td>destroy</td>
<td>delete a specific ad belonging to a specific magazine</td>
</tr>
</tbody>
</table>
Que en hacen uso de las acciones:** index, new, create, show, edit, update y destroy**. Pero todas haciendo referencia a una magazine que la contiene. También nos genera los helpers: **magazine_ads_url**, **edit_magazine_ad_path**, etc.
¿Cómo viajan los parámetros? Si ingresamos una URL: magazines/1/ads2 tendríamos un:

{% highlight ruby %}
params = {"magazine_id"=>"1", "action"=>"show", "id"=>"2", "controller"=>"ads"}
{% endhighlight %}

Otra forma de generar los links (en un link_to) es con arreglos:

{% highlight ruby %}
<%= link_to "Ad details", magazine_ad_path(@magazine, @ad) %># un link a show
{% endhighlight %}

También podemos usar :name_prefix para configurar un poco más nuestras rutas anidadas:
1) Cambiarle el nombre

{% highlight ruby %}
map.resources :magazines do |magazine| 
  magazine.resources :ads, :name_prefix => 'periodical' 
end
{% endhighlight %}

2) Ocultar el prefijo:

{% highlight ruby %}
map.resources :magazines do |magazine| 
  magazine.resources :ads, :name_prefix => nil 
end
{% endhighlight %}

nota: en este último caso necesitaremos pasar por parámetros el id del magazine en los helpers:** ads_url(@magazine)** y **edit_ad_path(@magazine, @ad)**.
**
Resources únicos:**

{% highlight ruby %}
map.resources :photos do |photo|
  photo.resource :photographer
end
# o la forma corta:
# map.resources :photos, :has_one => :photographer
{% endhighlight %}

Por último, cuando queremos crear un nuevo elemento, ¿Cómo armamos el formulario? pues con:

{% highlight ruby %}
<% form_for([ @magazine, @ad ]) do |f| %>
{% endhighlight %}

[1] [http://guides.rubyonrails.org/routing.html](http://guides.rubyonrails.org/routing.html)
