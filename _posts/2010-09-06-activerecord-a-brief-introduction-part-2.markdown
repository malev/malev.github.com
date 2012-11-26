--- 
layout: post
title: ActiveRecord, a brief introduction - Part 2
wordpress_id: 520
wordpress_url: http://blog.malev.com.ar/?p=520
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
- title: order
  slug: order
  autoslug: order
---
{{ page.title }}
================
**Preguntas en ActiveRecord**
3.0) ActiveRecord está conectado?

{% highlight ruby %}
User.connected? # => true
{% endhighlight %}

3.1) Change

{% highlight ruby %}
u = User.first
u.changed? # => false
u.first_name = "NewName" # => "NewName"
u.changed? # => true
{% endhighlight %}

3.1.2) ¿Qué cambió?

{% highlight ruby %}
u.changed # => ["first_name"]
{% endhighlight %}

3.1.3) ¿Cuáles son los cambios?

{% highlight ruby %}
u.changes # => {"first_name"=>["Enrique", "NewName"]}
{% endhighlight %}

3.2) ¿Cómo preguntar si guardamos los cambios?
No existe un método saved?, pero **save** nos devuelve true pudimos guardar en la base de datos y false si por alguna razón no pudimos guardar (en gral, no cumplimos con las validaciones).

{% highlight ruby %}
>> u.first_name = "Elisa" # => "Elisa"
>> u.save
  SQL (0.1ms)   BEGIN
  User Update (0.4ms)   UPDATE `users` SET `first_name` = 'Elisa' WHERE `id` = 2
  SQL (35.8ms)   COMMIT
=> true
{% endhighlight %}

3.5) ¿Lo eliminamos?

{% highlight ruby %}
u.last
u.destroyed? # => nil
u.destroy
  SQL (0.2ms)   BEGIN
  User Destroy (0.4ms)   DELETE FROM `users` WHERE `id` = 1
  SQL (54.4ms)   COMMIT
u.destroyed? # => true
{% endhighlight %}

3.6) Trabajando con nuevos elementos

{% highlight ruby %}
>> u = User.new :first_name => "Antonia"
+------------+-----------+--------+
| first_name | last_name | email  |
+------------+-----------+--------+
| Antonia    |           |        |
+------------+-----------+--------+
1 row in set
>> u.attribute_present? :last_name
=> false
>> u.new_record?
=> true
>> u.save
  SQL (0.1ms)   BEGIN
  User Create (0.4ms)   INSERT INTO `users` (`last_name`, `first_name`, `email`) VALUES(NULL, 'Antonia', NULL)
  SQL (43.3ms)   COMMIT
=> true
>> u.new_record?
=> false
{% endhighlight %}

3.6) Existe ese atributo

{% highlight ruby %}
>> u.has_attribute? :phone_number # => false
{% endhighlight %}

Este último punto no es muy utilizado, pero está bueno saber de su existencia.

**Order**
4.1) Especificar el orden en la consulta:

{% highlight ruby %}
>> u = User.all :limit => 10, :order => 'last_name'
  User Load (64.3ms)   SELECT * FROM `users` ORDER BY last_name LIMIT 10
>> p = Post.all :order => "created_at DESC"
  Post Load (116.9ms)   SELECT * FROM `posts` ORDER BY created_at DESC
# OR Post.all :order => "created_at ASC"
{% endhighlight %}

4.2) Ordenes consecutivos (así se dice?)
Primero se ordena por fecha de creación y luego por título

{% highlight ruby %}
>> p = Post.all :limit => 10, :order => "created_at DESC, title"
  Post Load (53.4ms)   SELECT * FROM `posts` ORDER BY created_at DESC, title LIMIT 10
>> u = User.all :order => ["first_name ASC, email DESC"]
  User Load (30.0ms)   SELECT * FROM `users` ORDER BY first_name ASC, email DESC
{% endhighlight %}

4.3) Orden por defecto:
En el modelo agregamos:

{% highlight ruby %}
class User < ActiveRecord::Base
  default_scope :order => 'last_name, first_name'
end
{% endhighlight %}

4.4) Saltando el orden por defecto:

{% highlight ruby %}
$ >> p = Post.all :with_exclusive_scope, :limit => 10
SELECT * FROM `posts` ORDER BY title LIMIT 10
{% endhighlight %}

Y todas nuestras consultas tendrán ese orden.
