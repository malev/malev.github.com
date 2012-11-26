--- 
layout: post
title: attaching files in desktopcouch
wordpress_id: 269
wordpress_url: http://blog.malev.com.ar/?p=269
comments: true
categories: 
- title: couchdb
  slug: couchdb
  autoslug: couchdb
- title: Programacion
  slug: programacion
  autoslug: programacion
- title: Python
  slug: python
  autoslug: python
tags: 
- title: couchdb
  slug: couchdb
  autoslug: couchdb
- title: desktopcouch
  slug: desktopcouch
  autoslug: desktopcouch
- title: Python
  slug: python
  autoslug: python
---
{{ page.title }}
================
Hola, les cuento que estoy programando en un proyecto muy interesante, en otro post les cuento bien de que se trata.
En el mismo teníamos la necesidad de adjuntar un archivo a una base de datos tipo CouchDB. Según el libro [0], el procedimiento es muy simple, pero como ahora me **_las doy de_** opportunistic developer [1] me decidí por desktopcouch. Buscando en python-snippets (acire)[2] encontre el siguiente _recorte_:

{% highlight python %}
import sys
from desktopcouch.records.record import Record
from desktopcouch.records.server import CouchDatabase

# get the jpg to add from the command line

if len(sys.argv) > 1:
    path = sys.argv[0]
    db = CouchDatabase("addattachment", create=True)
    record = Record(record_type="url")
    record.attach(path, "blob", "image/jpg")
    db.put_record(record)
else:
    print "Please pass the path of the jpg to add."

# got to /home/$USER/.local/share/desktop-couch/couchdb.html to see the
# attached file
{% endhighlight %}

Pero... **no funciona** :( hay un gran detalle, no almacena el archivo. Consultando en #couchdb, tuve la suerte de encontrarme con Turophile [3], con quien nos pusimos a experimentar y descubrimos que el verdadero procedimiento consiste en usar **open**, y aquí la solución:

{% highlight python %}
>>> # Importar todo lo necesario, mirar arriba
>>> record = Record(record_type="url")
>>> # Aquí viene la magia
>>> record.attach(open("/home/malev/pic.jpg","r"),"pic.jpg","image/jpg")
>>> id = db.put_record(record)
>>> rec = db.get_record(id)
>>> pic = open("/home/malev/pic_out.jpg","w")
>>> pic.write(rec.attachment_data(rec.list_attachments()[0])[0])
>>> pic.close()
>>> # LISTO!!
{% endhighlight %}

Y ahora si vamos... yo voy a mi home, tengo el archivo _recuperado_ :)
[0] [http://books.couchdb.org/relax/](http://books.couchdb.org/relax/)
[1] [http://www.gnomejournal.org/article/94/unchaining-the-opportunistic-developer](http://www.gnomejournal.org/article/94/unchaining-the-opportunistic-developer)
[2] [http://aciresnippets.wordpress.com/](http://aciresnippets.wordpress.com/)
[3] [http://swixel.net](http://swixel.net)
