--- 
layout: post
title: ActiveRecord, a brief introduction - Part 4
wordpress_id: 550
wordpress_url: http://blog.malev.com.ar/?p=550
comments: true
categories: 
- title: rails
  slug: rails
  autoslug: rails
- title: Ruby
  slug: ruby
  autoslug: ruby
- title: ActiveRecord
  slug: activerecord
  autoslug: activerecord
tags: 
- title: rails
  slug: rails
  autoslug: rails
- title: ActiveRecord
  slug: activerecord
  autoslug: activerecord
- title: counter_cache
  slug: counter_cache
  autoslug: counter_cache
- title: size
  slug: size
  autoslug: size
---
{{ page.title }}
================
**6.- Counter cache**
En el mismo modelo con el que venimos trabajando, ahora queremos mostrar: los usuarios con la cantidad de posts de cada uno.
En el controlador podríamos tener algo como:

{% highlight ruby %}
# controller
@users = User.all :limit => 10

# view
<% @users.each do |user| %>
  <%= user.first_name> has: <%= user.posts.length %> posts
<% end %>
{% endhighlight %}

Esto generaría lo siguiente en el log:

{% highlight sql %}
Processing UsersController#index (for 127.0.0.1 at 2010-09-05 19:33:04) [GET]
  User Load (0.2ms)   SELECT * FROM `users` LIMIT 0, 10
  User Columns (0.7ms)   SHOW FIELDS FROM `users`
  SQL (0.1ms)   SELECT count(*) AS count_all FROM `users`
Rendering template within layouts/users
Rendering users/index
  Post Load (0.1ms)   SELECT * FROM `posts` WHERE (`posts`.user_id = 2) ORDER BY title
  Post Load (0.1ms)   SELECT * FROM `posts` WHERE (`posts`.user_id = 3) ORDER BY title
  Post Load (0.1ms)   SELECT * FROM `posts` WHERE (`posts`.user_id = 4) ORDER BY title
  Post Load (0.1ms)   SELECT * FROM `posts` WHERE (`posts`.user_id = 5) ORDER BY title
  Post Load (0.1ms)   SELECT * FROM `posts` WHERE (`posts`.user_id = 6) ORDER BY title
  Post Load (0.1ms)   SELECT * FROM `posts` WHERE (`posts`.user_id = 7) ORDER BY title
  Post Load (0.1ms)   SELECT * FROM `posts` WHERE (`posts`.user_id = 8) ORDER BY title
  Post Load (0.1ms)   SELECT * FROM `posts` WHERE (`posts`.user_id = 9) ORDER BY title
  Post Load (0.1ms)   SELECT * FROM `posts` WHERE (`posts`.user_id = 10) ORDER BY title
  Post Load (0.1ms)   SELECT * FROM `posts` WHERE (`posts`.user_id = 11) ORDER BY title
Completed in 74ms (View: 61, DB: 2) | 200 OK [http://localhost/users]
{% endhighlight %}

Como vemos, hacemos una consulta para cada cuenta de posts. Y encima obtenermos **TODA** la información de cada post para recién ejecutar la cuenta en ruby a través del método length de la lista.** COMPLEMENTE INEFICIENTE!**.
Mejora 1: En vez de llamar al método **lenght**, llamamos **count**, así la "cuenta" de la cantidad de posts es realizada por el motor de bases de datos.
Mejora 2: Implementamos **counter_cache**

6.1 **Implementamos counter_cache**
Configuramos el model post.rb para que acepte counter_cache:

{% highlight ruby %}
# post.rb
class Post < ActiveRecord::Base
  belongs_to :user, :counter_cache => true
end
{% endhighlight %}

También agregamos una columna a la tabla users que guarda la cantidad de posts que tiene cada usuario. En la misma migración actualizamos la cuenta de los usuarios ya existentes en la tabla.

{% highlight ruby %}
class AddCounterCacheToUsers < ActiveRecord::Migration
  def self.up
    add_column :users, :posts_count, :integer, :default => 0

    User.reset_column_information
    User.all do |user|
      User.update_counters user.id, :posts_count => u.posts.count
    end
  end

  def self.down
    remove_column :users, :posts_count
  end
end
{% endhighlight %}


Por último, debemos cambiar nuestra vista a:
**RECORDAR**: usar el método **size** en vez de length o count. Si usamos count, AR ejecutará las consultas de cuenta y si usamos length (un método de la clase Array), AR traerá todos los posts de cada usuario y luego contará en ruby la longitud del arreglo (la forma más más ineficiente).

{% highlight ruby %}
<% @users.each do |user| %>
  <%= user.first_name> has: <%= user.posts.size %> posts
<% end %>
{% endhighlight %}

Nuestra salida por consola quedaría:

{% highlight sql %}
Processing UsersController#index (for 127.0.0.1 at 2010-09-05 20:31:44) [GET]
  User Load (0.4ms)   SELECT * FROM `users` LIMIT 0, 10
  User Columns (0.9ms)   SHOW FIELDS FROM `users`
  SQL (0.7ms)   SELECT count(*) AS count_all FROM `users`
Rendering template within layouts/users
Rendering users/index
Completed in 136ms (View: 56, DB: 2) | 200 OK [http://localhost/users]
{% endhighlight %}

Como podemos ver, optimizamos todo a una única consulta a la base de datos :)

**Referencias**
[http://apidock.com/rails/ActiveRecord/Base/update_counters/class](http://apidock.com/rails/ActiveRecord/Base/update_counters/class)
[http://asciicasts.com/episodes/23-counter-cache-column](http://asciicasts.com/episodes/23-counter-cache-column)
[http://apidock.com/rails/ActiveRecord/Associations/AssociationCollection/size](http://apidock.com/rails/ActiveRecord/Associations/AssociationCollection/size)
