--- 
layout: post
title: Tutorial de Bazaar
wordpress_id: 196
wordpress_url: http://blog.malev.com.ar/?p=196
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
- title: cvs
  slug: cvs
  autoslug: cvs
- title: Programacion
  slug: programacion
  autoslug: programacion
- title: Python
  slug: python
  autoslug: python
---
{{ page.title }}
================
Hace algunas semanas hable sobre Bazaar [1] en [este](http://blog.malev.com.ar/2009/12/probando-bazaar/) post. Lo he probado durante algún tiempo y me ha parecido muy simple! por supuesto, yo trabajo solo y casi nunca tengo conflictos con... migo mismo, pero en fin. Aquí presento un pequeño tutorial al respecto:
[
![](/images/posts/2010/03/Bazaar1-300x125.png "Bazaar1")
](http://174.132.180.130/~nahuisar/blog/wp-content/uploads/2010/03/Bazaar11.png)
Primero básico e imprescindible: **Iniciar un proyecto**. Podemos empezar uno nosotros mismos o clonar uno de algún **repositorio**. (Es decir copiarnos el de alguien para modificarlo nosotros).
<blockquote>1) Creamos nosotros un nuevo proyecto
mkdir proyecto_nuevo
cd proyecto_nuevo
bzr init
2) hacemos **branch** de un proyecto:
bzr branch lp:lalita</blockquote>
Configuramos nuestro repositorio: Principalmente, se seleccionan que archivos no queremos formen parte del repositorio: archivos propios del IDE, configuraciones exclusivas de nuestro sistemas, la base de datos que estemos usando, backups del sistema, *.pyc, etc. Si bien ya hay un archivo que se encarga de esto en Bazaar, también podemos crear el nuestro **.bzrignore**. Ojo! debe estar dentro de un commit (ver más adelante)
<blockquote>*.o
*~
*.tmp
*.py[co]
bzr ignore
bzr ignore --old-default-rules</blockquote>
Listo! ya podemos empezar a trabajar en nuestro proyecto. Una vez que tengamos algunos cambios y estemos seguros de que funcionan deberíamos hacer **commit**. En el caso de que hayamos creado o borrado algunos archivos a drede antes deberíamos avisarle a **Bazaar**:
<blockquote>bzr add nombre_del_archivo
bzr add . #agregamos todos los archivos nuevos
bzr remove nombre_del_archivo
bzr commit -m "esto es un commit"
bzr commit -m "solo comiteamos un archivo" file_name
bzr commit --author "Patricio Rey <prey@losredondos.com>"</blockquote>
Revisemos como vamos:
<blockquote>bzr status # nos dice si hicimos cambios desde el último commit
bzr diff # nos muestra todos los cambios desde el último commit
bzr diff file.py #exclusivo para un archivo
bzr diff --diff-options --side-by-side foo.py #a probar!
bzr log # nos muestra un historial de commits
bzr log foo.py  # el historial de un solo archivo
bzr cat -r X file #la versión X del archivo file
bzr viz # Herramienta gráfica!</blockquote>
¡Errores! Supongamos que hemos hecho algunos cambios y ... no nos gusta ninguno. Podemos hacer un:
<blockquote>bzr revert # y eliminar los cambios hasta el último commit
bzr revert archivo # elimina los cambios de un archivo
bzr uncommit # elimina el último commit
bzr uncommit -r -3 # vuelve 3 commits para atrás </blockquote>
A veces, tenemos una versión funcional, pero le queremos agregar algunas opciones nuevas. A riesgo de no "romper" el programa que ya está andando, lo que hacemos es crear un **branch** y luego, cuando nos aseguramos que las nueva features funcionan genial, hacemos un **merge** entre las dos versiones:
<blockquote>#no ubicamos un directorio antes en el árbol:
pwd  -> /home/malev/code/lalita
cd..
bzr branch lalita nuevo_branch
# jugamos en nuevo_branch y cuando nos convence
# en el directorio de lalita hacemos
pwd -> /home/malev/code/lalita
bzr merge ../nuevo_branch
# y si no hay conflictos, YA ESTÁ!</blockquote>
Preparando un release! Esta opción nos permite copiar todos los archivos y directorios de un branch y empaquetarlos en un archivo o directorio. También es posible taggear o etiquetar un conjunto de archivos con un número de versión.
<blockquote>bzr export ../releases/my-stuff-1.5.tar.gz
bzr tag version-1-5
bzr diff -r tag:version-1-5
bzr tag 2.0-beta-4 --delete #si nos confundimos :)
</blockquote>
Todo este tutorial ha sido extraído de [2]. Aunque también recibí ayuda de Facu, Dermi y Gonzalo, todos del **canal de IRC de PyAr** [3]
También existe una aplicación visual [4], pero todavía no descubrí como instalarla :(
Y por último, he encontrado otro tutorial en español que parece interesante aquí en [5]
[1] [http://bazaar.canonical.com](http://bazaar.canonical.com)
[2] [http://doc.bazaar.canonical.com](http://doc.bazaar.canonical.com)
[3] [http://python.org.ar](http://python.org.ar)
[4] [http://doc.bazaar.canonical.com/explorer/en/visual-tour-gnome.html](http://doc.bazaar.canonical.com/explorer/en/visual-tour-gnome.html)
[5] [http://palangano.com.ar/tag/bazaar/](http://palangano.com.ar/tag/bazaar/)
