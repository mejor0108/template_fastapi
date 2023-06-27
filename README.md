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

## Servicio básico

```py
# module
from fastapi import FastAPI
from typing import Union

# objecto aplicación
app = FastAPI()

# Decorator: método GET, path raiz, sin parametro, sin query y sin body.
@app.get("/")
def read_root():        # Define la función que ejecutar
    return {"Hello": "World"}       # retorna un dictionary

# Decorator: método GET, path /items/{Entero}, sin parametro, con query q y sin body.
@app.get("/items/{item_id}")
def read_item(item_id: int, q: Union[str, None] = None):
    return {"item_id": item_id, "q": q}  # retorna un dictionary
```

## Dependencia Opcionales

### Usadas por Pydantic
- email_validator : validador de emails
### Usados por Starlette
- httpx : requerido para el uso de TestClient
- jinja2 : Requerido para usar la configuración por defecto de templates
- python-multipart : Soporte a "parsing de formularios, con request.form
- itsdangerous : Soporte para SessionMiddleware
- pyyaml : Soporte al SchemaGenerator de Starlette
- graphene : Requerido para dar soporte a GraphQApp
- ujson : Requerido para usar UJSONResponse
### Usado FastAPI / Starlette
- uvicorn : Para el servidor que carga y sirve la aplicación
- orjson : Requerido para usar ORJSONResponse

Puedes instalarlos con ```py ip install fastapi[all]```.

## Caracteristicas de FastAPI

