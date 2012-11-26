--- 
layout: post
title: "ActiveRecord, a brief introduction \xE2\x80\x93 Part 5"
wordpress_id: 568
wordpress_url: http://blog.malev.com.ar/?p=568
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
- title: Ruby
  slug: ruby
  autoslug: ruby
- title: ActiveRecord
  slug: activerecord
  autoslug: activerecord
- title: has_many
  slug: has_many
  autoslug: has_many
---
{{ page.title }}
================
**7.- Many to Many association**
Aquí vamos a usar un ejemplo típico: Posts y Categories.
ActiveRecord nos ofrece dos opciones para hacer esta asociación:
**:has_and_belongs_to_many** y **:has_many :trought**

**7.1. :has_and_belongs_to_many**
Creamos una tabla intermedia, sin ID, ya que AR no nos permite tener más de 2 columnas en nuestra tabla intermedia:

{% highlight ruby %}
create_table :categories_posts, :id => false do |t|
  t.integer :category_id
  t.integer :post_id
end
{% endhighlight %}

y agregamos a nuestros modelos la asociación:

{% highlight ruby %}
class Category < ActiveRecord::Base
  has_and_belongs_to_many :posts
end
class Post < ActiveRecord::Base
  has_and_belongs_to_many :categories
end
{% endhighlight %}

Esto nos permite trabajar con la asociación como si fuese un Array:

{% highlight ruby %}
p = Post.first
p.categories << c.create(:name => "Ruby")
{% endhighlight %}

Esta forma está buena y funciona, pero es muy limitada. Ya que por ejemplo no podemos agregar ninguna otra información a nuestra tabla intermedia.

**7.2. :has_many :trought**
Esta forma usa, además de una tabla intermendia, un modelo intermedio. No necesita el nombre de: "categories_posts", por lo que podemos bautizar nuestro modelo con un nombre mucho más intuitivo para nosotros. Además podemos agregar otras columnas a nuestra tabla intermedia, con lo que ganamos gran flexibilidad.

{% highlight ruby %}
$ ruby script/generate model categorization post_id:integer category_id:integer created_at:datetime
{% endhighlight %}


{% highlight ruby %}
class Categorization < ActiveRecord::Base
  belongs_to :post
  belongs_to :category
end

class Category < ActiveRecord::Base
  has_many :categorizations
  has_many :posts, :through => :categorizations
end

class Post < ActiveRecord::Base
  belongs_to :user, :counter_cache => true
  has_many :categorizations
  has_many :categories, :through => :categorizations
end
{% endhighlight %}


**7.3. Bibliografía**
[http://railscasts.com/episodes/47-two-many-to-many](http://railscasts.com/episodes/47-two-many-to-many)
