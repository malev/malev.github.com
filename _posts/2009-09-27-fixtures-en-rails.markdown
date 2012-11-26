--- 
layout: post
title: Fixtures en Rails
wordpress_id: 38
wordpress_url: http://blog.malev.com.ar/?p=38
comments: true
categories: 
- title: gems
  slug: gems
  autoslug: gems
- title: rails
  slug: rails
  autoslug: rails
- title: Ruby
  slug: ruby
  autoslug: ruby
tags: 
- title: faker
  slug: faker
  autoslug: faker
- title: fixtures
  slug: fixtures
  autoslug: fixtures
- title: populator
  slug: populator
  autoslug: populator
- title: rails
  slug: rails
  autoslug: rails
---
{{ page.title }}
================
¿Qué son los fixtures? Son datos de ejemplo que almacenamos en nuestra base de datos nada más que para realizar pruebas. Rails permite jugar con 2 tipos de fuxtures: YAML y CSV. Yo solo se del primero, así que solo de ese les voy a hablar. Los fixtures se almacenan en test/fixtures. Para cargalos a nuestra base de datos, solo basta hacer: rake db:fixtures:load. Si editamos uno, encontraremos algo así:
<blockquote>one:
  name: TV
  description: MyText
two:
  name: Literature
  description: MyText
</blockquote>
Rails, además nos permite usar erb. Esto es muy útil por ejemplo si necesitamos crear algunos usuarios de ejemplo y estamos utilizando un sistema de gestión de usuarios como authlogic. Ahí yo siempre hago algo como esto:
<blockquote><%
  u = User.new
  u.password = '1234'
%>

one:
  username: admin
  email: admin@myapp.com
  crypted_password: "<%= u.crypted_password %>"
  password_salt: "<%= u.password_salt %>"
  persistence_token: "<%= u.persistence_token %>"

</blockquote>
Ahora, qué pasa cuando tenemos modelos con asociaciones, por ejemplo: Tenemos un modelo users y un modelo products. Como se imaginarán, un user puede tener muchos products (has_many). Cómo se cargarían estos fixtures en nuestra base de datos?
<blockquote># users.yml
user1:
  username: admin
  email: admin@myapp.com
user2:
  username: matz
  email: matz@ruby.org

# products.yml
product1:
  user_id: user1
  name: PC
  price: 999.99
product2:
  user_id: user2
  name: macbook
  price: 1599.99
</blockquote>
¿Dónde está la magia? Para indicar que el product2 le pertenece a matz, solo basta indicarle en el user_id el nombre que nosotros le dimos antes al otro fixture. Esta es una manera muy simple de usar fixtures cuando tenemos asociaciones en nuestra base de datos!

**Populator y faker**
Dos gemas muy interesante para rellenar nuestra base de datos con múltiples ejemplos. [Populator](http://populator.rubyforge.org/) nos permite crear una suerte de ciclos para rellenar la base de datos y [faker](http://faker.rubyforge.org/) genera código con cierta coherencia, por ejemplo nos puede generar direcciones de correo electrónico que pasarán nuestras validaciones, direcciones postales, nombres reales, etc. [RyanB](http://railscasts.com/episodes/126-populating-a-database) nos deja un tutorial de como usarlos. Yo aquí les presento un [ejemplo](http://gist.github.com/194975) con sus explicaciones comentadas.