FastAPI te provee lo siguiente:
Basado en estándares abiertos
- [OpenAPI](https://github.com/OAI/OpenAPI-Specification) para la creación de APIs, incluyendo declaraciones de path operations, parámetros, body requests, seguridad, etc.
- Documentación automática del modelo de datos con [JSON Schema](https://json-schema.org/) (dado que OpenAPI mismo está basado en JSON Schema).
- Diseñado alrededor de estos estándares después de un estudio meticuloso. En vez de ser una capa añadida a último momento.
- Esto también permite la generación automática de código de cliente para muchos lenguajes.

## OpenAPI

**FastAPI** genera un "schema" con toda tu API usando el estándar para definir APIs, OpenAPI.

**"Schema"**

Un **"schema"** es una definición o descripción de algo. No es el código que la implementa, sino solo una descripción abstracta.

**"Schema" de la API**

En este caso, [OpenAPI](https://github.com/OAI/OpenAPI-Specification) es una especificación que dicta como se debe definir el schema de tu API.
La definición del schema incluye los paths de tu API, los parámetros que podría recibir, etc.

**"Schema" de datos**

El concepto "schema" también se puede referir a la forma de algunos datos, como un contenido en formato JSON.
En ese caso haría referencia a los atributos del JSON, los tipos de datos que tiene, etc.

**OpenAPI y JSON Schema**

OpenAPI define un schema de API para tu API. Ese schema incluye definiciones (o "schemas") de los datos enviados y recibidos por tu API usando **JSON Schema**, el estándar para los schemas de datos en JSON.

**Revisa el openapi.json**

Si sientes curiosidad por saber cómo se ve el schema de OpenAPI en bruto, FastAPI genera automáticamente un (schema) JSON con la descripción de todo tu API.

Lo puedes ver directamente en: [http://127.0.0.1:8000/openapi.json](https://github.com/OAI/OpenAPI-Specification).

Esto te mostrará un JSON que comienza con algo como:

```json
{
    "openapi": "3.0.2",
    "info": {
        "title": "FastAPI",
        "version": "0.1.0"
    },
    "paths": {
        "/items/": {
            "get": {
                "responses": {
                    "200": {
                        "description": "Successful Response",
                        "content": {
                            "application/json": {
...

```

**¿Para qué se usa OpenAPI?**

El schema de OpenAPI es lo que alimenta a los dos sistemas de documentación interactiva incluidos.

También hay docenas de alternativas, todas basadas en OpenAPI. Podrías añadir fácilmente cualquiera de esas alternativas a tu aplicación construida con **FastAPI**.

También podrías usarlo para generar código automáticamente, para los clientes que se comunican con tu API. Por ejemplo, frontend, móvil o aplicaciones de IoT.


## Template FastAPI

### Path y Query ejemplos
```py 
# main.py
from fastapi import FastAPI
from typing import Union

app =  FastAPI()        # Crea la instancia del objeto, nombre app.

@app.get('/')           # decorator: Método GET y path /
async def root():        
    return {"message":"Hello World"}    # contenido que se retorna
                                        # tipos : dict, list, str, int, models Pydantic

@app.get('/items/{item_id}')            # El path contiene un paramentro
async def read_item(item_id: int):      # la función recibe el paramentro en la variable item_id(int)
    return {"item_id": item_id}         # Si se define el tipo(int), se valida que sea el correcto.

@app.get("/items/")                     # Defino un path con query /items/?skip=0&limit=10
async def read_item(skip: int = 0, limit: int = 10): 
    return fake_items_db[skip : skip + limit]

@app.get("/items/{item_id}")            # Defino un path con query /items/{item_id]?skip=0&limit=10
async def read_item(item_id: str, q: Union[str, None] = None):  # Define q como opcianl, encaso de no 
    if q:                                                       # existir es None
        return {"item_id": item_id, "q": q}
    return {"item_id": item_id}

@app.get("/users/{user_id}/items/{item_id}") # Defino un path con multiple parametro {user_id} y {item_id}
async def read_user_item(
    user_id: int, item_id: str, q: Union[str, None] = None, short: bool = False
):                                           # Define queryString opcional y {short} variable global
    item = {"item_id": item_id, "owner_id": user_id}
    if q:
        item.update({"q": q})
    if not short:
        item.update(
            {"description": "This is an amazing item that has a long description"}
        )
    return item

@app.get("/items/{item_id}")                # Defino un path con multiple parametro {item_id} y un
async def read_user_item(item_id: str, needy: str): # query {needy} obligatorio
    item = {"item_id": item_id, "needy": needy}
    return item
```
### Path con restricción
```py
from enum import Enum                   # nos permite la creación de un Enum
from fastapi import FastAPI

class ModelName(str, Enum):
    alexnet = "alexnet"
    resnet = "resnet"
    lenet = "lenet"

app = FastAPI()

@app.get("/models/{model_name}")                # Definimos un path que solo puede recibir string 
async def get_model(model_name: ModelName):     # que coincida con ModelName{alexnet, resnet, lenet}
    if model_name is ModelName.alexnet:
        return {"model_name": model_name, "message": "Deep Learning FTW!"}
    if model_name.value == "lenet":
        return {"model_name": model_name, "message": "LeCNN all the images"}
    return {"model_name": model_name, "message": "Have some residuals"}
```
### Request Body

```py
from fastapi import FastAPI
from pydantic import BaseModel      # import pydantic para definir model

class Item(BaseModel):              # la clase Item hereda del BaseModel
    name: str                       #   {   "name" :            ,
    description: str | None = None  #       "description" :     ,   // opt
    price: float                    #       "price' :           , 
    tax: float | None = None        #       "tax" :                 // opt
                                    #   }

app = FastAPI()

@app.post("/items/")
async def create_item(item: Item):      # la función recibe un objeto Item(BaseModel)
    item_dict = item.dict()             # podemos convertir el objeto a dictionary
    if item.tax:                        # o podemos acceder directamente a los atributos
        price_with_tax = item.price + item.tax
        item_dict.update({"price_with_tax": price_with_tax})
    return item_dict

@app.put("/items/{item_id}")        # Se define un path con parametro y body
async def create_item(item_id: int, item: Item):    # fastApi pude identificar el parametro y el body
    return {"item_id": item_id, **item.dict()}

@app.put("/items/{item_id}")        # Se define un path con parametro, query y body
async def create_item(item_id: int, item: Item, q: str | None = None): 
    result = {"item_id": item_id, **item.dict()}    # {item_id} : path
    if q:                                           # item :  Object Item(BaseModel) Body
        result.update({"q": q})                     # q : query parameters
    return result

```
### Query
#### Validation Query Parameters and string Validation
```py
## Version de fastAPI >= 0.95.1 / python >= 3.10
from typing import Annotated
from fastapi import FastAPI, Query

app = FastAPI()

@app.get("/items/")     # acepta un query del tipo str(opcional) y con un 
                        # tamaño máximo de 50 caracteres
async def read_items(q: Annotated[str | None, Query(max_length=50)] = None):
    results = {"items": [{"item_id": "Foo"}, {"item_id": "Bar"}]}
    if q:
        results.update({"q": q})
    return results

@app.get("/items/")     # define que q es obligatorio y minimo 3 caracteres
async def read_items(q: Annotated[str, Query(min_length=3)]):
    results = {"items": [{"item_id": "Foo"}, {"item_id": "Bar"}]}
    if q:
        results.update({"q": q})
    return results

@app.get("/items/")
async def read_items(
    q: Annotated[
        str | None,
        Query(
            alias="item-query",         # metadata
            title="Query string",       # metadata
            description="Query string for the items to search in the database that have a good match",          # metadata
            min_length=3,               # requisito - validación
            max_length=50,              # requisito - validación
            regex="^fixedquery$",       # requisito - validación
            deprecated=True,            # metada
        ),
    ] = None
):
    results = {"items": [{"item_id": "Foo"}, {"item_id": "Bar"}]}
    if q:
        results.update({"q": q})
    return results

## Query: Validation
## max_length = XX   : largo máximo hasta xx caracteres
## min_length = XX   : largo minimo hasta xx caracteres
## regex = <pattern> : valida que cumpla la expresión regular <pattern>

## Query: Metadata:
## alias:
## title:
## description :
## deprecated : 
```
#### Versión alternativa(old)
```py
# version: fastAPI >= 0.95.1 y python >= 3.10
from fastapi import FastAPI, Query     # Sin usar Annotated

app = FastAPI()

@app.get("/items/")         # con la función se define un valor default None
async def read_items(q: str | None = Query(default=None, max_length=50)):
    results = {"items": [{"item_id": "Foo"}, {"item_id": "Bar"}]}
    if q:
        results.update({"q": q})
    return results
```

### Path Parameters and Numeric Validations

```py
# Versión : fastAPI >= 0.95.1 y python >= 3.10
from typing import Annotated
from fastapi import FastAPI, Path, Query

app = FastAPI()
@app.get("/items/{item_id}")
async def read_items(           # la variable item_id es un path y se define un título
    item_id: Annotated[int, Path(title="The ID of the item to get")],
    q: Annotated[str | None, Query(alias="item-query")] = None,
):
    pass 

```


### Metodos disponibles:
- GET       : Leer datos del servicio
- POST      : Crear datos
- PUT       : Actualizar datos
- DELETE    : Borrar datos 
- OPTIONS   : Recupera los métodos implementados
- HEAD      : Recuperar lso metadatos de los encabezados
- PATCH     : Sobreescribe completamente el recurso o datos
- TRACE     : Solicita que introduzca en la respuesta todos los datos que reciba


## Orden de evaluación PATH

Recordar que la evaluación del path se realiza en orden, por lo cual en path similares:

```py
@app.get("/users/me")           # Si el orden estuviese al revés, asume que /users/me es parte de
@app.get("/users/{user_id}")    # /users/{user_id}

```
