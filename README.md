# template_fastapi

## Descipción

El objetivo de este template, es poder de disponer de información para la implementación o ejemplo de servicio con el framework FASTAIP.

## Requisitos

> Python 3.7 o mayor

> Starlette
> Pydantic

## Instalación

Recomendación, antes instarla el entorno virtual :
```bash
# Instalación de entorno virtual en el directori actual
$> python -m venv .         

```

Instalación de Framework y el servidor ASGI

```bash
# Instalación del framework
$> pip install fastapi    

# instalación del servidor ASIG 
## opcion 1  - Uvicorn
$> pip install uvicorn[standard]

## opcion 2 - hipercorn
$> pip install hypercorn
```

## Ejemplo main.py

Se puede crear un esquelo basico de main con el archivo

[Archivo Main](./example/main-example.py)


O se puede utiliza el archivo con comando async / await

[Archivo Main ASYNC](./example/main-async.py)

## Ejecutar la aplicación

Dependiendo del servidor, se deje ejecutar de la siguiente manera:

> Uvicorn
```bash
$> uvicorn main:app --reload

# uvicorn: aplicación servidor ASIG
# main : hace referencia al archivo python de la aplicación
# app : nombre del objeto aplicación que se encuentra definiddo dentro del
#        archivo main.py
# --reload: Cada vez que se modifica el código se reinicia el servidor
#           [IMPORTANTE] No usar en PROD.

```

> Hypercorn
```bash
$> hypercorn main:app 

# uvicorn: aplicación servidor ASIG
# main : hace referencia al archivo python de la aplicación
# app : nombre del objeto aplicación que se encuentra definiddo dentro del
#        archivo main.py
# --reload: Cada vez que se modifica el código se reinicia el servidor
#           [IMPORTANTE] No usar en PROD.

```

## Documentación interactiva de APIS

Existe dos docuemtnaciones:
> Swagger UI - El cual se accede con la siguiente URL
http://[IP:URL]/docs
> Redoc : - El cual se accede con la siguiente URL
http://[IP:URL]/redoc
