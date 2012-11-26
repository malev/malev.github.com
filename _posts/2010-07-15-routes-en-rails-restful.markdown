--- 
layout: post
title: routes.rb en rails - RESTful
wordpress_id: 362
wordpress_url: http://blog.malev.com.ar/?p=362
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
- title: routes.rb
  slug: routes-rb
  autoslug: routes.rb
- title: restful
  slug: restful
  autoslug: restful
- title: routes
  slug: routes
  autoslug: routes
- title: rutas
  slug: rutas
  autoslug: rutas
---
{{ page.title }}
================
Varias veces necesito refrescar como trabajan el sistema de rutas **routes** de rails, por lo que aquí presento un resumen. Por supuesto, basado en el guía oficial de rails [1].
A.- El distpatcher de rails se encarga de 2 cosas: procesar las peticiones del browser y de generar helpers para el manejo de rutas internos del desarrollo.
B.- Tipos de rutas:


 * 1.- RESTful

 * 2.- Named routes

 * 3.- Nested routes

 * 4.- Regular routes

 * 5.- Default routes



**1.- RESTful routes**
Representational State Transfer es una técnica de arquitectura software para sistemas hipermedia  distribuidos como la World Wide Web. Forma parte de la tesis doctoral de Roy Fielding [2]. En lo que se refiere a rutas, RESTful se puede resumir (con el perdón de Roy) en:
1) Usar un identificador de recursos (las URL en nuestro caso)
2) Enviar una representación de la acción a hacer: GET, POST, PUT y DELETE.
ej DELETE /users/1

1.1 Uso

{% highlight ruby %}
ActionController::Routing::Routes.draw do |map|
  map.resources :photos
end
{% endhighlight %}


Esto nos habilitará:
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
<td>/photos</td>
<td>Photos</td>
<td>index</td>
<td>display a list of all photos</td>
</tr>
<tr>
<td>GET</td>
<td>/photos/new</td>
<td>Photos</td>
<td>new</td>
<td>return an HTML form for creating a new  photo</td>
</tr>
<tr>
<td>POST</td>
<td>/photos</td>
<td>Photos</td>
<td>create</td>
<td>create a new photo</td>
</tr>
<tr>
<td>GET</td>
<td>/photos/1</td>
<td>Photos</td>
<td>show</td>
<td>display a specific photo</td>
</tr>
<tr>
<td>GET</td>
<td>/photos/1/edit</td>
<td>Photos</td>
<td>edit</td>
<td>return an HTML form for editing a photo</td>
</tr>
<tr>
<td>PUT</td>
<td>/photos/1</td>
<td>Photos</td>
<td>update</td>
<td>update a specific photo</td>
</tr>
<tr>
<td>DELETE</td>
<td>/photos/1</td>
<td>Photos</td>
<td>destroy</td>
<td>delete a specific photo</td>
</tr>
</tbody>
</table>
Lo interesante es "action", ya que combinando los verbos con la forma de la URL podemos acceder a las acciones básicas de un registro: nuevo, crear, mostrar, editar, actualizar y borrar y por supuesto el index que en general se usa para mostrar un listado con todos los registros.

1.2 Traducido a visión de controller, tendríamos:

{% highlight ruby %}
class PhotosController < ActionController::Base
    # GET new_photo_url
    def new
      # return an HTML form for describing the new account
    end

    # POST photo_url
    def create
      # create an photo
    end

    # GET photo_url
    def show
      # find and return the photo
    end

    # GET edit_photo_url
    def edit
      # return an HTML form for editing the photo
    end

    # PUT photo_url
    def update
      # find and update the photo
    end

    # DELETE photo_url
    def destroy
      # delete the photo
    end
  end
{% endhighlight %}


1.3 ¿Cómo hacemos una petición DELETE o PUT? Los browsers no la soportan, pero podemos hacer:


{% highlight ruby %}
<%= link_to "delete", video, :method => "delete" %>
# En el caso de un form_tag
<%= form_tag('/posts/1', :method => :put) %>
# =>
{% endhighlight %}

