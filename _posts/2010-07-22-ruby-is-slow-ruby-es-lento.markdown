--- 
layout: post
title: Ruby is Slow - Ruby es lento
wordpress_id: 398
wordpress_url: http://blog.malev.com.ar/?p=398
comments: true
categories: 
- title: Programacion
  slug: programacion
  autoslug: programacion
- title: Ruby
  slug: ruby
  autoslug: ruby
- title: ruby-core
  slug: ruby-core
  autoslug: ruby-core
tags: 
- title: core
  slug: core
  autoslug: core
- title: Ruby
  slug: ruby
  autoslug: ruby
- title: garbage
  slug: garbage
  autoslug: garbage
- title: colector
  slug: colector
  autoslug: colector
---
{{ page.title }}
================
Hace unos días encontre este post de envylabs [1] en que muestran un video con una charla de Joe Damato [2] sobre el **Garbage colector** [3] y el **heap**[4] de Ruby. La conclusión y el pensamiento de muchos desarrolladores de Ruby no es muy alentador y titula este post. Pero en la charla nos dan algunos consejos de como optimizar el manejo de memoria.

<object classid="clsid:D27CDB6E-AE6D-11cf-96B8-444553540000" width="545" height="451" id="viddler_87ae120a"><param name="movie" value="http://www.viddler.com/player/87ae120a/" /><param name="allowScriptAccess" value="always" /><param name="allowFullScreen" value="true" /><embed src="http://www.viddler.com/player/87ae120a/" width="545" height="451" type="application/x-shockwave-flash" allowscriptaccess="always" allowfullscreen="true" name="viddler_87ae120a"></embed></object>

**Presentación:**
[Garbage Collection and the Ruby Heap](http://www.scribd.com/doc/32718051/Garbage-Collection-and-the-Ruby-Heap "View Garbage Collection and the Ruby Heap on Scribd") <object id="doc_878657598300320" name="doc_878657598300320" height="500" width="100%" type="application/x-shockwave-flash" data="http://d1.scribdassets.com/ScribdViewer.swf" style="outline:none;"><param name="movie" value="http://d1.scribdassets.com/ScribdViewer.swf" /><param name="wmode" value="opaque" /> <param name="bgcolor" value="#ffffff" /> <param name="allowFullScreen" value="true" /> <param name="allowScriptAccess" value="always" /> <param name="FlashVars" value="document_id=32718051&access_key=key-1hl4d18vocqmc9ilk9a&page=1&viewMode=slideshow" /> <embed id="doc_878657598300320" name="doc_878657598300320" src="http://d1.scribdassets.com/ScribdViewer.swf?document_id=32718051&access_key=key-1hl4d18vocqmc9ilk9a&page=1&viewMode=slideshow" type="application/x-shockwave-flash" allowscriptaccess="always" allowfullscreen="true" height="500" width="100%" wmode="opaque" bgcolor="#ffffff"></embed> </object>

Si les gusto (como a mi) y quieren seguir leyendo al respecto (cosa que pienso hacer pronto), pueden pasear por aquí [5] y aquí [6].

**Links:**
[1] [http://blog.envylabs.com](http://blog.envylabs.com)
[2] [http://timetobleed.com](http://timetobleed.com)
[3] [http://es.wikipedia.org/wiki/Recolector_de_basura](http://es.wikipedia.org/wiki/Recolector_de_basura)
[4] [http://es.wikipedia.org/wiki/Mont%C3%ADculo_%28inform%C3%A1tica%29](http://es.wikipedia.org/wiki/Mont%C3%ADculo_%28inform%C3%A1tica%29)
[5] [http://blog.pluron.com/2008/01/ruby-on-rails-i.html](http://blog.pluron.com/2008/01/ruby-on-rails-i.html)
[6] [http://www.coffeepowered.net/2009/06/13/fine-tuning-your-garbage-collector/](http://www.coffeepowered.net/2009/06/13/fine-tuning-your-garbage-collector/)
