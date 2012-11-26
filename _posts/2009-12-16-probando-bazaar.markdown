--- 
layout: post
title: Probando Bazaar
wordpress_id: 70
wordpress_url: http://blog.malev.com.ar/?p=70
comments: true
categories: 
- title: cvs
  slug: cvs
  autoslug: cvs
- title: Programacion
  slug: programacion
  autoslug: programacion
tags: 
- title: bazaar
  slug: bazaar
  autoslug: bazaar
- title: bzr
  slug: bzr
  autoslug: bzr
- title: cvs
  slug: cvs
  autoslug: cvs
---
{{ page.title }}
================
[
![bazaar-logo](/images/posts/2009/12/bazaar-logo1.png "bazaar-logo")
](http://bazaar.canonical.com)Según la wikipedia: Es un sistema de control de revisiones distribuido ([distributed revision control](http://en.wikipedia.org/wiki/Distributed_revision_control)) auspiciado por Canonical Ltd., diseñado para facilitar la contribución con proyectos de software libres o abiertos.[1]
Según Mark Shuttleworth, "Bazaar está diseñado para equipos globales de desarrolladores colaborativos" y tiene la ventaja sobre los sistemas similares centralizados como Subversion o CVS en que no requiere un servidor dedicado. [2]
Bazaar funciona con un solo comando: **bzr**, todo el resto del manejo se hace a través de parámetros:
<blockquote>bzr init
bzr add
bzr commit -m "Primer commit"
bzr add .bzrignore  # Contiene los archivos a ignorar
bzr diff
bzr log
bzr remove -v archivo_a_remover.chau
bzr pull

**“Branchear un proyecto ya existente”**
bzr branch http://www.example.com/myproject

bzr help
bzr help commands</blockquote>
Se puede migrar desde otros sistemas como GIT, Subversion y Mercurial. Yo no lo he probado y no lo probaría a menos que me sea muy necesario ;) También cuenta con un Explorador visual [3][4][5]. Y se lo puede usar con [Launchpad,](https://launchpad.net/) [GNU Savannah,](http://savannah.gnu.org/) [Sourceforge](http://sourceforge.net/).
Y algo muy interesante, hay un plugin para Gedit: https://launchpad.net/bzr-gedit/

[1] [http://en.wikipedia.org/wiki/Bazaar_%28software%29](http://en.wikipedia.org/wiki/Bazaar_%28software%29)
[2] [http://www.vivalinux.com.ar/soft/bazaar-1.0](http://www.vivalinux.com.ar/soft/bazaar-1.0)
[3] [http://wiki.bazaar.canonical.com/Documentation](http://wiki.bazaar.canonical.com/Documentation)
[4] [http://doc.bazaar.canonical.com/bzr.2.0/en/tutorials/using_bazaar_with_launchpad.html](http://doc.bazaar.canonical.com/bzr.2.0/en/tutorials/using_bazaar_with_launchpad.html)
[5] [http://doc.bazaar.canonical.com/explorer/en/visual-tour-gnome.html](http://doc.bazaar.canonical.com/explorer/en/visual-tour-gnome.html)