1.4 Diferencias entre url y path:
<table>
<tbody>
<tr>
<th>helper
</th><th>resultado
</th></tr>
<tr>
<td>photos_path</td>
<td>/photos</td>
</tr>
<tr>
<td>photos_url</td>
<td>http://www.ejemplo.com/photos</td>
</tr>
<tr>
<td>new_photo_path</td>
<td>/photo/new</td>
</tr>
<tr>
<td>new_photo_url</td>
<td>http://www.ejemplo.com/photo/new
</td></tr>
</tbody>
</table>

1.5 Singular resources

Hasta ahora todo plural, pero a veces manejamos elementos que son y serán únicos:

{% highlight ruby %}
map.resource :avatar
{% endhighlight %}

Muy similar al anterior, pero ahora solo tenemos componentes en **singular**. Y tenemos los siguientes helpers:
new_avatar_X, edit_avatar_X, avatar_X (donde X puede ser: url o path). Dentro del controlador tendíamos las acciones: show, new, create, edit, update y destroy.

1.6 Customizing
Uno de los slogans de rails dice: Convention over Configuration - CoC - Convención sobre configuración. Pero que pasa cuando, tenemos que salirnos de la convención? Para esto son las opciones de configuración:
**:controller**
map.resources :adminphotos, :controller => 'nombre_del_controlador'
map.resources :adminphotos, :controller => 'admin/controlador'  # namespace
map.resources :adminphotos, :controller => '/admin/controlador' # fuerza ir al top-level
**:requeriments**
map.resources :photos, :requirements => {:id  => /[A-Z][A-Z][0-9]+/}  
Aquí el parámetro :id debe cumplir con la expresión regular.

**:conditions**
Define condiciones a las rutas:
map.connect 'post/:id', :controller => 'posts', :action => 'show', :conditions => { :method => :get }

**:path_prefix**
map.resources :photos, :path_prefix => 'photo/:photo_id'

# :name_prefix - Define a prefix for all generated routes, usually ending in an underscore. Use this if you have named routes that may clash.

map.resources :photos, :path_prefix => '/photographers/:photographer_id',  :name_prefix => 'photographer_' map.resources :photos, :path_prefix => '/agencies/:agency_id',  :name_prefix => 'agency_' 
Permite usar rutas como: /photographers/photographer_2, donde obvio id = 2 :)

**: only**
map.resources :photos, :only => [:index, :show] 
Solo son válidas las acciones index y show.

**:except**
map.resources :photos, :except => :destroy
Todas las acciones son válidas excepto por destroy.

1.7 Agregando más rutas [5]
La verdad es que con show, edit e index casi nunca es suficiente. En general necesitamos search, preview, etc. Cómo agregamos estas rutas?

{% highlight ruby %}
map.resources :photos, :collection => { :search => :get } #=> photos/search
map.resources :photos, :member => { :preview  => :get } #=> photos/1/preview
map.resources :photos, :new => {:draft => :get}        #=> photos/new/draft
{% endhighlight %}

Todas generan rutas nuevas que aputan a diferentes actions del controlador. La primera apunta a search, la 2da a preview (y admite un id como parámetro) y la tercera a draft. Todas arman sus helpers con terminaciones en url y path: search_photos_X, preview_photo(), draft_photos. Por otro lado, todas se pueden configurar con diferentes verbos. Cambiando ":get" por [:get, :put] o lo que sea.
Hasta aquí la primer parte de este pequeño ayuda memoria. Con los que a mi entender, son las opciones más comunes del RESTful.
Por último los dejo con este [3] video, donde los chicos de rails envy nos recomiendan: Stay RESTful y con este [4] tutorial sobre RESTful y Rails que estoy por leer en estos días :)

[1] [http://guides.rubyonrails.org/routing.html](http://guides.rubyonrails.org/routing.html)
[2] [http://es.wikipedia.org/wiki/Representational_State_Transfer](http://es.wikipedia.org/wiki/Representational_State_Transfer)
[3] [http://www.youtube.com/watch?v=p30dcETXwD4&feature=related](http://www.youtube.com/watch?v=p30dcETXwD4&feature=related)
[4] [http://sobrerailes.com/2007/06/20/tutorial-desarrollo-rest-con-ruby-on-rails/](http://sobrerailes.com/2007/06/20/tutorial-desarrollo-rest-con-ruby-on-rails/)
[5] [http://stackoverflow.com/questions/1667146/whats-the-difference-between-new-collection-and-member-routes](http://stackoverflow.com/questions/1667146/whats-the-difference-between-new-collection-and-member-routes)
