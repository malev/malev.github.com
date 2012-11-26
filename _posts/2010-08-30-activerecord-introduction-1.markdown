--- 
layout: post
title: ActiveRecord, a brief introduction - Part 1
wordpress_id: 497
wordpress_url: http://blog.malev.com.ar/?p=497
comments: true
categories: 
- title: rails
  slug: rails
  autoslug: rails
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
- title: queries
  slug: queries
  autoslug: queries
---
{{ page.title }}
================
**Active record** es un patrón de diseño. Es un enfoque al problema de acceder a los datos de una base de datos. Donde cada tabla es una clase por lo que cada fila es asociada con objetos del lenguaje de programación usado. Cuando se crea uno de estos objetos, se añade una fila a la tabla de la base de datos. Cuando se modifican los atributos del objeto, se actualiza la fila de la base de datos.[1]

**1.- Usos super básicos**[2]

{% highlight ruby %}
# == Schema Information
# Schema version: 20100829195102
#
# Table name: users
#
#  id         :integer(4)      not null, primary key
#  first_name :string(255)
#  last_name  :string(255)
#  email      :string(255)
#

class User < ActiveRecord::Base
  has_many :posts
end

# == Schema Information
# Schema version: 20100829195102
#
# Table name: posts
#
#  id         :integer(4)      not null, primary key
#  title      :string(255)
#  body       :text
#  state      :string(255)
#  user_id    :integer(4)
#  created_at :datetime
#  updated_at :datetime
#

class Post < ActiveRecord::Base
  belongs_to :user
end
{% endhighlight %}

**Agregando datos:**
1.1) Atributo por atributo

{% highlight ruby %}
u = User.new
u.first_name = "Marcos"
u.last_name = "Vanetta"
u.email = "marcos@correo.com"
u.save
{% endhighlight %}

1.2) Pasando todos los atributos con un diccionario

{% highlight ruby %}
u2 = User.create :first_name => "John", :last_name => "Doe", :email => "johndoe@correo.com"
{% endhighlight %}

**nota:** Aquí no es necesario el save

1.3) Actualizando un atributo en particular:

{% highlight ruby %}
u.update_attribute :email, "johndoe@gmail.com"
{% endhighlight %}

Nota: Este procedimiento **no incurre en validaciones**, vea también [3]

1.4) Actualizando con un diccionario:

{% highlight ruby %}
u = User.first
u.update_attributes :first_name => "Dexter", :last_name => "Morgan", :email => "dexter@morgan.com"
{% endhighlight %}

nota: update_attributes! Es igual al anterior, pero sin validaciones.

1.5) Eliminar un registro:

{% highlight ruby %}
u = User.last
u.destroy
{% endhighlight %}


**Búsquedas simples**
2.1) Obtener todos los registros:

{% highlight ruby %}
users = User.find(:all)
users = User.all # => Alias para el anterior
{% endhighlight %}

nota: Nunca recomendable. Si hay una gran cantidad de registros se puede dar un gran consumo de memoria.

{% highlight ruby %}
users = User.all :limit => 10, :offset => 0
# Muestra los primeros 10. Manipulando :limit y :offset, se pude obtener un paginado de los recursos.
{% endhighlight %}

2.2) Obtener registros en particular (x ID)

{% highlight ruby %}
u = User.first
u = User.last
u = User.find 7
u = User.find [7, 9 , 10] # Entrega 3 elementos con ID: 7, 9 y 10
{% endhighlight %}

2.2.1) Tipos de respustas
Ante cada consulta AR nos puede devolver 2 tipos de objetos: un objeto AR (el que buscamos o solicitamos). O un arreglo de objetos AR:

{% highlight ruby %}
u = User.first
u.class.to_s # => "User"

u = User.find [5,6,7]
u.class # => Array
{% endhighlight %}

