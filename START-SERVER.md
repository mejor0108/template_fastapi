# Servidores ASIG

> Versión original : https://runebook.dev/es/docs/fastapi/deployment/manually/index

## Ejecutar un servidor manualmente - Uvicorn

Lo principal que necesita para ejecutar una aplicación **FastAPI** en una máquina de servidor remoto es un programa de servidor ASGI como **Uvicorn** .

Hay 3 alternativas principales:

- **Uvicorn** : un servidor ASGI de alto rendimiento.
- **Hypercorn** : un servidor ASGI compatible con HTTP/2 y Trio entre otras características.
- **Daphne** : el servidor ASGI creado para Django Channels.

## Máquina servidor y programa servidor

Hay un pequeño detalle sobre los nombres a tener en cuenta. 💡

La palabra **servidor*** se usa comúnmente para referirse tanto a la computadora remota/en la nube (la máquina física o virtual) como al programa que se ejecuta en esa máquina (por ejemplo, Uvicorn).

Solo tenga eso en cuenta cuando lea **servidor** en general, podría referirse a una de esas dos cosas.

Al referirse a la máquina remota, es común llamarla **servidor** , pero también **máquina** , **VM** (máquina virtual), **nodo** . Todos ellos se refieren a algún tipo de máquina remota, normalmente ejecutando Linux, donde ejecuta programas.
Instale el programa del servidor

Puede instalar un servidor compatible con ASGI con:

### Uvicorn

```bash
# Uvicorn , un servidor ASGI ultrarrápido, basado en uvloop y httptools.
$ pip install "uvicorn[standard]"
```

### Tip

Al agregar el *standard* , Uvicorn instalará y usará algunas dependencias adicionales recomendadas.

Eso incluye *uvloop* , el reemplazo directo de alto rendimiento para *asyncio* , que proporciona un gran aumento de rendimiento de concurrencia.

### Hypercorn
```bash
#Hypercorn , un servidor ASGI también compatible con HTTP/2.
$ pip install hypercorn

```

...o cualquier otro servidor ASGI.

## Ejecute el programa del servidor

Luego puede ejecutar su aplicación de la misma manera que lo hizo en los tutoriales, pero sin la opción **--reload** ,por ejemplo:

Uvicorn
```bash
$ uvicorn main:app --host 0.0.0.0 --port 80
<span style="color: green;">INFO</span>:     Uvicorn running on http://0.0.0.0:80 (Presione CTRL+C para salir)

```

Hypercorn
```bash
$ hypercorn main:app --bind 0.0.0.0:80
Running on 0.0.0.0:8080 over http (CTRL + C to quit)
```

### Warning

Recuerde eliminar la opción *--reload* si la estaba usando. 
La opción *--reload* consume muchos más recursos, es más inestable, etc.
Ayuda mucho durante el desarrollo , pero no deberías usarlo en producción .

## Hipercorn con Trio

*Starlette* y *FastAPI* se basan en **AnyIO** , lo que las hace compatibles con la biblioteca estándar de Python, **asyncio** y **Trio** .
Sin embargo, Uvicorn actualmente solo es compatible con asyncio, y normalmente usa **uvloop** , el reemplazo directo de alto rendimiento para asyncio .
Pero si desea usar **Trio** directamente , puede usar Hypercorn ya que lo admite. ✨

### Instalar Hypercorn con Trio

Primero necesita instalar Hypercorn con soporte Trio:

```bash
$ pip install "hypercorn[trio]"

```

### corre con trío

Luego puede pasar la opción de línea de comando *--worker-class* con el valor *trio* :

```bash
$ hypercorn main:app --worker-class trio
```

Y eso iniciará Hypercorn con su aplicación usando Trio como backend.
Ahora puede usar Trio internamente en su aplicación. O incluso mejor, puede usar AnyIO para mantener su código compatible con Trio y asyncio. 🎉

## Deployment Concepts

Estos ejemplos ejecutan el programa del servidor (por ejemplo, Uvicorn), iniciando un solo proceso , escuchando en todas las direcciones IP ( **0.0.0.0** ) en un puerto predefinido (por ejemplo, **80** ).

Esta es la idea básica. Pero probablemente querrá encargarse de algunas cosas adicionales, como:

- Seguridad - HTTPS
- Corriendo al inicio
- Restarts
- Replicación (la cantidad de procesos en ejecución)
- Memory
- Pasos previos antes de empezar

Te contaré más sobre cada uno de estos conceptos, cómo pensar en ellos y algunos ejemplos concretos con estrategias para manejarlos en los próximos capítulos. 🚀