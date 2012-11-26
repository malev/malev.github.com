--- 
layout: post
title: "Stipple - Sincronizando archivos de configuraci\xC3\xB3n"
wordpress_id: 379
wordpress_url: http://blog.malev.com.ar/?p=379
comments: true
categories: 
- title: couchdb
  slug: couchdb
  autoslug: couchdb
- title: Python
  slug: python
  autoslug: python
- title: tools
  slug: tools
  autoslug: tools
tags: 
- title: bzr
  slug: bzr
  autoslug: bzr
- title: launchpad
  slug: launchpad
  autoslug: launchpad
- title: Python
  slug: python
  autoslug: python
- title: stipple
  slug: stipple
  autoslug: stipple
---
{{ page.title }}
================
Desde hace algunos meses estoy participando del proyecto Stipple junto a [Duane Hinnen](http://okiebuntu.homelinux.com/), miembre del [Ubuntu Beginners Team](https://wiki.ubuntu.com/BeginnersTeam).
El proyecto es una aplicación encargada de sincronizar los archivos de configuración. Esos archivos ocultos que se guardan en la home, como por ejemplo: **.bash**, **.vimrc**, **.emacs**, etc. También cuenta con la funcionalidad de sincronizar el listado de paquetes instalados y configuraciones más complejas, como todos los scripts de Vim.
Todo el código fuente está en launchpad:
https://launchpad.net/stipple
La aplicación usa [couchDB](http://couchdb.apache.org/), que ya es casi un estandar en Ubuntu y permite la sincronización en varias computadoras gracias a [Ubuntuone](https://one.ubuntu.com/). Lo hemos programado de manera que acepte la instalación de nuevos plugis, para que cada uno agregue algún archivo o directorio que quiera sincronizar automaticamente. Esa última parte esta bastante bien trabajada y hay algunos métodos disponibles para hacer esta tarea de manera muy simple.
Por lo pronto no esta empaquetada, pero la idea es empaquetarla lo más pronto posible. También queremos usar para probar otro sistema de sincronización para los usuarios que no usan ubuntu :)
Si alguno quiere chusmear su código o probarla, estan más que invitados!!
