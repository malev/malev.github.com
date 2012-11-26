--- 
layout: post
title: Routes in rails 3
wordpress_id: 680
wordpress_url: http://blog.malev.com.ar/?p=680
comments: true
categories: 
- title: rails
  slug: rails
  autoslug: rails
- title: Ruby
  slug: ruby
  autoslug: ruby
- title: buenas practicas
  slug: buenas-practicas
  autoslug: buenas-practicas
tags: 
- title: rails
  slug: rails
  autoslug: rails
- title: Ruby
  slug: ruby
  autoslug: ruby
- title: routes
  slug: routes
  autoslug: routes
---
{{ page.title }}
================
Hace algunas semanas publiqué dos breves resumenes [1][2] de cómo hacer funcionar las rutas en rails 2.3. A los días de publicado el artículo rails 3 vió la luz. Con vistas a empezar a usar rails 3 arme este nuevo resumen con algunas diferencias entre las rutas en los distintos rails.

**Nuevas rutas:**

{% highlight ruby %}
GET /patients/17
match "/patients/:id" => "patients#show"
{% endhighlight %}


La principal diferencia entre rails 2 y rails 3 en materias de routes es **map**:

{% highlight ruby %}
# rails 2.3.5
ActionController::Routing::Routes.draw do |map|
  map.resources :photos
end
# rails 3
Miniblog::Application.routes.draw do
  resources :photos
end
{% endhighlight %}

Por supuesto son estos nuevos resources, seguimos teniendo todos nuestros helpers y rutas que teníamos antes.

**También podemos definir muchos recursos en una sola línea:**

{% highlight ruby %}
resources :photos, :books, :trees
{% endhighlight %}

**Recursos únicos: (new - show - create - edit - update - destroy)**

{% highlight ruby %}
resource :account
{% endhighlight %}

**Namespaces: (admin/posts - admin/comments)**

{% highlight ruby %}
namespace "admin" do
  resources :posts, :comments
end
{% endhighlight %}

**Nested resources (recursos anidados):**

{% highlight ruby %}
resources :magazines do
  resources :ads
end
{% endhighlight %}

**Armado de URL:**
1.- Usando un helper:

{% highlight ruby %}
link_to "Ad details", magazine_ad_path(@magazine, @ad)
{% endhighlight %}

2.- Usando url_for:

{% highlight ruby %}
link_to "Ad details", url_for(@magazine, @ad)
{% endhighlight %}

3.- Usando un arreglo:

{% highlight ruby %}
link_to "Ad details", [@magazine, @ad]
{% endhighlight %}


**Flash messages en la misma línea que un redirect**[5]

{% highlight ruby %}
# in a controller
redirect_to user_path(@user), :notice =>"The user was successfully created"
{% endhighlight %}

Si has usado rails para algo más que para jugar, sabrás que los resources así como están aquí son insuficientes. Por suerte disponemos de: members y collections.

{% highlight ruby %}
# member and collections
resources :photos do
  member do
    get "preview" # nótese el verbo: get
  end
  collection do
    get 'search'
   end
end
### --- ###
# member & collections - short way
resources :photos do
  get 'preview', :on => :member
  get 'search', :on => :collection
end
{% endhighlight %}


Yendo un poco más haya de los límites de RESTful.

**Rutas rígidas:**

{% highlight ruby %}
match 'photos/:id' => 'photos#show'[:via => :get]
# /photos/12
# :controller => 'photos', :actions => 'show'
# también se puede simplificar a:
get 'photos/show'
{% endhighlight %}

**Root url:**

{% highlight ruby %}
root :to => 'main#index'
# también dentro de un namespace:
namespace "admin" do
  root :to => 'admin#index'
  resources :posts, :comments
end
{% endhighlight %}

**Obteniendo algo de ayuda:**
Desde la consola podemos tirar: rake routes para que rails nos liste todas las rutas disponibles con sus correspondientes helpers. A veces esta lista crece demasiado es complicado buscar las rutas de un controlador específico. Ahí podemos usar:
<blockquote>$ CONTROLLER=users rake routes</blockquote>
El resto de las cosas es bastante similar a como era en rail 2.3. Por lo que siguen siendo válidos todos los otros artículos acerca de routes en rails.
**Resources**
[1] [http://blog.malev.com.ar/2010/07/15/routes-en-rails-restful/](http://blog.malev.com.ar/2010/07/15/routes-en-rails-restful/)
[2] [http://blog.malev.com.ar/2010/07/20/routes-rb-nested-resources/](http://blog.malev.com.ar/2010/07/20/routes-rb-nested-resources/)
[3] [http://railscasts.com/episodes/203-routing-in-rails-3](http://railscasts.com/episodes/203-routing-in-rails-3)
[4] [http://rubyonrails.org/screencasts/rails3/getting-started-action-dispatch](http://rubyonrails.org/screencasts/rails3/getting-started-action-dispatch)
[5] [http://ryandaigle.com/articles/2009/12/20/what-s-new-in-edge-rails-set-flash-in-redirect_to](http://ryandaigle.com/articles/2009/12/20/what-s-new-in-edge-rails-set-flash-in-redirect_to)
