--- 
layout: post
title: ActiveRecord, a brief introduction - Part 3
wordpress_id: 529
wordpress_url: http://blog.malev.com.ar/?p=529
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
- title: named_scope
  slug: named_scope
  autoslug: named_scope
---
{{ page.title }}
================
**named_scope** [1][2][3]
Nos permite generar "métodos"[4] de clases para ejecutar consultas, de una manera muy legible.

5.1) Supongamos que siempre queremos los post de usuario con id = 3. Haríamos:

{% highlight ruby %}
p = Post.all :conditions => {:user_id => 3}
{% endhighlight %}

También queremos todos los post que están en estado: searchable

{% highlight ruby %}
p = Post.all :conditions => {:state => "searchable"}
{% endhighlight %}

Pero, también podríamos crear un named_scope en el model:

{% highlight ruby %}
# post.rb
class Post < ActiveRecord::Base
  belongs_to :user
  named_scope :three, :conditions => {:user_id => 3}
  named_scope :searchable, :conditions => {:state => 'searchable'}
end

[Development]>> reload!
Reloading...
=> true
[Development]>> p = Post.three
  SQL (0.1ms)   SET NAMES 'utf8'
  SQL (0.1ms)   SET SQL_AUTO_IS_NULL=0
  Post Load (0.1ms)   SELECT * FROM `posts` WHERE (`posts`.`user_id` = 3)
  Post Columns (1.2ms)   SHOW FIELDS FROM `posts`
...
[Development]>> p = Post.searchable
  SQL (0.1ms)   SET NAMES 'utf8'
  SQL (0.1ms)   SET SQL_AUTO_IS_NULL=0
  Post Load (49.9ms)   SELECT * FROM `posts` WHERE (`posts`.`state` = 'searchable')
  Post Columns (0.9ms)   SHOW FIELDS FROM `posts`
...
{% endhighlight %}


5.2) También podemos mezclar dos (o los que queramos) named_scope

{% highlight ruby %}
[Development]>> p = Post.searchable.three
  Post Load (66.5ms)   SELECT * FROM `posts` WHERE ((`posts`.`state` = 'searchable' AND `posts`.`user_id` = 3))
...
{% endhighlight %}


5.3) Ahora algo con fechas:
Queremos últimos posts, es decir aquellos que fueron creados hasta 2 semanas atras:

{% highlight ruby %}
class Post < ActiveRecord::Base
  belongs_to :user
  named_scope :recent, :conditions => { :created_at > 2.weeks.ago }
end
{% endhighlight %}

En la consola:

{% highlight ruby %}
[Development]>> p = Post.recent.count
  SQL (15.4ms)   SELECT count(*) AS count_all FROM `posts` WHERE (created_at > '2010-08-16 01:52:58')
=> 12029
[Development]>> puts "dejo pasar unos minutos"
dejo pasar unos minutos
=> nil
[Development]>> puts Time.now
Sun Aug 29 22:53:20 -0300 2010
=> nil
[Development]>> p = Post.recent.count
  SQL (0.2ms)   SELECT count(*) AS count_all FROM `posts` WHERE (created_at > '2010-08-16 01:52:58')
=> 12029
{% endhighlight %}

Como vemos el 2.weeks.ago no actualizó a los minutos que dejamos pasar. Esto se debe a que el 2.weeks.ago se ejecutó una sola vez, cuando se cargo la consola y se ejecutó el modelo. Para solucionar este problema y que el 2.weeks.ago sea la fecha que nosotros queremos que sea, tenemos que usar una función lambda que se ejecute cada vez que la llamamos:

{% highlight ruby %}
class Post < ActiveRecord::Base
  belongs_to :user
  named_scope :recent, lambda { {:conditions => ["created_at > ?", 2.weeks.ago] } }
end

[Development]>> p = Post.recent.count
  SQL (17.1ms)   SELECT count(*) AS count_all FROM `posts` WHERE (created_at > '2010-08-16 01:58:44')
=> 12029
[Development]>> puts "vuelvo a dejar pasar unos minutos..."
vuelvo a dejar pasar unos minutos...
=> nil
[Development]>> p = Post.recent.count
  SQL (13.4ms)   SELECT count(*) AS count_all FROM `posts` WHERE (created_at > '2010-08-16 02:01:46')
=> 12029
{% endhighlight %}

Y el tiempo se ha actualizado :)

5.4) Pasando parámetros a nuestros named_scope
Ahora nosotros queremos decirle a AR cuán recientes tienen que ser nuestros posts:

{% highlight ruby %}
class Post < ActiveRecord::Base
  belongs_to :user
  named_scope :recent, lambda { {:conditions => ["created_at > ?", 2.weeks.ago] } }
  named_scope :our_recent, lambda{ |time| {:conditions => ["created_at > ?", time] } }
end

>> p = Post.our_recent(2.days.ago).count
  SQL (17.4ms)   SELECT count(*) AS count_all FROM `posts` WHERE (created_at > '2010-08-28 02:38:46')
=> 12029
{% endhighlight %}

5.5) Usando parámetros opcionales

{% highlight ruby %}
class Post < ActiveRecord::Base
  belongs_to :user
  named_scope :our_recent_two, lambda { |*args| {:conditions => ["released_at > ?", (args.first || 2.weeks.ago)]} }
end
[Development]>> p = Post.our_recent_two.count
  SQL (17.1ms)   SELECT count(*) AS count_all FROM `posts` WHERE (created_at > '2010-08-16 02:41:15')
=> 12029
[Development]>> p = Post.our_recent_two(1.hour.ago).count
  SQL (10.6ms)   SELECT count(*) AS count_all FROM `posts` WHERE (created_at > '2010-08-30 01:41:24')
=> 0
{% endhighlight %}

**nota:** el *, es un operador que convierte todos nuestros parámetros en un arreglo. Algo propio de Ruby.

5.6) Scope por defecto:
Ya hemos visto de esto el la parte de **order**. Por ejemplo, en el model:

{% highlight ruby %}
default_scope :order => "title"
# también puede ser algo más complicado:
default_scope :order => 'last_name, first_name'
{% endhighlight %}


[1] http://apidock.com/rails/ActiveRecord/NamedScope/ClassMethods/named_scope
[2] http://ryandaigle.com/articles/2008/11/18/what-s-new-in-edge-rails-default-scoping
[3] http://railscasts.com/episodes/108-named-scope
[4] La verdad es que no son métodos, pero lo parecen :)
