--- 
layout: post
title: Bang methods in ActiveRecord
wordpress_id: 447
wordpress_url: http://blog.malev.com.ar/?p=447
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
- title: rubyonrails
  slug: rubyonrails
  autoslug: rubyonrails
- title: ActiveRecord
  slug: activerecord
  autoslug: activerecord
- title: validations
  slug: validations
  autoslug: validations
---
{{ page.title }}
================
Leyendo [1] descubrí la diferencia entre save y save! en **ActiveRecord**:

{% highlight ruby %}
@user #=> ActiveRecord Instance
@user.save  # También podría ser: @user.update_atributes(params)
@user.save! # =>  @user.update_atributes!(params)
{% endhighlight %}

La 2da opción (línea 3) llama al método save bang (!) [2] a diferencia de la línea 2 que llama al save común. **¿Cuál es la diferencia?** La respuesta viene de la mano de las validaciones (validations). Si @user no cumple con las validations, el save común devolverá false, mientras que save! levantará una excepción (ActiveRecord::RecordInvalid) [3].
Por otro lado, los métodos: create y update **devuelven el mismo objeto si se actualizó o no la base de datos**. Entonces cómo vamos a saber si un registro es válido para ser guardado, pues con el método: valid? O su gemelo malvado: invalid?

**¿Cómo saltearnos las validaciones?**

{% highlight ruby %}
@objeto.save(false)
{% endhighlight %}

[1] [http://guides.rubyonrails.org/activerecord_validations_callbacks.html](http://guides.rubyonrails.org/activerecord_validations_callbacks.html)
[2] [https://wincent.com/wiki/Ruby_%22bang%22_methods](https://wincent.com/wiki/Ruby_%22bang%22_methods)
[3] [http://apidock.com/rails/ActiveRecord/Base/save!](http://apidock.com/rails/ActiveRecord/Base/save!)