Esto es muy importante a la hora de armar las vistas. Si nosotros solicitamos un objeto y AR no lo encuentra, nos devuelve "**nil**", por lo que tendremos que contemplar esta posibilidad en la vista. En cambio cuando hacemos una solicitud de un arreglo, AR nos puede devolver: un arreglo de objetos (**array**), un arreglo con un único elemento o un arreglo vacío. Aquí no suelen aparecer problemas ya que en genera en la vista usamos: objeto.each do ... y en caso de que el arreglo este vacío, no se da ninguna iteración, pero tampoco salta ninguna exeption.

2.3) Usando dynamic finders
En pos de la legibilidad rails genera "buscadores dinámicos" asociados a los atributos de la tabla:

{% highlight ruby %}
# Devuelven un objeto AR
u = User.find_by_email "marcos@correo.com"
u = User.find_by_last_name "Vanetta"
u = User.find_by_first_name "Marcos"

# Devuelven un arreglo de objetos AR
p = Post.find_all_by_title "Et"
{% endhighlight %}

2.4) Búsquedas con condiciones

{% highlight ruby %}
u = User.find :first_name => "Marcos"
u = User.find :all, :conditions => ["first_name = ?", "Natalie"]
u = User.all :conditions => ["first_name = ?", "Natalie"]
u = User.all :conditions => ["first_name = ? AND last_name = ?", "Natalie", "Erdman"]
u = Post.all :limit => 10, :offset => 0, :conditions => ["state <> ?", "unsearchable"]
{% endhighlight %}

2.5) Más condiciones

{% highlight ruby %}
p = Post.all :limit => 10, :offset => 0, :conditions => ["state <> ? AND body LIKE ?", "unsearchable", "%Texto%"]
p = Post.all :conditions => ["user_id = ? AND state = ?", "2", "draft"]
{% endhighlight %}

2.6) Condiciones con un Array
p = Post.all :limit => 10, :conditions => ["state IN (?)", ["published", "draft"]]
Podemos notar tuvimos que agregar el IN(?) para que la consulta se haga de manera correcta.

2.7) Conditions' hash

{% highlight ruby %}
>> p = Post.all :conditions => {:state => ["published", "draft"]}
  Post Load (57.6ms)   SELECT * FROM `posts` WHERE (`posts`.`state` IN ('published','draft'))
{% endhighlight %}

**Y, esta es la posta! **Por que no necesitamos aclarar en la consulta si es un = on un IN.
Otro ejemplo:

{% highlight ruby %}
>> u = User.all :conditions => {:last_name => nil}
  User Load (2.1ms)   SELECT * FROM `users` WHERE (`users`.`last_name` IS NULL)
{% endhighlight %}


2.8) Range conditions

{% highlight ruby %}
>> p = Post.all :conditions => {:created_at => (Time.now.midnight - 1.day)..Time.now.midnight}
    Post Load (25.8ms)   SELECT * FROM `posts` WHERE (`posts`.`created_at` BETWEEN '2010-08-28 00:00:00' AND '2010-08-29 00:00:00') 
>> p = Post.all :conditions => {:user_id => (3..4)}
  Post Load (27.5ms)   SELECT * FROM `posts` WHERE (`posts`.`user_id` BETWEEN 3 AND 4)
{% endhighlight %}

Esta es la primer entrega de este mini tutorial. La idea es hacer un repaso de AR desde cosas super básicas como estas, hasta algunas un poco más avanzadas.

[1][ http://es.wikipedia.org/wiki/Patr%C3%B3n_ActiveRecord](http://es.wikipedia.org/wiki/Patr%C3%B3n_ActiveRecord)
[2] [http://api.rubyonrails.org/classes/ActiveRecord/Base.html](http://api.rubyonrails.org/classes/ActiveRecord/Base.html)
[3] [http://blog.malev.com.ar/2010/08/04/bang-methods-in-activerecord/](http://blog.malev.com.ar/2010/08/04/bang-methods-in-activerecord/)
[4] [http://railscasts.com/episodes/15-fun-with-find-conditions](http://railscasts.com/episodes/15-fun-with-find-conditions)
