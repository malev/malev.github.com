--- 
layout: post
title: Plugins en python - 1
wordpress_id: 259
wordpress_url: http://blog.malev.com.ar/?p=259
comments: true
categories: 
- title: metaprogramacion
  slug: metaprogramacion
  autoslug: metaprogramacion
- title: Programacion
  slug: programacion
  autoslug: programacion
tags: 
- title: metaprogramming
  slug: metaprogramming
  autoslug: metaprogramming
- title: plugins
  slug: plugins
  autoslug: plugins
- title: Python
  slug: python
  autoslug: python
---
{{ page.title }}
================
Una manera de extender facilmente el alcance o la funcionalidad de una aplicación es mediante el uso de plugins.
En este caso el objetivo es crear una clase PluginBase donde se colocarán todos los métodos genéricos y las variables del sistema que los plugins podrán usar en el futuro. También podría tener un constructor para inicializar una base de datos por ejemplo, o lo que sea.
Conviene que los plugins se instalen en un directorio particular ("./plugins" por ejemplo ^_^), así vamos brindandole privacidad respecto del resto del código del proyecto.
En este caso, cada plugin responderá a una serie de "eventos" conocidos, evento1 y evento2, que serán llamados por el programa principal cuándo sea necesario.

{% highlight python %}
# Clase madre de los plugins, ubicada en algún lugar del código de nuestro programita
class PluginBase(object):
    variables_sistema = 0
    def metodo1_madre(self):
        pass
    def available(self):
        return True
    def syncronized(self):
        return "something"
{% endhighlight %}

Aquí un plugin de ejemplo. Primero debe importar la clase PluginBase desde donde sea que este ubicada y debe contener los métodos eventos conocidos, en este caso metodo1.

{% highlight python %}
# ./plugins/plugin1.py
from core.SuperPrograma import PluginBase
class Plugin(PluginBase):
    def __init__(self):
        self.name = "plugin1"
        self.variable = variable
    def metodo1(parametros):
        print self.name + " ha ejecutado metodo1"
{% endhighlight %}

Método de carga de plugins. En este caso, busca todos los archivos del directorio ./plugins, pero omite los que inician con __ (__init__.py por ejemplo) y los que terminan con pyc (los bytcodes generados). Luego importa cada archivo o plugin encontrado. Y por último instancia cada clase Plugin encontrado y guarda todas las instancias en la lista "objetos".

{% highlight python %}
def loadPlugins():
    objetos = []
    for arch in os.listdir("./plugins"):
        if arch.startswith("_") of arch.endswith("pyc"):
            continue
        mod = __import__("plugins." + arch.split(".")[0])
        mod = getattr(mod, arch.split(".")[0])
        objetos.append(mod.Plugin())
    return objetos
{% endhighlight %}

Llamada. Cuando salta X evento y se necesita llamar a todos los plugins que tengan método1, se puede crear una método que busque que plugins contienen el método1 y los ejecute.

{% highlight python %}
def llamar_metodo1_plugins():
    for ob in objetos:
        if hasattr(ob, "metodo1"):
            ob.metodo1(algun_parametro)
{% endhighlight %}

Por último, esta es UNA forma de usar plugins. No la ÚNICA! Por ejemplo [aquí](http://www.ibm.com/developerworks/aix/library/au-cli_plugins/index.html) que voy a leer en un rato.
**Bibliografía**
En este caso me dedique a plagiar código y luego analizarlo. Saque cosas de [lalita](https://launchpad.net/lalita) y de [failbot](https://launchpad.net/failbot). Además conte con la ayuda de gente del canal de pyar.
