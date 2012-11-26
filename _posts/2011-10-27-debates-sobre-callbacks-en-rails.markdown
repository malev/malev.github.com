--- 
layout: post
title: Sobre callbacks en Rails
wordpress_id: 896
wordpress_url: http://blog.malev.com.ar/?p=896
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
- title: Ruby
  slug: ruby
  autoslug: ruby
- title: callbacks
  slug: callbacks
  autoslug: callbacks
- title: after_create
  slug: after_create
  autoslug: after_create
- title: active_record
  slug: active_record
  autoslug: active_record
---
{{ page.title }}
================
¿Qué son los **callbacks**? Son métodos que son llamados en determinados momentos del ciclo de vida de un objeto. Esto nos permite escribir código que se va a ejecutar cuando nuestros **objetos** (**ActiveRecord** u otros) son creados, grabados, actualizados, borrados, validados o cargados desde la base de datos (created, saved, updated, deleted, validated, or loaded).
Supongamos que nosotros queremos que inmediatamente después de haber creado un usuario, se cree automáticamente una cuenta para ese usuario. Podríamos hacer esto (por favor no lo intenten en sus casas, puede ser peligroso):

{% highlight ruby %}
class UsersController < ApplicationController
  def create
    @user = User.new(params[:user])
    @account = Account.create :user => @user
  end
end
{% endhighlight %}

Sin embargo, estaríamos contaminando nuestro **controller** con lógica de modelo. Una primera aproximación, podría ser, usar un callback: after_create. Se ejecuta siempre que se crea un objeto y se lo guarda en la base por primera vez:

{% highlight ruby %}
class User < ActiveRecord::Base
  #...
  def after_create
    Account.create :user => self
  end
  #...
end
{% endhighlight %}

Esta aproximación funciona y está buena, pero tiene algunos problemillas: el nombre del método no describe lo que hace y si tenemos que hacer varias cosas, tendríamos un método gigante. Aquí entran en acción el método de clase: after_create:

{% highlight ruby %}
class User < ActiveRecord::Base
  #...
  after_create :create_account
  
  def create_account
    Account.create :user => self
  end
  #...
end
{% endhighlight %}

Una vez creado el objeto, ActiveRecord buscará un método llamado "**create_account**" y lo ejecutará. Esto nos permite tener varios métodos que se ejecuten como callbacks, he inclusive podemos llamarlos de manera secuencial:

{% highlight ruby %}
class Snippet < ActiveRecord::Base
  after_create :test1
  after_create :test2

  def test1
    puts "test 1"
  end

  def test2
    puts "test 2"
  end
end
{% endhighlight %}

y los resultados:

{% highlight ruby %}
ruby-1.9.2-p136 :002 > s = Snippet.new
 => #<Snippet id: nil, content: nil, user_id: nil... private: false> 
ruby-1.9.2-p136 :003 > s.save
test 1
test 2
 => true 
ruby-1.9.2-p136 :004 >
{% endhighlight %}

Por supuesto, after_create no es el único callback. Hay mucha información al respecto [aquí](http://guides.rubyonrails.org/active_record_validations_callbacks.html#callbacks-overview). Aquí solo voy a listar los callbacks disponibles:

**Creando un objeto**

* before_validation
* after_validation
* before_save
* before_create
* around_create
* after_create
* after_save

**Actualizando un objeto**

* before_validation
* after_validation
* before_save
* before_update
* around_update
* after_update
* after_save

**Eliminando un objeto**

* before_destroy
* after_destroy
* around_destroy

Su uso es ampliamente recomendable y una cosa a tener en cuenta es... no podemos evitarlos! No son como las validaciones, en las que a veces podemos no ejecutarlas. Aquí si alguna vez necesitamos saltearlos algún callback, vamos a tener que repensar la lógica del modelo de vuelta. Usar con responsabilidad.
